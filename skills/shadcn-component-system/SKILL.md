---
name: shadcn-component-system
description: "Use when building or extending UI components with Shadcn UI and class-variance-authority (CVA). Enforces variant-first architecture, semantic theming, atomic decomposition, and prop-driven scalability for Next.js and Vite projects."
---

# Shadcn Component System — Next.js & Vite

You are an expert React architect specializing in design system construction
using Shadcn UI and class-variance-authority (CVA). When given a UI
requirement, build components that are variant-driven, theme-synced, and
atomically decomposed.

> For component decomposition, see **react-component-splitter**.
> For project directory structure, see **react-project-structure**.

---

## Stack Detection

First, detect the stack from the code or context:

- **Next.js (App Router)** — `app/` routing, Server/Client Components,
  Tailwind CSS, `cn()` utility
- **Next.js (Pages Router)** — `pages/`, same styling approach
- **React + Vite** — standard React patterns, Tailwind CSS, no SSR

Always verify: Is Shadcn UI already installed? Does a `cn()` utility exist
in `lib/utils.ts`? Is `tailwind.config` using CSS variables for theming?

---

## Core Rules

### 1. Shadcn First

Always check if a Shadcn primitive exists before building from scratch.
Extend existing primitives — never duplicate them.

```bash
# Check available components
npx shadcn@latest add button
npx shadcn@latest add input
npx shadcn@latest add dialog
```

If Shadcn provides it, use it. If you need customization, wrap and extend
it with CVA — never fork and modify the source.

### 2. CVA for All Visual States

Every component with visual variants uses `class-variance-authority`.
No inline conditional classes. No ternary className logic. States like
`loading`, `error`, `active`, and `disabled` are first-class CVA variants.

```tsx
// Wrong — inline conditional
className={isError ? "border-destructive" : "border-input"}

// Right — CVA variant
className={cn(inputVariants({ state: isError ? "error" : "default" }))}
```

> See `references/cva-patterns.md` for variant schemas, compound variants,
> and composition patterns.

### 3. Semantic Theme Tokens Only

All colors reference CSS variables from `globals.css`. Never use raw
Tailwind colors (`blue-500`, `red-600`) or hex codes in components.

```css
/* globals.css — the single source of truth */
:root {
  --primary: 222.2 47.4% 11.2%;
  --primary-foreground: 210 40% 98%;
  --destructive: 0 84.2% 60.2%;
  --muted: 210 40% 96.1%;
  --muted-foreground: 215.4 16.3% 46.9%;
  --accent: 210 40% 96.1%;
  --radius: 0.5rem;
}
```

**Map every color to a semantic token:**

| Raw color intent | Semantic token |
|-----------------|----------------|
| Brand main color | `bg-primary`, `text-primary` |
| Danger/error state | `bg-destructive`, `text-destructive` |
| Subdued text | `text-muted-foreground` |
| Hover highlight | `bg-accent` |
| Card/section bg | `bg-card` |
| Input borders | `border-input` |

Both light and dark modes must work automatically via CSS variable swaps —
no `dark:` prefixes scattered through component code.

### 4. Atomic Decomposition

Break every UI requirement into three layers:

| Level | Location | Role | Examples |
|-------|----------|------|----------|
| Atoms | `components/ui/` | Single-purpose primitives, zero business logic | Button, Input, Badge, Avatar, Skeleton |
| Molecules | `components/shared/` | Composed atoms forming a reusable unit | FormField, StatCard, SearchBar, EmptyState |
| Organisms | `features/*/components/` | Feature-specific blocks with business logic | UserProfileCard, InvoiceTable, ChatMessage |

**Decomposition rule:** If it wraps one HTML element → atom. If it combines
2+ atoms for a generic purpose → molecule. If it contains business logic
or belongs to one feature → organism.

### 5. Prop-Driven Scalability

Every component preserves native HTML attributes using TypeScript's
`ComponentPropsWithoutRef`. CVA variants are explicit props. Native HTML
attrs pass through via rest spread.

```tsx
interface ButtonProps
  extends React.ComponentPropsWithoutRef<"button">,
    VariantProps<typeof buttonVariants> {
  loading?: boolean
}
```

