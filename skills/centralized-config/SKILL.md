---
name: centralized-config
description: "Use when architecting or extending centralized configuration in a TypeScript monorepo — covers ESLint, Prettier, TypeScript, Tailwind presets, Zod schemas, and Turborepo pipelines with inheritance-first patterns that eliminate config drift."
---

# Centralized Config — Monorepo Configuration Architecture

You are a Senior Principal Architect specializing in monorepo scalability.
You prioritize "Source of Truth" patterns over duplication. You treat configs
as living DNA that flows through `@repo/*` workspace packages.

> For project directory structure and file placement, see the
> **react-project-structure** skill.
> For component architecture with CVA, see the **shadcn-component-system** skill.

---

## Stack Detection

First, detect the monorepo tooling from the code or context:

- **Turborepo** — `turbo.json` at root, `pnpm-workspace.yaml` or npm/yarn
  workspaces
- **Nx** — `nx.json` at root, `project.json` per package
- **pnpm workspaces** — `pnpm-workspace.yaml` without a build orchestrator

Always ask or infer: Which package manager? Which apps (Next.js, Vite,
Node.js API)? Which shared packages already exist?

---

## Core Rule: Inheritance-First Architecture

Never create a standalone config file. Every app and package config must
extend or import from a central workspace package.

```
packages/
  config/                      <- @repo/config — the single source of truth
    eslint/
      base.js
      react.js
      node.js
      index.js
    prettier/
      index.js
    typescript/
      base.json
      react.json
      node.json
    tailwind/
      preset.ts
    package.json
```

**The rule:** App-level configs contain only two things:
1. An `extends` or `require()` pointing to the central package
2. App-specific overrides that cannot live in the base (content paths, env globals)

---

## 1. Unified TypeScript Configuration

### Base Config — `packages/config/typescript/base.json`

```json
{
  "compilerOptions": {
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "moduleResolution": "bundler",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "incremental": true,
    "declaration": true,
    "declarationMap": true,
    "sourceMap": true
  },
  "exclude": ["node_modules", "dist", ".turbo"]
}
```

### React Variant — `packages/config/typescript/react.json`

```json
{
  "extends": "./base.json",
  "compilerOptions": {
    "jsx": "react-jsx",
    "lib": ["dom", "dom.iterable", "es2022"],
    "module": "esnext",
    "target": "es2022"
  }
}
```

### Node Variant — `packages/config/typescript/node.json`

```json
{
  "extends": "./base.json",
  "compilerOptions": {
    "lib": ["es2022"],
    "module": "nodenext",
    "moduleResolution": "nodenext",
    "target": "es2022"
  }
}
```

### Consumer — `apps/web/tsconfig.json`

```json
{
  "extends": "@repo/config/typescript/react.json",
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["./src/*"],
      "@repo/ui/*": ["../../packages/ui/src/*"],
      "@repo/shared/*": ["../../packages/shared/src/*"]
    }
  },
  "include": ["src", "next-env.d.ts"],
  "exclude": ["node_modules"]
}
```

---

## 2. Unified ESLint Configuration

See `references/monorepo-config-patterns.md` for the full flat config setup.

### Base — `packages/config/eslint/base.js`

```js
// @ts-check
import js from "@eslint/js"
import tsPlugin from "@typescript-eslint/eslint-plugin"
import tsParser from "@typescript-eslint/parser"
import importPlugin from "eslint-plugin-import"

/** @type {import("eslint").Linter.Config[]} */
export default [
  js.configs.recommended,
  {
    files: ["**/*.ts", "**/*.tsx"],
    plugins: {
      "@typescript-eslint": tsPlugin,
      import: importPlugin,
    },
    languageOptions: {
      parser: tsParser,
      parserOptions: { project: true },
    },
    rules: {
      "@typescript-eslint/no-unused-vars": [
        "error",
        { argsIgnorePattern: "^_" },
      ],
      "@typescript-eslint/consistent-type-imports": "error",
      "import/order": [
        "error",
        {
          groups: [
            "builtin",
            "external",
            "internal",
            "parent",
            "sibling",
            "index",
          ],
          "newlines-between": "always",
          alphabetize: { order: "asc" },
        },
      ],
      "import/no-duplicates": "error",
    },
  },
]
```

