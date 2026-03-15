# Feature-Slice Architecture

Organize React code by business domain rather than technical role.
Each feature is a self-contained module with its own components, hooks,
utils, types, and public API.

---

## Structure

```
features/
  auth/
    components/           ← feature-specific UI
      LoginForm.tsx
      SignupForm.tsx
      AuthGuard.tsx
    hooks/                ← feature-specific hooks
      useAuth.ts
      useSession.ts
    actions/              ← server actions (Next.js only)
      loginAction.ts
      signupAction.ts
    utils/                ← pure helper functions
      validatePassword.ts
      formatAuthError.ts
    types/                ← feature-specific types
      auth.types.ts
    index.ts              ← barrel export (public API)
```

---

## Rules

### 1. Each Feature Owns Its Domain

A feature folder contains **everything** related to one business domain:
auth, dashboard, payments, settings, etc.

- Components, hooks, utils, and types all live together
- No reaching across feature boundaries for internals
- Cross-feature communication goes through shared hooks or the barrel export

### 2. Barrel Exports Define the Public API

The `index.ts` file exports **only what other features need**. Internal
implementation details stay private.

```tsx
// features/auth/index.ts
// Components
export { LoginForm } from "./components/LoginForm"
export { SignupForm } from "./components/SignupForm"
export { AuthGuard } from "./components/AuthGuard"

// Hooks
export { useAuth } from "./hooks/useAuth"
export { useSession } from "./hooks/useSession"

// Types
export type { User, AuthState, LoginCredentials } from "./types/auth.types"

// DO NOT export:
// - Internal utilities (validatePassword, formatAuthError)
// - Internal components (PasswordStrengthMeter — used only by SignupForm)
// - Context providers (AuthContext — implementation detail)
```

**Rule: Named exports only.** Never use default exports in barrel files.

### 3. Import Direction

Features can import from:
- ✅ `components/ui/` — shared atoms
- ✅ `components/shared/` — shared molecules
- ✅ `hooks/` — shared hooks
- ✅ `lib/` — utilities, API clients, config
- ✅ `types/` — global types
- ✅ Other features' `index.ts` — their public API only
- ❌ Other features' internal files — never reach into another feature's components/ or hooks/

```tsx
// ✅ Correct — importing from another feature's public API
import { useAuth } from "@/features/auth"

// ❌ Wrong — reaching into another feature's internals
import { useAuth } from "@/features/auth/hooks/useAuth"
```

### 4. Server Actions (Next.js App Router)

Server actions live in `actions/` within the feature folder. They handle
form submissions, mutations, and server-side logic.

```tsx
// features/auth/actions/loginAction.ts
"use server"

import { z } from "zod"
import { redirect } from "next/navigation"
import { createSession } from "@/lib/session"

const loginSchema = z.object({
  email: z.string().email(),
  password: z.string().min(8),
})

export async function loginAction(formData: FormData) {
  const parsed = loginSchema.safeParse({
    email: formData.get("email"),
    password: formData.get("password"),
  })

  if (!parsed.success) {
    return { error: "Invalid credentials" }
  }

  // authenticate, create session, redirect
  await createSession(parsed.data)
  redirect("/dashboard")
}
```

---

## When to Create a New Feature

Create a new feature folder when:

- [ ] The domain has 2+ components that share state or types
- [ ] The code has its own hooks for data fetching or state management
- [ ] There are domain-specific utility functions
- [ ] The feature has its own route(s) in the app

**Don't create a feature for:**
- A single reusable component → goes in `components/ui/` or `components/shared/`
- A single utility function → goes in `lib/`
- A single type → goes in `types/`

---

## Cross-Feature Communication

When features need to share data, use one of these patterns:

### Option 1: Shared Hook (preferred)

```tsx
// hooks/useCurrentUser.ts — shared, not in any feature
import { useAuth } from "@/features/auth"

export function useCurrentUser() {
  const { user } = useAuth()
  return user
}
```

### Option 2: Props from Parent Route

```tsx
// app/dashboard/page.tsx — Server Component
import { getUser } from "@/lib/api"
import { DashboardContent } from "@/features/dashboard"
import { UserBadge } from "@/features/users"

export default async function DashboardPage() {
  const user = await getUser()
  return (
    <>
      <UserBadge user={user} />
      <DashboardContent userId={user.id} />
    </>
  )
}
```

### Option 3: Event-Based (for loosely coupled features)

```tsx
// lib/events.ts
type EventMap = {
  "cart:updated": { itemCount: number }
  "auth:logout": undefined
}

const bus = new EventTarget()

export function emit<K extends keyof EventMap>(event: K, detail: EventMap[K]) {
  bus.dispatchEvent(new CustomEvent(event, { detail }))
}

export function on<K extends keyof EventMap>(event: K, handler: (detail: EventMap[K]) => void) {
  const listener = (e: Event) => handler((e as CustomEvent).detail)
  bus.addEventListener(event, listener)
  return () => bus.removeEventListener(event, listener)
}
```

---

## Promoting Code to Shared

When a component or hook used in one feature starts being needed in a
second feature, promote it:

1. **Component used in 1 feature** → stays in `features/*/components/`
2. **Component used in 2+ features** → move to `components/shared/`
3. **Hook used in 1 feature** → stays in `features/*/hooks/`
4. **Hook used in 2+ features** → move to root `hooks/`
5. **Type used in 1 feature** → stays in `features/*/types/`
6. **Type used in 2+ features** → move to root `types/`

**Rule:** Don't prematurely extract. Wait until you actually need it in
a second place before promoting to shared.

---

## Example: E-Commerce Feature Layout

```
features/
  cart/
    components/
      CartDrawer.tsx
      CartItem.tsx
      CartSummary.tsx
    hooks/
      useCart.ts
      useCartTotal.ts
    utils/
      calculateDiscount.ts
    types/
      cart.types.ts
    index.ts
  products/
    components/
      ProductCard.tsx
      ProductGrid.tsx
      ProductDetail.tsx
    hooks/
      useProducts.ts
      useProductSearch.ts
    utils/
      formatPrice.ts
    types/
      product.types.ts
    index.ts
  checkout/
    components/
      CheckoutForm.tsx
      PaymentMethod.tsx
      OrderSummary.tsx
    hooks/
      useCheckout.ts
    actions/
      placeOrderAction.ts
    types/
      checkout.types.ts
    index.ts
```

Each feature is independently understandable. A developer working on
`cart/` doesn't need to read `checkout/` internals — just its public API
from `index.ts`.