This ensures every component is "open for extension" — consumers can pass
`aria-*`, `data-*`, `id`, `style`, or any native attr without the
component needing to know about it.

### 6. Zero Class Repetition

If a Tailwind class combination appears in 2+ places, it must be either:
- A CVA variant (for visual states)
- A sub-component (for structural patterns)
- A `cn()` composition (for one-off merges)

Never copy-paste long className strings across components.

---

## The `cn()` Utility

Every project using this system must have a `cn()` utility that merges
classes with conflict resolution:

```tsx
// lib/utils.ts
import { type ClassValue, clsx } from "clsx"
import { twMerge } from "tailwind-merge"

export function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs))
}
```

Use `cn()` everywhere — it handles conditional classes, CVA output merging,
and consumer `className` overrides without conflicts.

---

## Component Anatomy

Every component in this system follows the same structure:

```tsx
// 1. Imports
import * as React from "react"
import { cva, type VariantProps } from "class-variance-authority"
import { cn } from "@/lib/utils"

// 2. CVA variant definition
const componentVariants = cva(
  "base-classes-here",
  {
    variants: {
      variant: { /* ... */ },
      size: { /* ... */ },
    },
    defaultVariants: {
      variant: "default",
      size: "md",
    },
  }
)

// 3. Props interface extending HTML attrs + CVA variants
interface ComponentProps
  extends React.ComponentPropsWithoutRef<"div">,
    VariantProps<typeof componentVariants> {
  // custom props here
}

// 4. Component with forwardRef
const Component = React.forwardRef<HTMLDivElement, ComponentProps>(
  ({ variant, size, className, ...props }, ref) => {
    return (
      <div
        ref={ref}
        className={cn(componentVariants({ variant, size }), className)}
        {...props}
      />
    )
  }
)
Component.displayName = "Component"

// 5. Named exports
export { Component, componentVariants }
export type { ComponentProps }
```

> See `references/shadcn-extension.md` for patterns on wrapping and
> extending existing Shadcn components.

---

## Output Format

When building a component or set of components, always deliver:

1. **Requirement decomposition** — atoms, molecules, organisms identified
2. **CVA schema** — variant definitions with all states
3. **Theme tokens used** — which CSS variables the component relies on
4. **File tree** — where each file goes
5. **Complete code** — no placeholders, no `// TODO` comments
6. **Usage example** — how to consume the component with variants

---

## Framework-Specific Rules

### Next.js

- Atoms and molecules are typically Server Components (no interactivity)
- Add `"use client"` only when the component uses useState, event handlers,
  or browser APIs
- Keep `"use client"` on the organism level, not on atoms
- Shadcn primitives that use Radix UI are client components — import them
  only in client component trees

### Vite

- All components are client-side by default
- Lazy load heavy organisms: `const Table = lazy(() => import(...))`
- No SSR considerations for theme hydration

---

## Anti-Patterns

| Never do this | Do this instead |
|---------------|-----------------|
| `className={isActive ? "bg-blue-500" : "bg-gray-200"}` | CVA variant: `variant: { active: "bg-primary", inactive: "bg-muted" }` |
| Raw colors: `text-red-500`, `bg-blue-600` | Semantic tokens: `text-destructive`, `bg-primary` |
| Copy-pasting className strings across files | Extract CVA variant or sub-component |
| Building from scratch when Shadcn has it | `npx shadcn@latest add [component]` then extend |
| `dark:bg-gray-800` scattered everywhere | CSS variable swap in `globals.css` |
| Massive `className` prop with 15+ classes | Split into CVA base + variants |
| Accepting only custom props, no HTML attrs | `extends ComponentPropsWithoutRef<"element">` |
| Hardcoded border radius: `rounded-lg` | Theme token: `rounded-[var(--radius)]` or Shadcn default |
| Forking Shadcn source files to modify | Wrap with CVA extension layer |

---

## Example: Building a Status Badge System

**Requirement:** A badge component that shows entity status across the app —
orders (pending, processing, shipped, delivered), users (active, inactive,
banned), payments (success, failed, refunded).

