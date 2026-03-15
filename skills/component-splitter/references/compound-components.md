# Compound Component Pattern

Build complex, flexible widgets (Tabs, Accordion, Modal, Select, Stepper)
using internal React Context and composable sub-components.

---

## When to Use

Use the compound component pattern when:

- Building a widget with **multiple cooperating parts** (trigger + panel, header + body)
- Users need to **customize layout and content** of sub-parts
- The component needs **shared internal state** not exposed to consumers
- You're building: Tabs, Accordion, Dropdown, Modal, Select, Stepper,
  Sidebar, NavigationMenu, AlertDialog

**Don't use when:**
- A simple component with props suffices
- There's only one child element
- No shared state between parts

---

## Architecture

```
components/
  ui/
    Tabs/
      Tabs.tsx           ← root provider + static sub-components
      TabsContext.tsx     ← internal context (not exported)
      TabsList.tsx        ← sub-component
      TabsTrigger.tsx     ← sub-component
      TabsContent.tsx     ← sub-component
      index.ts           ← barrel export
```

---

## Full Example: Tabs

### Internal Context

```tsx
// components/ui/Tabs/TabsContext.tsx
import { createContext, useContext } from "react"

interface TabsContextValue {
  activeTab: string
  setActiveTab: (id: string) => void
}

const TabsContext = createContext<TabsContextValue | null>(null)

export function useTabsContext() {
  const context = useContext(TabsContext)
  if (!context) {
    throw new Error("Tabs compound components must be used within <Tabs>")
  }
  return context
}

export { TabsContext }
```

### Root Component

```tsx
// components/ui/Tabs/Tabs.tsx
import { useState, type ReactNode } from "react"
import { TabsContext } from "./TabsContext"
import { TabsList } from "./TabsList"
import { TabsTrigger } from "./TabsTrigger"
import { TabsContent } from "./TabsContent"

interface TabsProps {
  defaultValue: string
  value?: string
  onValueChange?: (value: string) => void
  children: ReactNode
  className?: string
}

function TabsRoot({ defaultValue, value, onValueChange, children, className }: TabsProps) {
  const [internalValue, setInternalValue] = useState(defaultValue)

  const activeTab = value ?? internalValue
  const setActiveTab = (id: string) => {
    if (!value) setInternalValue(id)
    onValueChange?.(id)
  }

  return (
    <TabsContext.Provider value={{ activeTab, setActiveTab }}>
      <div className={className}>{children}</div>
    </TabsContext.Provider>
  )
}

// Attach sub-components as static properties
export const Tabs = Object.assign(TabsRoot, {
  List: TabsList,
  Trigger: TabsTrigger,
  Content: TabsContent,
})
```

### Sub-Components

```tsx
// components/ui/Tabs/TabsList.tsx
import type { ReactNode } from "react"

interface TabsListProps {
  children: ReactNode
  className?: string
}

export function TabsList({ children, className }: TabsListProps) {
  return (
    <div role="tablist" className={cn("flex border-b", className)}>
      {children}
    </div>
  )
}
```

```tsx
// components/ui/Tabs/TabsTrigger.tsx
import { useTabsContext } from "./TabsContext"

interface TabsTriggerProps {
  value: string
  children: React.ReactNode
  disabled?: boolean
  className?: string
}

export function TabsTrigger({ value, children, disabled, className }: TabsTriggerProps) {
  const { activeTab, setActiveTab } = useTabsContext()
  const isActive = activeTab === value

  return (
    <button
      role="tab"
      aria-selected={isActive}
      aria-controls={`tabpanel-${value}`}
      disabled={disabled}
      onClick={() => setActiveTab(value)}
      className={cn(
        "px-4 py-2 text-sm font-medium transition-colors",
        isActive
          ? "border-b-2 border-primary text-primary"
          : "text-muted-foreground hover:text-foreground",
        disabled && "opacity-50 cursor-not-allowed",
        className
      )}
    >
      {children}
    </button>
  )
}
```

```tsx
// components/ui/Tabs/TabsContent.tsx
import { useTabsContext } from "./TabsContext"

interface TabsContentProps {
  value: string
  children: React.ReactNode
  className?: string
}

export function TabsContent({ value, children, className }: TabsContentProps) {
  const { activeTab } = useTabsContext()

  if (activeTab !== value) return null

  return (
    <div
      role="tabpanel"
      id={`tabpanel-${value}`}
      className={cn("py-4", className)}
    >
      {children}
    </div>
  )
}
```

### Barrel Export

```tsx
// components/ui/Tabs/index.ts
export { Tabs } from "./Tabs"
export type { TabsProps } from "./Tabs"
```

### Usage

```tsx
import { Tabs } from "@/components/ui/Tabs"

function SettingsPage() {
  return (
    <Tabs defaultValue="general">
      <Tabs.List>
        <Tabs.Trigger value="general">General</Tabs.Trigger>
        <Tabs.Trigger value="security">Security</Tabs.Trigger>
        <Tabs.Trigger value="notifications">Notifications</Tabs.Trigger>
      </Tabs.List>

      <Tabs.Content value="general">
        <GeneralSettings />
      </Tabs.Content>
      <Tabs.Content value="security">
        <SecuritySettings />
      </Tabs.Content>
      <Tabs.Content value="notifications">
        <NotificationSettings />
      </Tabs.Content>
    </Tabs>
  )
}
```

---

## Controlled vs Uncontrolled

Always support both patterns:

```tsx
// Uncontrolled — component manages its own state
<Tabs defaultValue="general">...</Tabs>

// Controlled — parent manages state
const [tab, setTab] = useState("general")
<Tabs value={tab} onValueChange={setTab}>...</Tabs>
```

The pattern: if `value` prop is provided, use it (controlled). Otherwise,
use internal state initialized from `defaultValue` (uncontrolled).

---

## Accessibility Checklist

- [ ] Root container has appropriate `role` (if applicable)
- [ ] Triggers have `role="tab"` and `aria-selected`
- [ ] Panels have `role="tabpanel"` with matching `id`
- [ ] Triggers reference panels via `aria-controls`
- [ ] Keyboard navigation: arrow keys between triggers, Enter/Space to activate
- [ ] Disabled triggers have `aria-disabled` and `disabled`
- [ ] Focus management: active trigger is focusable, inactive are in tab order

---

## Applying to Other Widgets

The same pattern works for:

| Widget | Root | Trigger | Content |
|--------|------|---------|---------|
| Tabs | `<Tabs>` | `<Tabs.Trigger>` | `<Tabs.Content>` |
| Accordion | `<Accordion>` | `<Accordion.Header>` | `<Accordion.Panel>` |
| Select | `<Select>` | `<Select.Trigger>` | `<Select.Option>` |
| Modal | `<Modal>` | `<Modal.Trigger>` | `<Modal.Content>` |
| Stepper | `<Stepper>` | `<Stepper.Step>` | `<Stepper.Panel>` |

Each follows the same internal structure:
1. Context for shared state
2. Root component as provider
3. Sub-components consuming context
4. Static property attachment for ergonomic API
5. Barrel export from the component folder
