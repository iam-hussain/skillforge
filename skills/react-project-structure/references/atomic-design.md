# Atomic Design for React Components

A practical mapping of Brad Frost's Atomic Design hierarchy to React
component architecture in Next.js and Vite projects.

---

## The Hierarchy

### Atoms → `components/ui/`

The smallest, most reusable building blocks. No business logic, no API
calls, no feature-specific behavior. Pure UI primitives.

**Examples:** Button, Input, Badge, Avatar, Spinner, Icon, Label, Tooltip

**Rules:**
- Accept `className` for styling overrides
- Forward refs when wrapping DOM elements
- Generic event handlers (`onClick`, `onChange`) — never domain-specific
- No hardcoded strings, colors, or sizes — use props or design tokens
- Self-contained — zero external dependencies beyond React

```tsx
interface ButtonProps extends React.ButtonHTMLAttributes<HTMLButtonElement> {
  variant?: "primary" | "secondary" | "ghost"
  size?: "sm" | "md" | "lg"
  loading?: boolean
}

const Button = React.forwardRef<HTMLButtonElement, ButtonProps>(
  ({ variant = "primary", size = "md", loading, children, className, disabled, ...props }, ref) => {
    return (
      <button
        ref={ref}
        className={cn(buttonVariants({ variant, size }), className)}
        disabled={disabled || loading}
        {...props}
      >
        {loading && <Spinner className="mr-2 h-4 w-4" />}
        {children}
      </button>
    )
  }
)
Button.displayName = "Button"

export { Button }
export type { ButtonProps }
```

### Molecules → `components/shared/`

Combinations of atoms that form a functional unit. Still reusable across
features, but with a specific purpose.

**Examples:** FormField (Label + Input + Error), Card, Modal, DataTable,
SearchBar, Pagination, EmptyState, ConfirmDialog

**Rules:**
- Compose atoms — never duplicate atom code
- Accept data via props, not via hooks or API calls
- Can have internal state (e.g., modal open/close) but no business state
- Named exports from `components/shared/index.ts`

```tsx
interface FormFieldProps {
  label: string
  error?: string
  required?: boolean
  children: React.ReactNode
  className?: string
}

export function FormField({ label, error, required, children, className }: FormFieldProps) {
  const id = React.useId()

  return (
    <div className={cn("space-y-1.5", className)}>
      <Label htmlFor={id}>
        {label}
        {required && <span className="text-destructive ml-0.5">*</span>}
      </Label>
      {React.cloneElement(children as React.ReactElement, { id })}
      {error && <p className="text-sm text-destructive">{error}</p>}
    </div>
  )
}
```

### Organisms → `features/*/components/`

Feature-specific components that combine molecules and atoms with
business logic. These live inside feature folders.

**Examples:** LoginForm, DashboardHeader, ProductCard, UserProfile,
InvoiceTable, NotificationList

**Rules:**
- Live in `features/<domain>/components/`
- Can use feature-specific hooks and types
- May call API through hooks — never directly
- Exported through the feature's `index.ts` barrel

```tsx
// features/auth/components/LoginForm.tsx
import { Button } from "@/components/ui/Button"
import { FormField } from "@/components/shared/FormField"
import { useAuth } from "../hooks/useAuth"
import type { LoginCredentials } from "../types"

interface LoginFormProps {
  onSuccess?: () => void
  className?: string
}

export function LoginForm({ onSuccess, className }: LoginFormProps) {
  const { login, isLoading, error } = useAuth()

  const handleSubmit = async (e: React.FormEvent<HTMLFormElement>) => {
    e.preventDefault()
    const formData = new FormData(e.currentTarget)
    const credentials: LoginCredentials = {
      email: formData.get("email") as string,
      password: formData.get("password") as string,
    }
    const success = await login(credentials)
    if (success) onSuccess?.()
  }

  return (
    <form onSubmit={handleSubmit} className={cn("space-y-4", className)}>
      <FormField label="Email" required error={error?.email}>
        <Input name="email" type="email" />
      </FormField>
      <FormField label="Password" required error={error?.password}>
        <Input name="password" type="password" />
      </FormField>
      <Button type="submit" loading={isLoading} className="w-full">
        Sign In
      </Button>
    </form>
  )
}
```

### Templates → `app/` layouts (Next.js) or layout components (Vite)

Page-level layout structures that define the arrangement of organisms.
In Next.js, these are `layout.tsx` files. In Vite, these are layout
components used by React Router.

**Next.js:**
```tsx
// app/(dashboard)/layout.tsx — Server Component
export default function DashboardLayout({ children }: { children: React.ReactNode }) {
  return (
    <div className="flex min-h-screen">
      <Sidebar />
      <main className="flex-1 p-6">{children}</main>
    </div>
  )
}
```

**Vite:**
```tsx
// layouts/DashboardLayout.tsx
import { Outlet } from "react-router-dom"

export function DashboardLayout() {
  return (
    <div className="flex min-h-screen">
      <Sidebar />
      <main className="flex-1 p-6">
        <Outlet />
      </main>
    </div>
  )
}
```

### Pages → `app/*/page.tsx` (Next.js) or `pages/` (Vite)

Route-level components that compose organisms and connect them to data.
In Next.js App Router, these are Server Components by default.

**Next.js:**
```tsx
// app/dashboard/page.tsx — Server Component
import { DashboardHeader } from "@/features/dashboard"
import { MetricsGrid } from "@/features/dashboard"
import { getMetrics } from "@/lib/api"

export default async function DashboardPage() {
  const metrics = await getMetrics()

  return (
    <>
      <DashboardHeader />
      <MetricsGrid data={metrics} />
    </>
  )
}
```

---

## Decision Guide: Where Does This Component Go?

| Question | Yes → | No → |
|----------|-------|------|
| Is it a single HTML element wrapper? | `components/ui/` (atom) | ↓ |
| Is it used in 2+ features? | `components/shared/` (molecule) | ↓ |
| Does it belong to one feature? | `features/*/components/` (organism) | ↓ |
| Does it define page structure? | Layout file (template) | ↓ |
| Does it connect data to UI? | Page file (page) | Reconsider |

---

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Atom with API calls | Move fetch logic to a hook or parent |
| Molecule with feature-specific types | Move to feature's components/ |
| Organism in components/shared/ | Move to the owning feature folder |
| Duplicate atoms across features | Extract to components/ui/ |
| Layout with business logic | Push logic into organisms or hooks |
