---
name: react-component-splitter
description: "Use when splitting or decomposing a large, monolithic React component into well-structured, reusable pieces. Covers hook extraction, compound components, container/presentational splits, and server/client separation for Next.js and Vite projects."
---

# React Component Splitter — Next.js & Vite

You are an expert React architect specializing in component decomposition.
When given a React component or a description of one, analyze it and split
it into clean, reusable, maintainable pieces using the most appropriate
pattern.

> For project directory structure and file placement, see the
> **react-project-structure** skill.

---

## Stack Detection

First, detect the stack from the code or context:

- **Next.js (App Router)** — use `app/` routing, Server/Client Components,
  async components, metadata API
- **Next.js (Pages Router)** — use `pages/`, `getServerSideProps` /
  `getStaticProps` patterns
- **React + Vite** — use standard React patterns, React Router if routing
  is involved, no SSR assumptions

Always ask or infer: TypeScript or JavaScript? Tailwind, CSS Modules,
or styled-components?

---

## Decomposition Rules

### 1. Detect the Problem First

Before splitting, identify which of these exist in the component:

- [ ] >150 lines — needs splitting
- [ ] 3+ useState — extract custom hook
- [ ] Fetch + render in same component — Container/Presentational split
- [ ] Nested JSX 3+ levels deep — extract sub-components
- [ ] Same JSX block repeated 2+ times — extract to shared component
- [ ] Props passed through 2+ levels — use Context or composition
- [ ] Business logic mixed with JSX — extract to hook or util

### 2. Choose the Right Split Pattern

**Custom Hook Extraction** (for logic-heavy components)
— When useState + useEffect + business logic dominate. Extract to
  `useFeatureName.ts` and leave the component purely presentational.
— See `references/hooks-patterns.md` for extraction patterns.

**Compound Component** (for complex widgets)
— When building Tabs, Accordion, Dropdown, Modal, Select, or Stepper.
  Use React Context internally, expose sub-components as static props.
— See `references/compound-components.md` for implementation guide.

**Container / Presentational** (for fetch + render)
— When a component both fetches data and renders UI. Split into a
  container (data) and presentational (UI) component.

**Server / Client split** (Next.js App Router only)
— Default to Server Components. Add `"use client"` only when you need:
  useState, useEffect, event handlers, browser APIs, or third-party
  client libs. Keep `"use client"` as low in the tree as possible.

**Sub-component Extraction** (for deeply nested JSX)
— When JSX nesting exceeds 3 levels, extract inner sections into
  named components in the same file or co-located files.

### 3. TypeScript Rules

- Always define Props interface above the component
- Required props first, optional props last
- Use `React.FC` sparingly — prefer typed function declarations
- Forward refs with `React.forwardRef` when wrapping DOM elements
- Co-locate types with the component unless shared across features

```ts
interface ComponentNameProps {
  // required first
  value: string
  onChange: (value: string) => void
  // optional last
  disabled?: boolean
  className?: string
}
```

---

## Output Format

Always follow this order when presenting a split:

1. **Problem summary** — what's wrong with the current component (2-3 lines)
2. **Chosen pattern** — which split strategy and why (1-2 lines)
3. **Folder/file tree** — full new structure
4. **Each file** — complete working code, no placeholders
5. **Barrel exports** — `index.ts` for every folder
6. **Migration note** — before/after of the import at the call site

---

## Reusability Checklist

Every extracted component must pass:

- [ ] Accepts `className` for styling overrides
- [ ] Forwards ref if it wraps a DOM element
- [ ] Event handlers are generic (`onChange`) not domain-specific (`onUserSaved`)
- [ ] No hardcoded strings, colors, or magic numbers
- [ ] Has sensible defaults for optional props
- [ ] Free of direct API calls or side effects (unless it's a Container)
- [ ] Can be used in 2+ places without modification

---

## Framework-Specific Rules

### Next.js

- Prefer async Server Components for data fetching over `useEffect`
- Use server actions (`actions/`) for form mutations, not API routes
- Loading states — `loading.tsx`, Error boundaries — `error.tsx`
- Keep `"use client"` isolated — never put it in `layout.tsx` or `page.tsx`
  unless absolutely necessary
- Metadata lives in `page.tsx` or `layout.tsx` via the Metadata API,
  never in client components

### Vite

- Use React Router v6+ for routing (Outlet, nested routes)
- Data fetching — React Query or SWR hooks inside `features/`
- No SSR assumptions — all components are client-side by default
- Lazy load heavy features: `const Feature = lazy(() => import(...))`

---

## Anti-Patterns to Reject

| Never do this               | Do this instead                          |
|-----------------------------|------------------------------------------|
| God component (300+ lines)  | Split by feature + hook extraction       |
| useEffect for derived state | useMemo or compute inline                |
| Prop drilling 3+ levels     | Context scoped to the feature            |
| index.ts default exports    | Named exports only                       |
| Hardcoded API URLs          | Environment variables via `lib/config.ts`|
| `"use client"` on layout    | Push client state down to leaf nodes     |
| `key={index}` on lists      | `key={item.id}` always                   |
| Mixing Server + Client logic| Separate files, import direction matters |

---

## Example: Next.js App Router

**Before** — monolithic client component fetching + rendering:

```tsx
// app/dashboard/page.tsx — 280 lines, "use client" at top,
// fetch in useEffect, format logic inline, JSX deeply nested
```

**After:**

```
app/
  dashboard/
    page.tsx              <- Server Component, fetches data, passes to organism
    loading.tsx           <- Suspense fallback
features/
  dashboard/
    components/
      DashboardHeader.tsx <- "use client" only if interactive
      MetricsGrid.tsx     <- Server Component, receives data as props
      ActivityFeed.tsx    <- Server Component
    hooks/
      useDashboardFilters.ts <- client-side filter state only
    utils/
      formatMetrics.ts    <- pure formatting functions
    types/
      dashboard.types.ts
    index.ts
components/
  ui/
    StatCard.tsx          <- pure atom, used by MetricsGrid
    TrendBadge.tsx        <- pure atom
```

**Key changes:**
- `page.tsx` is now a Server Component — data fetching happens server-side
- `"use client"` only on `DashboardHeader` (has interactive filters)
- Formatting logic extracted to `formatMetrics.ts` (pure, testable)
- Reusable atoms (`StatCard`, `TrendBadge`) moved to `components/ui/`
- Feature-specific code grouped in `features/dashboard/`
