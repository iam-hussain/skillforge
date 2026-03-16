# CVA Patterns for Component Variants

Production patterns for `class-variance-authority` (CVA) in Shadcn-based
design systems. Every visual state is a variant — no exceptions.

---

## Basic Variant Schema

A CVA definition has three parts: base classes, variants, and defaults.

```tsx
import { cva, type VariantProps } from "class-variance-authority"

const buttonVariants = cva(
  // Base — always applied
  "inline-flex items-center justify-center whitespace-nowrap rounded-md text-sm font-medium transition-colors focus-visible:outline-none focus-visible:ring-1 focus-visible:ring-ring disabled:pointer-events-none disabled:opacity-50",
  {
    variants: {
      // Each key is a variant axis
      variant: {
        default: "bg-primary text-primary-foreground shadow hover:bg-primary/90",
        destructive: "bg-destructive text-destructive-foreground shadow-sm hover:bg-destructive/90",
        outline: "border border-input bg-background shadow-sm hover:bg-accent hover:text-accent-foreground",
        secondary: "bg-secondary text-secondary-foreground shadow-sm hover:bg-secondary/80",
        ghost: "hover:bg-accent hover:text-accent-foreground",
        link: "text-primary underline-offset-4 hover:underline",
      },
      size: {
        sm: "h-8 rounded-md px-3 text-xs",
        default: "h-9 px-4 py-2",
        lg: "h-10 rounded-md px-8",
        icon: "h-9 w-9",
      },
    },
    defaultVariants: {
      variant: "default",
      size: "default",
    },
  }
)
```

**Rules:**
- Base classes handle layout, typography, transitions, focus styles
- Variant classes handle color, spacing, and visual identity
- Every variant axis must have a `defaultVariants` entry
- Use semantic tokens (`bg-primary`, `text-destructive`) — never raw colors

---

## Compound Variants

Use `compoundVariants` when a specific combination of variants needs
unique styling that neither variant alone defines.

```tsx
const alertVariants = cva(
  "relative w-full rounded-lg border px-4 py-3 text-sm",
  {
    variants: {
      variant: {
        default: "bg-background text-foreground",
        destructive: "text-destructive",
        // Note: emerald is used because Shadcn has no --success token by default.
        // Add `--success: 160 84% 39%;` to globals.css to fully tokenize.
        success: "text-emerald-700 dark:text-emerald-400",
      },
      size: {
        sm: "px-3 py-2 text-xs",
        default: "px-4 py-3 text-sm",
        lg: "px-6 py-4 text-base",
      },
      prominent: {
        true: "",
        false: "",
      },
    },
    compoundVariants: [
      // Prominent + destructive = filled background
      {
        variant: "destructive",
        prominent: true,
        class: "bg-destructive/15 border-destructive/50",
      },
      // Prominent + success = filled background
      {
        variant: "success",
        prominent: true,
        // Same emerald exception — no --success token in Shadcn defaults
        class: "bg-emerald-500/15 border-emerald-500/50",
      },
      // Prominent + default = subtle fill
      {
        variant: "default",
        prominent: true,
        class: "bg-muted border-muted-foreground/20",
      },
    ],
    defaultVariants: {
      variant: "default",
      size: "default",
      prominent: false,
    },
  }
)
```

**When to use compound variants:**
- A visual combination can't be expressed by one axis alone
- You need "if variant=X AND size=Y, apply these extra classes"
- You're handling boolean modifiers that interact with other variants

---

## State Variants

Treat component states as first-class CVA variants. Never use
conditional className ternaries for states.

```tsx
const inputVariants = cva(
  "flex h-9 w-full rounded-md border bg-transparent px-3 py-1 text-sm shadow-sm transition-colors file:border-0 file:bg-transparent file:text-sm file:font-medium placeholder:text-muted-foreground focus-visible:outline-none focus-visible:ring-1 focus-visible:ring-ring disabled:cursor-not-allowed disabled:opacity-50",
  {
    variants: {
      state: {
        default: "border-input",
        error: "border-destructive focus-visible:ring-destructive",
        // Note: emerald is used because Shadcn has no --success token by default.
        // Add `--success: 160 84% 39%;` to globals.css to fully tokenize.
        success: "border-emerald-500 focus-visible:ring-emerald-500",
      },
      inputSize: {
        sm: "h-8 px-2 text-xs",
        default: "h-9 px-3 text-sm",
        lg: "h-10 px-4 text-base",
      },
    },
    defaultVariants: {
      state: "default",
      inputSize: "default",
    },
  }
)
```

