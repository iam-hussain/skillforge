# Skillforge

Opinionated, production-grade coding skills for AI assistants.

Works with **Claude Code**, **Cursor**, **Windsurf**, **GitHub Copilot**, **Cline**, **Continue**, **Aider**, **Zed**, **Codex CLI**, **Gemini CLI**, **Junie**, **Goose**, and any LLM-based coding tool.

---

## What is Skillforge?

Skillforge is a curated collection of engineering skills — structured prompts that teach AI coding assistants how to perform specific tasks the way a senior engineer would. Each skill is a standalone markdown file that works with any tool that accepts system prompts or project context.

**Key principles:**
- **Opinionated** — clear recommendations, not option lists
- **Stack-aware** — Next.js App Router, Vite, Node.js, TypeScript
- **Production-grade** — real patterns from real codebases
- **LLM-agnostic** — works with any AI coding assistant

---

## Available Skills

| Skill | Description | Tags |
|-------|-------------|------|
| [react-component-splitter](#react-component-splitter) | Split monolithic React components using hook extraction, compound components, and container/presentational patterns | `react` `next.js` `vite` `typescript` |
| [react-project-structure](#react-project-structure) | Scaffold and organize React project directory structure using Feature-Slice and Atomic Design patterns | `react` `next.js` `vite` `typescript` |
| [shadcn-component-system](#shadcn-component-system) | Build variant-driven, theme-synced UI components using Shadcn UI and CVA with atomic decomposition | `react` `shadcn` `cva` `tailwind` `typescript` |

---

## Skills

### react-component-splitter

Split monolithic React components into well-structured, reusable pieces. Covers hook extraction, compound components, container/presentational splits, and server/client separation.

**When to use:** A React component is too large (150+ lines), has 3+ useState calls, mixes fetch logic with rendering, or has deeply nested JSX.

**References included:**
- `hooks-patterns.md` — hook extraction patterns for state, data fetching, forms, and toggles
- `compound-components.md` — Tabs/Accordion/Modal implementation with React Context

<details>
<summary><strong>How to Use</strong></summary>

Once installed (see install instructions below), the skill is automatically loaded into your AI assistant's context. You don't need to reference it manually — just describe what you need and the AI will follow the skill's patterns.

**Example prompts you can use with any AI tool:**

```
Split the UserDashboard component — it's 400 lines with 6 useState hooks
and mixes data fetching with rendering.
```

```
This ProductCard component has nested JSX 5 levels deep. Decompose it
into clean sub-components.
```

```
Extract the form validation logic from SignupForm.tsx into a custom hook.
```

```
Convert this monolithic Tabs component into a compound component pattern
with internal context.
```

```
Split this Next.js client component into server and client parts —
only the search bar needs interactivity.
```

**What to expect:** The AI will analyze your component, identify which decomposition pattern fits (hook extraction, compound component, container/presentational, server/client split, or sub-component extraction), then produce complete code for every new file with barrel exports and a migration note showing how imports change at the call site.

</details>

<details>
<summary><strong>Install</strong></summary>

#### Claude Code

```bash
/plugin marketplace add iam-hussain/skillforge
/plugin install react-component-splitter-plugin
```

#### Cursor

```bash
mkdir -p .cursor/rules
curl -o .cursor/rules/react-component-splitter.mdc \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-component-splitter/SKILL.md
```

Or paste `SKILL.md` contents into **Cursor Settings > Rules for AI**.

#### Windsurf

```bash
curl -o .windsurfrules \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-component-splitter/SKILL.md
```

Or paste `SKILL.md` contents into **Windsurf > AI Rules**.

#### GitHub Copilot

```bash
mkdir -p .github
curl -o .github/copilot-instructions.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-component-splitter/SKILL.md
```

#### Cline / Roo Code

```bash
curl -o .clinerules \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-component-splitter/SKILL.md
```

Or paste `SKILL.md` contents into **Custom Instructions** in extension settings.

#### Continue

```bash
mkdir -p .continue/rules
curl -o .continue/rules/react-component-splitter.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-component-splitter/SKILL.md
```

Or add to `.continuerules` in your project root.

#### Aider

```bash
aider --read skills/react-component-splitter/SKILL.md
```

Or add to `.aider.conf.yml`:

```yaml
read:
  - skills/react-component-splitter/SKILL.md
```

#### Zed

```bash
curl -o .rules \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-component-splitter/SKILL.md
```

Or add to your **Rules Library** in Zed's Agent Panel settings.

#### Codex CLI (OpenAI)

```bash
curl -o AGENTS.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-component-splitter/SKILL.md
```

Or append to an existing `AGENTS.md` in your project root.

#### Gemini CLI

```bash
curl -o GEMINI.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-component-splitter/SKILL.md
```

Or append to `~/.gemini/GEMINI.md` for global use.

#### JetBrains AI / Junie

```bash
mkdir -p .junie
curl -o .junie/guidelines.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-component-splitter/SKILL.md
```

Or paste into **Settings > Tools > Junie > Project Settings > Guidelines**.

#### Goose

```bash
curl -o .goosehints \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-component-splitter/SKILL.md
```

Or append to an existing `.goosehints` file.

#### ChatGPT / Gemini / Any Chat LLM

Copy the contents of [`SKILL.md`](./skills/react-component-splitter/SKILL.md) and paste into your system prompt, custom instructions, or project context. Then paste your component code and ask the AI to split it.

#### Manual (curl)

```bash
# Skill file
curl -o SKILL.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-component-splitter/SKILL.md

# Reference files
curl -o hooks-patterns.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-component-splitter/references/hooks-patterns.md
curl -o compound-components.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-component-splitter/references/compound-components.md
```

</details>

---

### react-project-structure

Scaffold and organize React project directory structure using Feature-Slice architecture and Atomic Design component hierarchy.

**When to use:** Starting a new React project, reorganizing an existing codebase, or deciding where a new component/feature should live.

**References included:**
- `feature-slice.md` — feature folder rules, cross-feature communication, code promotion
- `atomic-design.md` — atoms/molecules/organisms/templates/pages mapping to React

<details>
<summary><strong>How to Use</strong></summary>

Once installed (see install instructions below), the skill is automatically loaded into your AI assistant's context. You don't need to reference it manually — just describe what you need and the AI will follow the skill's patterns.

**Example prompts you can use with any AI tool:**

```
Scaffold a new Next.js App Router project structure for an e-commerce
platform with auth, products, cart, and checkout features.
```

```
I have a flat src/components/ folder with 40+ components. Reorganize
it using Feature-Slice architecture with atomic design hierarchy.
```

```
Where should I put a new PaymentMethodSelector component? It's used
only in the checkout feature right now.
```

```
Set up the folder structure for a new "notifications" feature in my
Vite + React project. It needs components, hooks, types, and utils.
```

```
My SearchBar component is currently in features/products/components/
but now I need it in the header and the admin panel too. How should
I promote it?
```

**What to expect:** The AI will detect your stack (Next.js App Router, Pages Router, or Vite), then produce a complete directory tree with file placements following Feature-Slice architecture. Components are placed at the correct atomic level (atoms in `components/ui/`, molecules in `components/shared/`, organisms in `features/*/components/`). Barrel exports, naming conventions, and import rules are enforced automatically.

</details>

<details>
<summary><strong>Install</strong></summary>

#### Claude Code

```bash
/plugin marketplace add iam-hussain/skillforge
/plugin install react-project-structure-plugin
```

#### Cursor

```bash
mkdir -p .cursor/rules
curl -o .cursor/rules/react-project-structure.mdc \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-project-structure/SKILL.md
```

Or paste `SKILL.md` contents into **Cursor Settings > Rules for AI**.

#### Windsurf

```bash
curl -o .windsurfrules \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-project-structure/SKILL.md
```

Or paste `SKILL.md` contents into **Windsurf > AI Rules**.

#### GitHub Copilot

```bash
mkdir -p .github
curl -o .github/copilot-instructions.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-project-structure/SKILL.md
```

#### Cline / Roo Code

```bash
curl -o .clinerules \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-project-structure/SKILL.md
```

Or paste `SKILL.md` contents into **Custom Instructions** in extension settings.

#### Continue

```bash
mkdir -p .continue/rules
curl -o .continue/rules/react-project-structure.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-project-structure/SKILL.md
```

Or add to `.continuerules` in your project root.

#### Aider

```bash
aider --read skills/react-project-structure/SKILL.md
```

Or add to `.aider.conf.yml`:

```yaml
read:
  - skills/react-project-structure/SKILL.md
```

#### Zed

```bash
curl -o .rules \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-project-structure/SKILL.md
```

Or add to your **Rules Library** in Zed's Agent Panel settings.

#### Codex CLI (OpenAI)

```bash
curl -o AGENTS.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-project-structure/SKILL.md
```

Or append to an existing `AGENTS.md` in your project root.

#### Gemini CLI

```bash
curl -o GEMINI.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-project-structure/SKILL.md
```

Or append to `~/.gemini/GEMINI.md` for global use.

#### JetBrains AI / Junie

```bash
mkdir -p .junie
curl -o .junie/guidelines.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-project-structure/SKILL.md
```

Or paste into **Settings > Tools > Junie > Project Settings > Guidelines**.

#### Goose

```bash
curl -o .goosehints \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-project-structure/SKILL.md
```

Or append to an existing `.goosehints` file.

#### ChatGPT / Gemini / Any Chat LLM

Copy the contents of [`SKILL.md`](./skills/react-project-structure/SKILL.md) and paste into your system prompt, custom instructions, or project context. Then describe your project and ask the AI to scaffold or reorganize the structure.

#### Manual (curl)

```bash
# Skill file
curl -o SKILL.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-project-structure/SKILL.md

# Reference files
curl -o feature-slice.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-project-structure/references/feature-slice.md
curl -o atomic-design.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-project-structure/references/atomic-design.md
```

</details>

---

### shadcn-component-system

Build variant-driven, theme-synced UI components using Shadcn UI and class-variance-authority (CVA) with atomic decomposition and prop-driven scalability.

**When to use:** Building or extending UI components with Shadcn UI, creating a design system with CVA variants, enforcing semantic theming with CSS variables, or decomposing complex UI into atoms/molecules/organisms.

**References included:**
- `cva-patterns.md` — variant schemas, compound variants, state variants, composition patterns
- `shadcn-extension.md` — 4 patterns for wrapping and extending Shadcn primitives without forking

<details>
<summary><strong>How to Use</strong></summary>

Once installed (see install instructions below), the skill is automatically loaded into your AI assistant's context. You don't need to reference it manually — just describe what you need and the AI will follow the skill's patterns.

**Example prompts you can use with any AI tool:**

```
Build a StatusBadge component with CVA variants for success, warning,
error, info, and neutral states. It should support sm/md/lg sizes and
an optional dot indicator.
```

```
I need a data table component that extends Shadcn's Table with loading
skeletons, empty state, striped rows, and compact/relaxed density variants.
```

```
Create a ConfirmDialog molecule that wraps Shadcn's AlertDialog with
a simpler API — just trigger, title, description, and onConfirm.
```

```
This component uses className={isActive ? 'bg-blue-500' : 'bg-gray-200'}.
Refactor it to use CVA variants with semantic theme tokens instead of
raw Tailwind colors.
```

```
Decompose this dashboard card design into atoms (Badge, Avatar),
a molecule (StatCard), and an organism (MetricsGrid). Use CVA for
all visual variants and Shadcn primitives where they exist.
```

```
I need an IconButton that extends Shadcn's Button with color variants
(default, primary, destructive) and a rounded option. Don't modify
the original Button source.
```

**What to expect:** The AI will first check if Shadcn primitives exist for the requirement, then decompose the UI into atoms/molecules/organisms. Every component gets a CVA variant schema (no inline conditional classes), uses semantic theme tokens (no raw colors like `blue-500`), preserves native HTML attributes via `ComponentPropsWithoutRef`, and forwards refs. Output includes complete TypeScript code, CVA schema, file placement, and usage examples.

</details>

<details>
<summary><strong>Install</strong></summary>

#### Claude Code

```bash
/plugin marketplace add iam-hussain/skillforge
/plugin install shadcn-component-system-plugin
```

#### Cursor

```bash
mkdir -p .cursor/rules
curl -o .cursor/rules/shadcn-component-system.mdc \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/shadcn-component-system/SKILL.md
```

Or paste `SKILL.md` contents into **Cursor Settings > Rules for AI**.

#### Windsurf

```bash
curl -o .windsurfrules \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/shadcn-component-system/SKILL.md
```

Or paste `SKILL.md` contents into **Windsurf > AI Rules**.

#### GitHub Copilot

```bash
mkdir -p .github
curl -o .github/copilot-instructions.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/shadcn-component-system/SKILL.md
```

#### Cline / Roo Code

```bash
curl -o .clinerules \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/shadcn-component-system/SKILL.md
```

Or paste `SKILL.md` contents into **Custom Instructions** in extension settings.

#### Continue

```bash
mkdir -p .continue/rules
curl -o .continue/rules/shadcn-component-system.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/shadcn-component-system/SKILL.md
```

Or add to `.continuerules` in your project root.

#### Aider

```bash
aider --read skills/shadcn-component-system/SKILL.md
```

Or add to `.aider.conf.yml`:

```yaml
read:
  - skills/shadcn-component-system/SKILL.md
```

#### Zed

```bash
curl -o .rules \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/shadcn-component-system/SKILL.md
```

Or add to your **Rules Library** in Zed's Agent Panel settings.

#### Codex CLI (OpenAI)

```bash
curl -o AGENTS.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/shadcn-component-system/SKILL.md
```

Or append to an existing `AGENTS.md` in your project root.

#### Gemini CLI

```bash
curl -o GEMINI.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/shadcn-component-system/SKILL.md
```

Or append to `~/.gemini/GEMINI.md` for global use.

#### JetBrains AI / Junie

```bash
mkdir -p .junie
curl -o .junie/guidelines.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/shadcn-component-system/SKILL.md
```

Or paste into **Settings > Tools > Junie > Project Settings > Guidelines**.

#### Goose

```bash
curl -o .goosehints \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/shadcn-component-system/SKILL.md
```

Or append to an existing `.goosehints` file.

#### ChatGPT / Gemini / Any Chat LLM

Copy the contents of [`SKILL.md`](./skills/shadcn-component-system/SKILL.md) and paste into your system prompt, custom instructions, or project context. Then describe the component you need and the AI will build it following CVA + atomic design patterns.

#### Manual (curl)

```bash
# Skill file
curl -o SKILL.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/shadcn-component-system/SKILL.md

# Reference files
curl -o cva-patterns.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/shadcn-component-system/references/cva-patterns.md
curl -o shadcn-extension.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/shadcn-component-system/references/shadcn-extension.md
```

</details>

---

## Supported Tools

Skillforge skills work with any AI coding tool that accepts system prompts or custom instructions. Here's the full compatibility matrix:

| Tool | Instructions File | How It Loads |
|------|-------------------|--------------|
| **Claude Code** | Plugin marketplace | Auto — loaded on install via `/plugin install` |
| **Cursor** | `.cursor/rules/<skill>.mdc` | Auto — rules are loaded when working in the project |
| **Windsurf** | `.windsurfrules` | Auto — loaded when Windsurf opens the project |
| **GitHub Copilot** | `.github/copilot-instructions.md` | Auto — Copilot reads this file for every chat/edit |
| **Cline / Roo Code** | `.clinerules` | Auto — loaded when Cline starts in the project |
| **Continue** | `.continue/rules/<skill>.md` | Auto — rules are injected into Agent, Chat, and Edit modes |
| **Aider** | `.aider.conf.yml` → `read:` | Auto — loaded on every Aider session start |
| **Zed** | `.rules` | Auto — injected into every Agent Panel interaction |
| **Codex CLI** | `AGENTS.md` | Auto — Codex reads this file at the start of every run |
| **Gemini CLI** | `GEMINI.md` | Auto — Gemini loads this with every prompt |
| **JetBrains / Junie** | `.junie/guidelines.md` | Auto — Junie adds guidelines to every task |
| **Goose** | `.goosehints` | Auto — Goose reads hints when starting in the directory |
| **ChatGPT / Gemini / Any LLM** | Paste into prompt | Manual — paste `SKILL.md` contents into system prompt or custom instructions, then ask your question |

**All tools except chat LLMs load skills automatically** — once the file is in place, every conversation in that project benefits from the skill without you needing to reference it.

---

## Project Structure

```
skillforge/
  skills/                           <- one folder per skill
    react-component-splitter/
      SKILL.md                      <- main skill prompt
      references/
        hooks-patterns.md
        compound-components.md
    react-project-structure/
      SKILL.md
      references/
        feature-slice.md
        atomic-design.md
    shadcn-component-system/
      SKILL.md
      references/
        cva-patterns.md
        shadcn-extension.md
  .claude-plugin/
    marketplace.json                <- Claude Code marketplace catalog
  plugin.json                       <- plugin manifest
  CLAUDE.md                         <- repo context
  CONTRIBUTING.md                   <- contribution guide
```

---

## Skill Anatomy

Each skill follows this structure:

```
skills/<skill-name>/
  SKILL.md                     <- main prompt (under 500 lines)
  references/                  <- detailed reference docs
    pattern-name.md
```

`SKILL.md` contains YAML frontmatter (`name`, `description`) and the full prompt. Reference files provide deep-dive documentation that the main prompt links to. Keep `SKILL.md` as the orchestration layer — move anything over ~100 lines into `references/`.

---

## Roadmap

**Coming soon:** design-tokens, form-architecture, state-management, api-design, auth-patterns, and more.

See the full roadmap in [CLAUDE.md](./CLAUDE.md#skill-roadmap).

---

## Contributing

See [CONTRIBUTING.md](./CONTRIBUTING.md) for details on how to add a new skill.

---

## License

MIT