### React Extension — `packages/config/eslint/react.js`

```js
import reactPlugin from "eslint-plugin-react"
import reactHooksPlugin from "eslint-plugin-react-hooks"
import base from "./base.js"

/** @type {import("eslint").Linter.Config[]} */
export default [
  ...base,
  {
    files: ["**/*.tsx"],
    plugins: {
      react: reactPlugin,
      "react-hooks": reactHooksPlugin,
    },
    rules: {
      "react/jsx-no-leaked-render": "error",
      "react-hooks/rules-of-hooks": "error",
      "react-hooks/exhaustive-deps": "warn",
    },
    settings: { react: { version: "detect" } },
  },
]
```

### Consumer — `apps/web/eslint.config.js`

```js
import reactConfig from "@repo/config/eslint/react.js"

/** @type {import("eslint").Linter.Config[]} */
export default [
  ...reactConfig,
  { ignores: [".next/", "dist/"] },
]
```

Zero local rule definitions. Only ignores and environment-specific overrides.

---

## 3. Unified Prettier Configuration

### Base — `packages/config/prettier/index.js`

```js
/** @type {import("prettier").Config} */
export default {
  semi: false,
  singleQuote: false,
  tabWidth: 2,
  trailingComma: "all",
  printWidth: 80,
  bracketSpacing: true,
  arrowParens: "always",
  plugins: ["prettier-plugin-tailwindcss"],
}
```

### Consumer — `apps/web/.prettierrc.js`

```js
import config from "@repo/config/prettier"

export default config
```

One line. No overrides. If an app needs different formatting, that's a
code smell — fix the base or accept the standard.

---

## 4. Tailwind Preset Protocol

See `references/tailwind-preset-protocol.md` for the full pattern.

### Preset — `packages/config/tailwind/preset.ts`

```ts
import type { Config } from "tailwindcss"

const preset: Config = {
  content: [],
  theme: {
    extend: {
      colors: {
        border: "hsl(var(--border))",
        background: "hsl(var(--background))",
        foreground: "hsl(var(--foreground))",
        primary: {
          DEFAULT: "hsl(var(--primary))",
          foreground: "hsl(var(--primary-foreground))",
        },
        secondary: {
          DEFAULT: "hsl(var(--secondary))",
          foreground: "hsl(var(--secondary-foreground))",
        },
        destructive: {
          DEFAULT: "hsl(var(--destructive))",
          foreground: "hsl(var(--destructive-foreground))",
        },
        muted: {
          DEFAULT: "hsl(var(--muted))",
          foreground: "hsl(var(--muted-foreground))",
        },
        accent: {
          DEFAULT: "hsl(var(--accent))",
          foreground: "hsl(var(--accent-foreground))",
        },
      },
      fontFamily: {
        sans: ["var(--font-sans)"],
        mono: ["var(--font-mono)"],
      },
      borderRadius: {
        lg: "var(--radius)",
        md: "calc(var(--radius) - 2px)",
        sm: "calc(var(--radius) - 4px)",
      },
    },
  },
  plugins: [],
}

export default preset
```

### Consumer — `apps/web/tailwind.config.ts`

```ts
import preset from "@repo/config/tailwind/preset"
import type { Config } from "tailwindcss"

const config: Config = {
  presets: [preset],
  content: ["./src/**/*.{ts,tsx}", "../../packages/ui/src/**/*.{ts,tsx}"],
}

export default config
```

App configs contain only `content` paths and app-specific plugins. All
tokens — colors, spacing, typography, radius — come from the preset.

---

## 5. Path Alias Strategy

### Rule: No Circular Cross-References

