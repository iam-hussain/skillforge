# Tailwind Preset Protocol

Centralized design tokens and theming for monorepo UI packages using
Tailwind CSS presets and CSS custom properties.

---

## Why Presets Over Shared Config

| Approach | Problem |
|----------|---------|
| Copy-paste `theme` across apps | Tokens drift within days |
| Shared `tailwind.config.ts` file | Content paths break across workspaces |
| **Tailwind preset** | Tokens centralized, content paths per-app |

Presets solve the monorepo problem: shared tokens, independent content scanning.

---

## Preset Architecture

```
packages/config/tailwind/
  preset.ts           <- design tokens (colors, spacing, typography, radius)
  plugins/
    animations.ts     <- shared animation utilities
    typography.ts     <- prose configuration
```

### The Preset — Complete

```ts
// packages/config/tailwind/preset.ts
import type { Config } from "tailwindcss"

const preset: Config = {
  content: [], // intentionally empty — apps define their own content paths
  theme: {
    extend: {
      // Semantic colors via CSS variables
      colors: {
        border: "hsl(var(--border))",
        input: "hsl(var(--input))",
        ring: "hsl(var(--ring))",
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
        card: {
          DEFAULT: "hsl(var(--card))",
          foreground: "hsl(var(--card-foreground))",
        },
        popover: {
          DEFAULT: "hsl(var(--popover))",
          foreground: "hsl(var(--popover-foreground))",
        },
      },

      // Spacing scale extensions
      spacing: {
        "4.5": "1.125rem",
        "13": "3.25rem",
        "15": "3.75rem",
        "128": "32rem",
        "144": "36rem",
      },

      // Typography
      fontFamily: {
        sans: ["var(--font-sans)", "system-ui", "sans-serif"],
        mono: ["var(--font-mono)", "monospace"],
      },
      fontSize: {
        "2xs": ["0.625rem", { lineHeight: "0.875rem" }],
      },

      // Border radius scale
      borderRadius: {
        lg: "var(--radius)",
        md: "calc(var(--radius) - 2px)",
        sm: "calc(var(--radius) - 4px)",
      },

      // Shared keyframes
      keyframes: {
        "accordion-down": {
          from: { height: "0" },
          to: { height: "var(--radix-accordion-content-height)" },
        },
        "accordion-up": {
          from: { height: "var(--radix-accordion-content-height)" },
          to: { height: "0" },
        },
        "fade-in": {
          from: { opacity: "0" },
          to: { opacity: "1" },
        },
        "slide-in-from-top": {
          from: { transform: "translateY(-100%)" },
          to: { transform: "translateY(0)" },
        },
      },
      animation: {
        "accordion-down": "accordion-down 0.2s ease-out",
        "accordion-up": "accordion-up 0.2s ease-out",
        "fade-in": "fade-in 0.15s ease-in",
        "slide-in-from-top": "slide-in-from-top 0.2s ease-out",
      },
    },
  },
  plugins: [],
}

export default preset
```

---

## CSS Variables — Theme Layer

Each consuming app defines CSS variables in its global stylesheet.
The preset maps Tailwind classes to these variables.

### Light Theme — `apps/web/src/styles/globals.css`

```css
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer base {
  :root {
    --background: 0 0% 100%;
    --foreground: 222.2 84% 4.9%;
    --card: 0 0% 100%;
    --card-foreground: 222.2 84% 4.9%;
    --popover: 0 0% 100%;
    --popover-foreground: 222.2 84% 4.9%;
    --primary: 222.2 47.4% 11.2%;
    --primary-foreground: 210 40% 98%;
    --secondary: 210 40% 96.1%;
    --secondary-foreground: 222.2 47.4% 11.2%;
    --muted: 210 40% 96.1%;
    --muted-foreground: 215.4 16.3% 46.9%;
    --accent: 210 40% 96.1%;
    --accent-foreground: 222.2 47.4% 11.2%;
    --destructive: 0 84.2% 60.2%;
    --destructive-foreground: 210 40% 98%;
    --border: 214.3 31.8% 91.4%;
    --input: 214.3 31.8% 91.4%;
    --ring: 222.2 84% 4.9%;
    --radius: 0.5rem;
    --font-sans: "Inter", system-ui, sans-serif;
    --font-mono: "JetBrains Mono", monospace;
  }

  .dark {
    --background: 222.2 84% 4.9%;
    --foreground: 210 40% 98%;
    --card: 222.2 84% 4.9%;
    --card-foreground: 210 40% 98%;
    --popover: 222.2 84% 4.9%;
    --popover-foreground: 210 40% 98%;
    --primary: 210 40% 98%;
    --primary-foreground: 222.2 47.4% 11.2%;
    --secondary: 217.2 32.6% 17.5%;
    --secondary-foreground: 210 40% 98%;
    --muted: 217.2 32.6% 17.5%;
    --muted-foreground: 215 20.2% 65.1%;
    --accent: 217.2 32.6% 17.5%;
    --accent-foreground: 210 40% 98%;
    --destructive: 0 62.8% 30.6%;
    --destructive-foreground: 210 40% 98%;
    --border: 217.2 32.6% 17.5%;
    --input: 217.2 32.6% 17.5%;
    --ring: 212.7 26.8% 83.9%;
  }
}
```

---

## Consumer Config — What's Allowed

App-level `tailwind.config.ts` may only contain:

```ts
import preset from "@repo/config/tailwind/preset"
import type { Config } from "tailwindcss"

const config: Config = {
  presets: [preset],
  // ALLOWED: content paths (must be app-specific)
  content: [
    "./src/**/*.{ts,tsx}",
    "../../packages/ui/src/**/*.{ts,tsx}",
  ],
  // ALLOWED: app-specific plugins
  plugins: [require("@tailwindcss/typography")],
  // ALLOWED: safelist for dynamic classes
  safelist: ["bg-primary", "bg-destructive"],
}

export default config
```

### What's NOT Allowed in Consumer Configs

| Prohibited | Why |
|------------|-----|
| `theme.colors` | Defined in preset via CSS variables |
| `theme.fontFamily` | Defined in preset |
| `theme.borderRadius` | Defined in preset |
| `theme.spacing` (base overrides) | Use preset extensions only |
| Inline `keyframes` | Defined in preset |

If an app needs a new color or animation, add it to the preset — not the
app config.

---

## Multi-App Theming

Different apps can have different themes by defining different CSS
variable values while using the same Tailwind preset:

```
apps/
  web/          <- consumer site: blue primary, rounded corners
    globals.css <- --primary: 222 47% 11%; --radius: 0.5rem;
  admin/        <- internal tool: green primary, sharp corners
    globals.css <- --primary: 142 76% 36%; --radius: 0.25rem;
  docs/         <- documentation: purple primary, large radius
    globals.css <- --primary: 262 83% 58%; --radius: 0.75rem;
```

Same preset, same Tailwind classes, different visual output.
Components from `@repo/ui` render correctly in all three apps without
any code changes.

---

## Content Path Rules

Content paths must include all packages that contain Tailwind classes
consumed by that app:

```ts
content: [
  "./src/**/*.{ts,tsx}",                         // app's own files
  "../../packages/ui/src/**/*.{ts,tsx}",          // shared UI package
  "../../packages/feature-auth/src/**/*.{ts,tsx}",// feature package
]
```

**Rule:** If a package uses Tailwind classes, every app that imports
from it must include it in `content`. Missing content paths cause
classes to be purged in production.

**Tip:** Use `@repo/ui`'s `package.json` `exports` field to know which
files to scan:

```json
{
  "exports": {
    "./*": "./src/components/*/index.ts"
  }
}
```