### Step 1: Decompose

- **Atom** — `StatusBadge` in `components/ui/` (variant-driven, no business logic)
- No molecule needed — single atom handles it
- **Organism usage** — feature components consume `StatusBadge` with domain-specific mappings

### Step 2: CVA Schema

```tsx
// components/ui/StatusBadge.tsx
import * as React from "react"
import { cva, type VariantProps } from "class-variance-authority"
import { cn } from "@/lib/utils"

const statusBadgeVariants = cva(
  "inline-flex items-center rounded-full px-2.5 py-0.5 text-xs font-medium transition-colors",
  {
    variants: {
      status: {
        // Note: emerald/amber are used here because Shadcn's default token set
        // does not include --success or --warning variables. To fully tokenize,
        // add `--success: 160 84% 39%; --warning: 38 92% 50%;` to globals.css
        // then replace with `bg-success/15 text-success` / `bg-warning/15 text-warning`.
        success: "bg-emerald-500/15 text-emerald-700 dark:text-emerald-400",
        warning: "bg-amber-500/15 text-amber-700 dark:text-amber-400",
        error: "bg-destructive/15 text-destructive",
        info: "bg-primary/15 text-primary",
        neutral: "bg-muted text-muted-foreground",
      },
      size: {
        sm: "px-2 py-0.5 text-xs",
        md: "px-2.5 py-0.5 text-xs",
        lg: "px-3 py-1 text-sm",
      },
    },
    defaultVariants: {
      status: "neutral",
      size: "md",
    },
  }
)

interface StatusBadgeProps
  extends React.ComponentPropsWithoutRef<"span">,
    VariantProps<typeof statusBadgeVariants> {
  dot?: boolean
}

const StatusBadge = React.forwardRef<HTMLSpanElement, StatusBadgeProps>(
  ({ status, size, dot = false, className, children, ...props }, ref) => {
    return (
      <span
        ref={ref}
        className={cn(statusBadgeVariants({ status, size }), className)}
        {...props}
      >
        {dot && (
          <span className="mr-1.5 inline-block h-1.5 w-1.5 rounded-full bg-current" />
        )}
        {children}
      </span>
    )
  }
)
StatusBadge.displayName = "StatusBadge"

export { StatusBadge, statusBadgeVariants }
export type { StatusBadgeProps }
```

### Step 3: Domain Mapping in Organism

```tsx
// features/orders/components/OrderStatusBadge.tsx
import { StatusBadge } from "@/components/ui/StatusBadge"
import type { OrderStatus } from "../types/order.types"

const STATUS_MAP: Record<OrderStatus, { status: "success" | "warning" | "error" | "info" | "neutral"; label: string }> = {
  pending: { status: "warning", label: "Pending" },
  processing: { status: "info", label: "Processing" },
  shipped: { status: "info", label: "Shipped" },
  delivered: { status: "success", label: "Delivered" },
  cancelled: { status: "error", label: "Cancelled" },
}

interface OrderStatusBadgeProps {
  status: OrderStatus
  className?: string
}

export function OrderStatusBadge({ status, className }: OrderStatusBadgeProps) {
  const config = STATUS_MAP[status]
  return (
    <StatusBadge status={config.status} dot className={className}>
      {config.label}
    </StatusBadge>
  )
}
```

### Step 4: Usage

```tsx
// Before — inline conditional chaos
<span className={cn(
  "inline-flex items-center rounded-full px-2.5 py-0.5 text-xs font-medium",
  order.status === "delivered" && "bg-green-100 text-green-800",
  order.status === "pending" && "bg-yellow-100 text-yellow-800",
  order.status === "cancelled" && "bg-red-100 text-red-800",
)}>
  {order.status}
</span>

// After — variant-driven, theme-synced
<OrderStatusBadge status={order.status} />
```

**Key wins:**
- Zero conditional className logic at the call site
- Theme changes propagate automatically via CSS variables
- New statuses added in one place (the CVA schema + domain map)
- Dark mode works without touching component code
- Any HTML attribute passable via rest spread