**Common state axes:**

| State axis | Values | Use case |
|-----------|--------|----------|
| `state` | `default`, `error`, `success`, `warning` | Form validation feedback |
| `loading` | `true`, `false` | Async operations |
| `active` | `true`, `false` | Navigation, toggles |
| `selected` | `true`, `false` | Selection UI, chips |

---

## Composing Multiple CVA Schemas

When a component has distinct visual regions, use separate CVA schemas
rather than one massive schema.

```tsx
// A card with independently styled header, body, and footer

const cardVariants = cva(
  "rounded-xl border bg-card text-card-foreground shadow",
  {
    variants: {
      variant: {
        default: "border-border",
        outlined: "border-2 border-primary/20",
        elevated: "border-none shadow-lg",
      },
      padding: {
        none: "",
        sm: "p-4",
        default: "p-6",
        lg: "p-8",
      },
    },
    defaultVariants: { variant: "default", padding: "default" },
  }
)

const cardHeaderVariants = cva(
  "flex flex-col space-y-1.5",
  {
    variants: {
      alignment: {
        left: "items-start",
        center: "items-center text-center",
      },
    },
    defaultVariants: { alignment: "left" },
  }
)

// Each part gets its own CVA — composed at the component level
```

**Rule:** If a CVA schema exceeds 5 variant axes, split it into
sub-component schemas. Flat is better than deep.

---

## TypeScript Integration

Always extract the variant types from CVA using `VariantProps`:

```tsx
import { type VariantProps } from "class-variance-authority"

// Automatic type inference from the schema
type ButtonVariants = VariantProps<typeof buttonVariants>
// Result: { variant?: "default" | "destructive" | ... | null; size?: "sm" | "default" | ... | null }

// Use in component props
interface ButtonProps
  extends React.ComponentPropsWithoutRef<"button">,
    VariantProps<typeof buttonVariants> {
  loading?: boolean
}
```

**Rules:**
- Never manually duplicate variant types — always derive from CVA
- `VariantProps` makes all variants optional (they have defaults)
- Custom props (`loading`, `dot`, etc.) go in the interface, not CVA

---

## Variant Naming Conventions

| Axis | Good names | Bad names |
|------|-----------|-----------|
| Visual style | `variant` | `type`, `kind`, `style` |
| Dimensions | `size` | `scale`, `dimension` |
| Color intent | `status`, `severity` | `color`, `theme` |
| Boolean modifier | `prominent`, `rounded`, `dot` | `isProminent`, `hasRoundedCorners` |

**Rules:**
- Use `variant` as the primary visual axis (matches Shadcn convention)
- Use `size` for dimensional scaling (matches Shadcn convention)
- Boolean variants use bare names (`prominent: { true: "...", false: "..." }`)
- Never prefix booleans with `is` or `has` in CVA schemas

---

## Exporting Variants for Reuse

Export variant schemas alongside components so consumers can access
variant values programmatically:

```tsx
// components/ui/Button.tsx
export { Button, buttonVariants }
export type { ButtonProps }

// Consumer can check available variants at type level
import { buttonVariants } from "@/components/ui/Button"

// Or compose in another CVA schema
const iconButtonVariants = cva(
  buttonVariants({ variant: "ghost", size: "icon" }),
  {
    variants: {
      color: {
        default: "text-muted-foreground hover:text-foreground",
        destructive: "text-destructive hover:text-destructive/80",
      },
    },
    defaultVariants: { color: "default" },
  }
)
```

---

## Anti-Patterns

| Never do this | Do this instead |
|---------------|-----------------|
| `className={active ? "bg-primary" : "bg-muted"}` | CVA variant: `active: { true: "bg-primary", false: "bg-muted" }` |
| 10+ variant axes in one schema | Split into sub-component CVA schemas |
| `variant: { red: "bg-red-500" }` | `variant: { destructive: "bg-destructive" }` |
| Duplicating Shadcn's buttonVariants | Import and extend with composition |
| Manual type unions: `"sm" \| "md" \| "lg"` | `VariantProps<typeof schema>` |
| Mixing layout + color in one axis | Separate axes: `variant` for color, `size` for layout |
