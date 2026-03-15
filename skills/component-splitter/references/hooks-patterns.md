# Custom Hook Extraction Patterns

When and how to extract custom hooks from React components to separate
business logic from presentation.

---

## When to Extract a Hook

| Signal | Action |
|--------|--------|
| 3+ `useState` calls | Group related state into one hook |
| `useEffect` with fetch logic | Extract to `useQuery`-style hook |
| Business logic mixed with JSX | Extract logic, return computed values |
| Same state pattern in 2+ components | Extract shared hook |
| Complex event handlers (>5 lines) | Move handler logic to hook |

---

## Hook Architecture Rules

1. **No JSX in hooks** — hooks return data and handlers, never markup
2. **Return typed objects** — not arrays (except simple `[value, setter]` pairs)
3. **Co-locate with feature** — `features/<domain>/hooks/useFeature.ts`
4. **Shared hooks** go in root `hooks/` — only if used across 2+ features
5. **Name convention** — always `use` + domain + action: `useAuthLogin`, `useCartItems`
6. **Single responsibility** — one hook per concern, compose them in components

---

## Pattern 1: State Management Hook

Extract when a component has 3+ related `useState` calls.

**Before:**
```tsx
function ProductFilter() {
  const [category, setCategory] = useState("")
  const [priceRange, setPriceRange] = useState<[number, number]>([0, 1000])
  const [sortBy, setSortBy] = useState("newest")
  const [inStock, setInStock] = useState(true)

  const resetFilters = () => {
    setCategory("")
    setPriceRange([0, 1000])
    setSortBy("newest")
    setInStock(true)
  }

  // ... 100 lines of JSX
}
```

**After:**
```tsx
// features/products/hooks/useProductFilters.ts
interface ProductFilters {
  category: string
  priceRange: [number, number]
  sortBy: string
  inStock: boolean
}

const DEFAULT_FILTERS: ProductFilters = {
  category: "",
  priceRange: [0, 1000],
  sortBy: "newest",
  inStock: true,
}

export function useProductFilters(initial?: Partial<ProductFilters>) {
  const [filters, setFilters] = useState<ProductFilters>({
    ...DEFAULT_FILTERS,
    ...initial,
  })

  const updateFilter = <K extends keyof ProductFilters>(
    key: K,
    value: ProductFilters[K]
  ) => {
    setFilters((prev) => ({ ...prev, [key]: value }))
  }

  const resetFilters = () => setFilters({ ...DEFAULT_FILTERS, ...initial })

  return { filters, updateFilter, resetFilters }
}
```

```tsx
// features/products/components/ProductFilter.tsx
import { useProductFilters } from "../hooks/useProductFilters"

function ProductFilter() {
  const { filters, updateFilter, resetFilters } = useProductFilters()
  // Clean JSX only — no state management logic
}
```

---

## Pattern 2: Data Fetching Hook

Extract when a component fetches data in `useEffect`.

**Before:**
```tsx
function UserProfile({ userId }: { userId: string }) {
  const [user, setUser] = useState<User | null>(null)
  const [loading, setLoading] = useState(true)
  const [error, setError] = useState<Error | null>(null)

  useEffect(() => {
    let cancelled = false
    setLoading(true)
    fetchUser(userId)
      .then((data) => { if (!cancelled) setUser(data) })
      .catch((err) => { if (!cancelled) setError(err) })
      .finally(() => { if (!cancelled) setLoading(false) })
    return () => { cancelled = true }
  }, [userId])

  // ... render logic
}
```

**After (with React Query / TanStack Query):**
```tsx
// features/users/hooks/useUser.ts
import { useQuery } from "@tanstack/react-query"
import { fetchUser } from "@/lib/api"
import type { User } from "../types"

export function useUser(userId: string) {
  return useQuery<User>({
    queryKey: ["user", userId],
    queryFn: () => fetchUser(userId),
    enabled: !!userId,
  })
}
```

```tsx
// features/users/components/UserProfile.tsx
import { useUser } from "../hooks/useUser"

function UserProfile({ userId }: { userId: string }) {
  const { data: user, isLoading, error } = useUser(userId)
  // Pure rendering — no fetch logic
}
```

**For Next.js App Router — prefer Server Components instead:**
```tsx
// app/users/[id]/page.tsx — Server Component, no hook needed
import { fetchUser } from "@/lib/api"

export default async function UserPage({ params }: { params: { id: string } }) {
  const user = await fetchUser(params.id)
  return <UserProfile user={user} />
}
```

---

## Pattern 3: Form Logic Hook

Extract when form handling dominates a component.

```tsx
// features/auth/hooks/useLoginForm.ts
import { useState } from "react"
import { z } from "zod"

const loginSchema = z.object({
  email: z.string().email("Invalid email"),
  password: z.string().min(8, "Password must be 8+ characters"),
})

type LoginFormData = z.infer<typeof loginSchema>
type LoginFormErrors = Partial<Record<keyof LoginFormData, string>>

export function useLoginForm(onSubmit: (data: LoginFormData) => Promise<void>) {
  const [values, setValues] = useState<LoginFormData>({ email: "", password: "" })
  const [errors, setErrors] = useState<LoginFormErrors>({})
  const [isSubmitting, setIsSubmitting] = useState(false)

  const setValue = <K extends keyof LoginFormData>(field: K, value: LoginFormData[K]) => {
    setValues((prev) => ({ ...prev, [field]: value }))
    setErrors((prev) => ({ ...prev, [field]: undefined }))
  }

  const handleSubmit = async (e: React.FormEvent) => {
    e.preventDefault()
    const result = loginSchema.safeParse(values)
    if (!result.success) {
      const fieldErrors: LoginFormErrors = {}
      result.error.errors.forEach((err) => {
        const field = err.path[0] as keyof LoginFormData
        fieldErrors[field] = err.message
      })
      setErrors(fieldErrors)
      return
    }
    setIsSubmitting(true)
    try {
      await onSubmit(result.data)
    } finally {
      setIsSubmitting(false)
    }
  }

  return { values, errors, isSubmitting, setValue, handleSubmit }
}
```

---

## Pattern 4: Toggle / Disclosure Hook

For components with open/close, show/hide, or toggle behavior.

```tsx
// hooks/useDisclosure.ts — shared, used in 2+ features
export function useDisclosure(initial = false) {
  const [isOpen, setIsOpen] = useState(initial)

  const open = useCallback(() => setIsOpen(true), [])
  const close = useCallback(() => setIsOpen(false), [])
  const toggle = useCallback(() => setIsOpen((prev) => !prev), [])

  return { isOpen, open, close, toggle }
}
```

---

## Hook Composition

Compose multiple hooks in a component — each hook handles one concern:

```tsx
function Dashboard() {
  const { filters, updateFilter } = useDashboardFilters()
  const { data: metrics, isLoading } = useMetrics(filters)
  const { isOpen, toggle } = useDisclosure()

  // Each hook owns one concern
  // Component only handles layout and event wiring
}
```

---

## What NOT to Put in Hooks

| Don't | Why |
|-------|-----|
| JSX rendering | Hooks return data, components render |
| Direct DOM manipulation | Use refs in the component instead |
| Global side effects without cleanup | Always clean up in the return |
| Tightly coupled business logic from 2+ domains | Split into separate hooks |
| Static utility functions | Put in `utils/`, not a hook |
