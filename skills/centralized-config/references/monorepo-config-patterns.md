# Monorepo Config Patterns

Deep-dive into ESLint, TypeScript, and Prettier centralization patterns
for workspace-based monorepos.

---

## ESLint Flat Config — Full Setup

ESLint flat config (`eslint.config.js`) is the standard. Legacy
`.eslintrc` files are deprecated.

### Package Structure

```
packages/config/eslint/
  base.js       <- TypeScript + import rules
  react.js      <- extends base + React/JSX rules
  node.js       <- extends base + Node.js rules
  next.js       <- extends react + Next.js rules
  index.js      <- barrel export
```

### Base Config — Complete

```js
// packages/config/eslint/base.js
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
      parserOptions: {
        project: true,
      },
    },
    rules: {
      // TypeScript strict
      "@typescript-eslint/no-unused-vars": [
        "error",
        { argsIgnorePattern: "^_", varsIgnorePattern: "^_" },
      ],
      "@typescript-eslint/consistent-type-imports": [
        "error",
        { prefer: "type-imports", fixStyle: "inline-type-imports" },
      ],
      "@typescript-eslint/no-explicit-any": "warn",
      "@typescript-eslint/no-floating-promises": "error",
      "@typescript-eslint/await-thenable": "error",

      // Import hygiene
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
          alphabetize: { order: "asc", caseInsensitive: true },
        },
      ],
      "import/no-duplicates": "error",
      "import/no-cycle": ["error", { maxDepth: 3 }],
      "no-console": ["warn", { allow: ["warn", "error"] }],
    },
  },
  {
    ignores: [
      "node_modules/",
      "dist/",
      ".turbo/",
      "coverage/",
      "*.config.js",
      "*.config.ts",
    ],
  },
]
```

### React Extension — Complete

```js
// packages/config/eslint/react.js
import jsxA11y from "eslint-plugin-jsx-a11y"
import reactPlugin from "eslint-plugin-react"
import reactHooksPlugin from "eslint-plugin-react-hooks"
import base from "./base.js"

/** @type {import("eslint").Linter.Config[]} */
export default [
  ...base,
  {
    files: ["**/*.tsx", "**/*.jsx"],
    plugins: {
      react: reactPlugin,
      "react-hooks": reactHooksPlugin,
      "jsx-a11y": jsxA11y,
    },
    rules: {
      // React
      "react/jsx-no-leaked-render": "error",
      "react/self-closing-comp": "error",
      "react/jsx-boolean-value": ["error", "never"],
      "react/jsx-curly-brace-presence": [
        "error",
        { props: "never", children: "never" },
      ],

      // Hooks
      "react-hooks/rules-of-hooks": "error",
      "react-hooks/exhaustive-deps": "warn",

      // Accessibility
      "jsx-a11y/alt-text": "error",
      "jsx-a11y/anchor-is-valid": "error",
      "jsx-a11y/click-events-have-key-events": "error",
      "jsx-a11y/no-noninteractive-element-interactions": "warn",
    },
    settings: {
      react: { version: "detect" },
    },
  },
]
```

### Next.js Extension — Complete

```js
// packages/config/eslint/next.js
import nextPlugin from "@next/eslint-plugin-next"
import reactConfig from "./react.js"

/** @type {import("eslint").Linter.Config[]} */
export default [
  ...reactConfig,
  {
    plugins: { "@next/next": nextPlugin },
    rules: {
      "@next/next/no-html-link-for-pages": "error",
      "@next/next/no-img-element": "error",
    },
  },
]
```

### Node.js Extension — Complete

```js
// packages/config/eslint/node.js
import nodePlugin from "eslint-plugin-n"
import base from "./base.js"

/** @type {import("eslint").Linter.Config[]} */
export default [
  ...base,
  {
    files: ["**/*.ts"],
    plugins: { n: nodePlugin },
    rules: {
      "n/no-process-exit": "error",
      "n/no-unsupported-features/node-builtins": "error",
    },
  },
]
```

### Barrel Export

```js
// packages/config/eslint/index.js
export { default as base } from "./base.js"
export { default as react } from "./react.js"
export { default as next } from "./next.js"
export { default as node } from "./node.js"
```

---

## TypeScript Config Inheritance

### Inheritance Chain

```
tsconfig.base.json          <- strict compiler options, shared excludes
  ├── tsconfig.react.json   <- jsx, dom libs, esnext module
  ├── tsconfig.node.json    <- nodenext module resolution
  └── tsconfig.test.json    <- vitest globals, relaxed strict
```

### Path Aliases — Monorepo Pattern

Path aliases must match `package.json` workspace names:

```json
{
  "compilerOptions": {
    "paths": {
      "@repo/ui/*": ["../../packages/ui/src/*"],
      "@repo/shared/*": ["../../packages/shared/src/*"],
      "@repo/api-types": ["../../packages/api-types/src/index.ts"]
    }
  }
}
```

**Rule:** Every path alias maps to a workspace `src/` directory. Never
alias to `dist/` — that creates build-order coupling.

### Per-Package `tsconfig.json`

Every package in the monorepo gets its own `tsconfig.json` that extends
the appropriate base:

```json
{
  "extends": "@repo/config/typescript/react.json",
  "compilerOptions": {
    "outDir": "dist",
    "rootDir": "src"
  },
  "include": ["src"]
}
```

**Rule:** Only `outDir`, `rootDir`, `include`, and `paths` are allowed
in consumer configs. Everything else comes from the base.

---

## Prettier — Zero-Override Policy

Prettier is the one config where overrides are a code smell. The entire
monorepo uses one format.

### The Only Acceptable Overrides

```js
/** @type {import("prettier").Config} */
export default {
  ...baseConfig,
  // ONLY these are acceptable per-app overrides:
  overrides: [
    {
      files: "*.md",
      options: { proseWrap: "always" },
    },
    {
      files: "*.json",
      options: { tabWidth: 2 },
    },
  ],
}
```

If someone wants different semicolons or quotes in one app — the answer
is no. Consistency across the monorepo is non-negotiable.

---

## Config Package — `package.json`

```json
{
  "name": "@repo/config",
  "version": "0.0.0",
  "private": true,
  "type": "module",
  "exports": {
    "./eslint/base": "./eslint/base.js",
    "./eslint/react": "./eslint/react.js",
    "./eslint/next": "./eslint/next.js",
    "./eslint/node": "./eslint/node.js",
    "./prettier": "./prettier/index.js",
    "./typescript/*": "./typescript/*.json",
    "./tailwind/preset": "./tailwind/preset.ts"
  },
  "devDependencies": {
    "@eslint/js": "^9.0.0",
    "@next/eslint-plugin-next": "^15.0.0",
    "@typescript-eslint/eslint-plugin": "^8.0.0",
    "@typescript-eslint/parser": "^8.0.0",
    "eslint": "^9.0.0",
    "eslint-plugin-import": "^2.31.0",
    "eslint-plugin-jsx-a11y": "^6.10.0",
    "eslint-plugin-n": "^17.0.0",
    "eslint-plugin-react": "^7.37.0",
    "eslint-plugin-react-hooks": "^5.0.0",
    "prettier": "^3.4.0",
    "prettier-plugin-tailwindcss": "^0.6.0",
    "typescript": "^5.7.0"
  }
}
```

**Key:** Dependencies live in the config package. Consumer apps do not
install ESLint plugins directly — they get them transitively.

---

## Vitest Config Centralization

### Base — `packages/config/vitest/base.ts`

```ts
import { defineConfig } from "vitest/config"

export default defineConfig({
  test: {
    globals: true,
    environment: "node",
    coverage: {
      provider: "v8",
      reporter: ["text", "lcov"],
      exclude: ["node_modules/", "dist/", "**/*.d.ts", "**/*.config.*"],
    },
    include: ["src/**/*.test.ts", "src/**/*.test.tsx"],
  },
})
```

### React Variant — `packages/config/vitest/react.ts`

```ts
import { defineConfig, mergeConfig } from "vitest/config"
import base from "./base.js"

export default mergeConfig(
  base,
  defineConfig({
    test: {
      environment: "jsdom",
      setupFiles: ["@testing-library/jest-dom/vitest"],
    },
  }),
)
```

### Consumer — `apps/web/vitest.config.ts`

```ts
import { defineConfig, mergeConfig } from "vitest/config"
import reactConfig from "@repo/config/vitest/react"

export default mergeConfig(reactConfig, defineConfig({
  test: {
    alias: {
      "@/": new URL("./src/", import.meta.url).pathname,
    },
  },
}))
```

---

## Dependency Direction Rules

The monorepo dependency graph must be a DAG (Directed Acyclic Graph).

```
Layer 0 (leaf):   @repo/config, @repo/shared
Layer 1:          @repo/ui (depends on shared)
Layer 2:          @repo/feature-* (depends on ui, shared)
Layer 3 (root):   apps/* (depends on features, ui, shared)
```

**Rules:**
1. Lower layers never import from higher layers
2. Same-layer imports are allowed only within the same package
3. `@repo/config` is special — it's consumed via `extends`, not `import`
4. Use `import/no-cycle` in ESLint to catch violations automatically
