---
name: react-project-structure
description: "Use when scaffolding a new React project or reorganizing an existing codebase's file structure. Defines folder conventions for Next.js App Router, Pages Router, and Vite projects using Feature-Slice and Atomic Design patterns."
---

# React Project Structure — Next.js & Vite

You are an expert React architect specializing in scalable project
organization. When given a React project (new or existing), define or
restructure its directory layout using Feature-Slice architecture with
Atomic Design component hierarchy.

---

## Stack Detection

First, detect the stack from the code or context:

- **Next.js (App Router)** — `app/` routing, Server/Client Components,
  async components, metadata API
- **Next.js (Pages Router)** — `pages/`, `getServerSideProps` /
  `getStaticProps` patterns
- **React + Vite** — standard React patterns, React Router if routing
  is involved, no SSR assumptions

Always ask or infer: TypeScript or JavaScript? Tailwind, CSS Modules,
or styled-components?

---

## Primary Architecture

### Next.js App Router

```
src/
  app/                        <- routes, layouts, loading, error
    (auth)/
    dashboard/
  components/
    ui/                       <- pure, headless atoms (Button, Input,
    |                            Badge, Avatar, Spinner)
    shared/                   <- cross-feature molecules (FormField,
                                 Card, Modal, DataTable)
  features/                   <- one folder per domain
    auth/
      components/             <- feature-specific UI
      hooks/                  <- useAuth, useSession
      actions/                <- Next.js server actions
      utils/
      types/
      index.ts                <- barrel export (public API only)
    dashboard/
      ...
  hooks/                      <- shared custom hooks
  lib/                        <- api clients, configs, helpers
  types/                      <- global types and interfaces
```

### React + Vite

```
src/
  components/
    ui/                       <- atoms: Button, Input, Badge
    shared/                   <- molecules: FormField, Card, Modal
  features/                   <- domain folders
    auth/
      components/
      hooks/
      utils/
      types/
      index.ts
  pages/                      <- route-level components
  hooks/                      <- shared hooks
  lib/                        <- utils, api, config
  types/                      <- global types
```

> For detailed architecture patterns, see `references/feature-slice.md`
> and `references/atomic-design.md`.

---

## Component Placement Rules

### The Hierarchy

| Level | Directory | What goes here | Examples |
|-------|-----------|----------------|----------|
| Atoms | `components/ui/` | Smallest reusable UI primitives, no business logic | Button, Input, Badge, Avatar, Spinner |
| Molecules | `components/shared/` | Compositions of atoms, still reusable across features | FormField, Card, Modal, DataTable |
| Organisms | `features/*/components/` | Feature-specific UI with business logic | LoginForm, DashboardHeader, ProductCard |
| Templates | `app/` layouts or layout components | Page-level layout structures | DashboardLayout, AuthLayout |
| Pages | `app/*/page.tsx` or `pages/` | Route-level components connecting data to UI | DashboardPage, SettingsPage |

### Decision Guide

| Question | Yes | No |
|----------|-----|-----|
| Is it a single HTML element wrapper? | `components/ui/` (atom) | next question |
| Is it used in 2+ features? | `components/shared/` (molecule) | next question |
| Does it belong to one feature? | `features/*/components/` (organism) | next question |
| Does it define page structure? | Layout file (template) | next question |
| Does it connect data to UI? | Page file (page) | Reconsider |

> See `references/atomic-design.md` for full hierarchy with code examples.

---

## Feature Folder Rules

### 1. Each Feature Owns Its Domain

A feature folder contains **everything** related to one business domain:
auth, dashboard, payments, settings.

- Components, hooks, utils, and types live together
- No reaching across feature boundaries for internals
- Cross-feature communication goes through shared hooks or barrel exports

### 2. Barrel Exports Define the Public API

```tsx
// features/auth/index.ts
export { LoginForm } from "./components/LoginForm"
export { useAuth } from "./hooks/useAuth"
export type { User, AuthState } from "./types/auth.types"

// DO NOT export internal utilities or implementation details
```

**Rule:** Named exports only. Never use default exports in barrel files.

### 3. Import Direction

Features can import from:
- `components/ui/` and `components/shared/`
- `hooks/`, `lib/`, `types/` (shared)
- Other features' `index.ts` (public API only)
- **Never** another feature's internal files

```tsx
// Correct
import { useAuth } from "@/features/auth"

// Wrong — reaching into internals
import { useAuth } from "@/features/auth/hooks/useAuth"
```

