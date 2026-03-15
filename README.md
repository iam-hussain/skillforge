# Skillforge

Opinionated, production-grade coding skills for AI assistants.

Works with **Claude Code**, **Cursor**, **Windsurf**, **GitHub Copilot**, **Cline**, **Aider**, and any LLM-based coding tool.

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

## Quick Start

All skills can be installed via the Claude Code marketplace or manually via curl. Pick a skill below for tool-specific instructions.

```bash
# Claude Code — install any skill in one command
/plugin marketplace add iam-hussain/skillforge
/plugin install <skill-name>-plugin
```

---

## Skills

### react-component-splitter

Split monolithic React components into well-structured, reusable pieces. Covers hook extraction, compound components, container/presentational splits, and server/client separation.

**When to use:** A React component is too large (150+ lines), has 3+ useState calls, mixes fetch logic with rendering, or has deeply nested JSX.

**References included:**
- `hooks-patterns.md` — hook extraction patterns for state, data fetching, forms, and toggles
- `compound-components.md` — Tabs/Accordion/Modal implementation with React Context

<details>
<summary><strong>Install &amp; Use</strong></summary>

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

Paste `SKILL.md` contents into **Custom Instructions** in extension settings, or add to `.clinerules` in your project root.

#### Aider

```bash
aider --read skills/react-component-splitter/SKILL.md
```

Or add to `.aider.conf.yml`:

```yaml
read:
  - skills/react-component-splitter/SKILL.md
```

#### Manual

```bash
curl -o SKILL.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-component-splitter/SKILL.md
```

Include the reference files for full coverage:

```bash
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
<summary><strong>Install &amp; Use</strong></summary>

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

Paste `SKILL.md` contents into **Custom Instructions** in extension settings, or add to `.clinerules` in your project root.

#### Aider

```bash
aider --read skills/react-project-structure/SKILL.md
```

Or add to `.aider.conf.yml`:

```yaml
read:
  - skills/react-project-structure/SKILL.md
```

#### Manual

```bash
curl -o SKILL.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-project-structure/SKILL.md
```

Include the reference files for full coverage:

```bash
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
<summary><strong>Install &amp; Use</strong></summary>

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

Paste `SKILL.md` contents into **Custom Instructions** in extension settings, or add to `.clinerules` in your project root.

#### Aider

```bash
aider --read skills/shadcn-component-system/SKILL.md
```

Or add to `.aider.conf.yml`:

```yaml
read:
  - skills/shadcn-component-system/SKILL.md
```

#### Manual

```bash
curl -o SKILL.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/shadcn-component-system/SKILL.md
```

Include the reference files for full coverage:

```bash
curl -o cva-patterns.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/shadcn-component-system/references/cva-patterns.md
curl -o shadcn-extension.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/shadcn-component-system/references/shadcn-extension.md
```

</details>

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