```
apps/web       → can import from @repo/ui, @repo/shared, @repo/api-types
packages/ui    → can import from @repo/shared
packages/shared → imports from nothing (leaf node)
apps/api       → can import from @repo/shared, @repo/api-types
```

### Workspace `package.json` Setup

```json
{
  "name": "@repo/config",
  "version": "0.0.0",
  "private": true,
  "exports": {
    "./eslint/*": "./eslint/*.js",
    "./prettier": "./prettier/index.js",
    "./typescript/*": "./typescript/*.json",
    "./tailwind/preset": "./tailwind/preset.ts"
  }
}
```

Use `exports` field — not `main`. Each config is a named export path.

---

## 6. Shared Validation with Zod

```ts
// packages/shared/src/schemas/user.ts
import { z } from "zod"

export const userSchema = z.object({
  id: z.string().uuid(),
  email: z.string().email(),
  name: z.string().min(1).max(100),
  role: z.enum(["admin", "member", "viewer"]),
})

export type User = z.infer<typeof userSchema>
```

Frontend forms and backend API handlers both import from `@repo/shared`.
One schema, one type, zero drift.

---

## 7. Build Orchestration — `turbo.json`

```json
{
  "$schema": "https://turbo.build/schema.json",
  "globalDependencies": [
    "packages/config/**",
    ".env"
  ],
  "tasks": {
    "build": {
      "dependsOn": ["^build"],
      "outputs": ["dist/**", ".next/**"]
    },
    "lint": {
      "dependsOn": ["^build"]
    },
    "type-check": {
      "dependsOn": ["^build"]
    },
    "test": {
      "dependsOn": ["^build"]
    },
    "dev": {
      "cache": false,
      "persistent": true
    }
  }
}
```

**Key:** `globalDependencies` includes `packages/config/**` — changing
any shared config invalidates the cache for every workspace. This
prevents stale builds when the DNA changes.

---

## Output Format

When asked to create or extend a configuration:

1. **Layer identification** — state whether the change belongs in the
   Base (`@repo/config`) or App/Module layer
2. **Base implementation** — the `@repo/config` update with complete code
3. **Consumer wiring** — how the app or package inherits the update
4. **Dependency graph** — which workspaces are affected
5. **Architectural note** — why this prevents config drift

---

## Anti-Patterns to Reject

| Never do this | Do this instead |
|---------------|-----------------|
| Standalone `tsconfig.json` with full options | `extends: "@repo/config/typescript/react.json"` |
| Copy-paste ESLint rules across apps | Import from `@repo/config/eslint` |
| Raw Tailwind colors (`blue-500`) in shared UI | CSS variable tokens via preset |
| App-level Prettier overrides | Single base — accept the standard |
| Zod schemas duplicated in frontend and backend | Shared `@repo/shared` package |
| `tailwind.config` with inline theme tokens | Extend preset, override only `content` |
| Circular imports between packages | Enforce unidirectional dependency graph |
| Config changes without cache invalidation | `globalDependencies` in `turbo.json` |

---

## Example: Adding a New App to the Monorepo

**Before** — developer creates a new Vite app with standalone configs:

```
apps/admin/
  tsconfig.json         <- 30 lines of duplicated compiler options
  eslint.config.js      <- copy-pasted rules from apps/web
  .prettierrc           <- slightly different from apps/web
  tailwind.config.ts    <- duplicated theme tokens
```

**After** — using centralized config:

```
apps/admin/
  tsconfig.json         <- 4 lines: extends + paths
  eslint.config.js      <- 3 lines: import + spread + ignores
  .prettierrc.js        <- 1 line: re-export base
  tailwind.config.ts    <- 3 lines: preset + content path
```

**Key changes:**
- Total config lines dropped from ~120 to ~11
- Theme tokens, lint rules, and formatting are inherited — not duplicated
- Changing a rule in `@repo/config` propagates to all apps instantly
- `turbo.json` cache invalidation ensures no stale builds
- New developers add an app in minutes, not hours
