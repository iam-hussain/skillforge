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

Copy the contents of [`SKILL.md`](./skills/react-component-splitter/SKILL.md) and paste into your system prompt, custom instructions, or project context.

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

Copy the contents of [`SKILL.md`](./skills/react-project-structure/SKILL.md) and paste into your system prompt, custom instructions, or project context.

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

Copy the contents of [`SKILL.md`](./skills/shadcn-component-system/SKILL.md) and paste into your system prompt, custom instructions, or project context.

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

| Tool | Instructions File | Method |
|------|-------------------|--------|
| **Claude Code** | Plugin marketplace | `/plugin install` |
| **Cursor** | `.cursor/rules/<skill>.mdc` | Project rule file |
| **Windsurf** | `.windsurfrules` | Project rule file |
| **GitHub Copilot** | `.github/copilot-instructions.md` | Custom instruction file |
| **Cline / Roo Code** | `.clinerules` | Project rule file |
| **Continue** | `.continue/rules/<skill>.md` | Project rule file |
| **Aider** | `.aider.conf.yml` → `read:` | Context file |
| **Zed** | `.rules` | Project rule file |
| **Codex CLI** | `AGENTS.md` | Project instruction file |
| **Gemini CLI** | `GEMINI.md` | Project context file |
| **JetBrains / Junie** | `.junie/guidelines.md` | Project guideline file |
| **Goose** | `.goosehints` | Project hints file |
| **ChatGPT / Gemini / Any LLM** | Paste into prompt | System prompt / custom instructions |

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