> See `references/feature-slice.md` for full rules, cross-feature
> communication patterns, and promotion guidelines.

---

## Naming Conventions

| Type           | Convention        | Example                  |
|----------------|-------------------|--------------------------|
| Components     | PascalCase        | `UserCard.tsx`           |
| Hooks          | useCamelCase      | `useUserData.ts`         |
| Utils          | camelCase         | `formatCurrency.ts`      |
| Types          | PascalCase        | `UserCardProps`          |
| Server Actions | camelCase + Action | `submitFormAction.ts`    |
| Constants      | UPPER_SNAKE_CASE  | `MAX_RETRY_COUNT`        |
| Feature folders| kebab-case        | `user-settings/`         |

---

## When to Create a New Feature

Create a new feature folder when:

- The domain has 2+ components that share state or types
- The code has its own hooks for data fetching or state management
- There are domain-specific utility functions
- The feature has its own route(s) in the app

**Don't create a feature for:**
- A single reusable component — goes in `components/ui/` or `components/shared/`
- A single utility function — goes in `lib/`
- A single type — goes in `types/`

---

## Promoting Code to Shared

When code used in one feature starts being needed in a second:

1. **Component in 1 feature** — stays in `features/*/components/`
2. **Component in 2+ features** — move to `components/shared/`
3. **Hook in 1 feature** — stays in `features/*/hooks/`
4. **Hook in 2+ features** — move to root `hooks/`
5. **Type in 1 feature** — stays in `features/*/types/`
6. **Type in 2+ features** — move to root `types/`

**Rule:** Don't prematurely extract. Wait until you actually need it in
a second place before promoting to shared.

---

## Framework-Specific Rules

### Next.js

- `app/` owns routes, layouts, loading states, and error boundaries
- Server Components are the default — add `"use client"` only when needed
- Server actions live in `features/*/actions/`
- Keep `"use client"` as low in the tree as possible
- Never put `"use client"` on `layout.tsx` or `page.tsx` unless necessary
- Metadata lives in `page.tsx` or `layout.tsx` via the Metadata API

### Vite

- `pages/` holds route-level components
- Use React Router v6+ (Outlet, nested routes)
- Layout components live alongside pages or in a `layouts/` folder
- Lazy load heavy features: `const Feature = lazy(() => import(...))`

---

## Anti-Patterns

| Never do this | Do this instead |
|---------------|-----------------|
| All components in one `components/` folder | Split by ui/ shared/ features/ |
| Feature code scattered across `components/`, `hooks/`, `utils/` | Co-locate in `features/<domain>/` |
| Barrel file re-exports everything | Only export the public API |
| Default exports in barrel files | Named exports only |
| Reaching into another feature's internals | Import from its `index.ts` |
| Premature extraction to shared | Wait until used in 2+ places |
| `src/helpers/`, `src/services/` catch-all folders | Use `lib/` for shared, feature folder for domain |

---

## Example: Scaffolding an E-Commerce Project

```
src/
  app/                          <- Next.js App Router
    (auth)/
      login/page.tsx
      signup/page.tsx
    (shop)/
      products/page.tsx
      products/[id]/page.tsx
      cart/page.tsx
      checkout/page.tsx
    layout.tsx
  components/
    ui/
      Button.tsx
      Input.tsx
      Badge.tsx
      Avatar.tsx
    shared/
      FormField.tsx
      Card.tsx
      Modal.tsx
      DataTable.tsx
  features/
    auth/
      components/
        LoginForm.tsx
        SignupForm.tsx
      hooks/
        useAuth.ts
      actions/
        loginAction.ts
      types/
        auth.types.ts
      index.ts
    products/
      components/
        ProductCard.tsx
        ProductGrid.tsx
      hooks/
        useProducts.ts
      types/
        product.types.ts
      index.ts
    cart/
      components/
        CartDrawer.tsx
        CartItem.tsx
      hooks/
        useCart.ts
      types/
        cart.types.ts
      index.ts
    checkout/
      components/
        CheckoutForm.tsx
        OrderSummary.tsx
      hooks/
        useCheckout.ts
      actions/
        placeOrderAction.ts
      types/
        checkout.types.ts
      index.ts
  hooks/
    useCurrentUser.ts
    useDisclosure.ts
  lib/
    api.ts
    config.ts
  types/
    global.types.ts
```

Each feature is independently understandable. A developer working on
`cart/` doesn't need to read `checkout/` internals — just its public API.
