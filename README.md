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
| [centralized-config](#centralized-config) | Architect centralized monorepo configuration with ESLint, TypeScript, Tailwind presets, and Turborepo pipelines | `monorepo` `turborepo` `eslint` `typescript` `tailwind` `config` |

---

## Installation Guide

Pick your AI coding tool below. Each section shows how to install **all skills** with exact file paths.

> **Tip:** You can install multiple skills in the same project. Most tools support multiple rule files or allow you to concatenate them.

### Claude Code

Use the plugin marketplace to install skills directly:

```bash
# Step 1: Add the Skillforge marketplace (one-time)
/plugin marketplace add iam-hussain/skillforge

# Step 2: Install the skills you need
/plugin install react-component-splitter-plugin
/plugin install react-project-structure-plugin
/plugin install shadcn-component-system-plugin
/plugin install centralized-config-plugin
```

Skills are loaded automatically in every conversation within your project.

---

### Cursor

Add skills as project rule files in `.cursor/rules/`. Cursor loads all `.mdc` files from this folder automatically.

```bash
mkdir -p .cursor/rules

# react-component-splitter
curl -o .cursor/rules/react-component-splitter.mdc \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-component-splitter/SKILL.md

# react-project-structure
curl -o .cursor/rules/react-project-structure.mdc \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-project-structure/SKILL.md

# shadcn-component-system
curl -o .cursor/rules/shadcn-component-system.mdc \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/shadcn-component-system/SKILL.md

# centralized-config
curl -o .cursor/rules/centralized-config.mdc \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/centralized-config/SKILL.md
```

**Alternative:** Open **Cursor Settings > Rules for AI** and paste the contents of each `SKILL.md` file.

Skills are loaded automatically for every AI interaction in the project.

---

### Windsurf

Windsurf reads `.windsurfrules` from your project root. To use multiple skills, concatenate them into one file:

```bash
# Single skill
curl -o .windsurfrules \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-component-splitter/SKILL.md

# Multiple skills — concatenate into one file
curl -s https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-component-splitter/SKILL.md > .windsurfrules
echo -e "\n---\n" >> .windsurfrules
curl -s https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-project-structure/SKILL.md >> .windsurfrules
echo -e "\n---\n" >> .windsurfrules
curl -s https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/shadcn-component-system/SKILL.md >> .windsurfrules
echo -e "\n---\n" >> .windsurfrules
curl -s https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/centralized-config/SKILL.md >> .windsurfrules
```

**Alternative:** Open **Windsurf > AI Rules** and paste the contents.

Skills are loaded automatically when Windsurf opens the project.

---

### GitHub Copilot

Copilot reads custom instructions from `.github/copilot-instructions.md`. To use multiple skills, concatenate them:

```bash
mkdir -p .github

# Single skill
curl -o .github/copilot-instructions.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-component-splitter/SKILL.md

# Multiple skills — concatenate into one file
curl -s https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-component-splitter/SKILL.md > .github/copilot-instructions.md
echo -e "\n---\n" >> .github/copilot-instructions.md
curl -s https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-project-structure/SKILL.md >> .github/copilot-instructions.md
echo -e "\n---\n" >> .github/copilot-instructions.md
curl -s https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/shadcn-component-system/SKILL.md >> .github/copilot-instructions.md
echo -e "\n---\n" >> .github/copilot-instructions.md
curl -s https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/centralized-config/SKILL.md >> .github/copilot-instructions.md
```

Skills are loaded automatically for every Copilot chat and inline edit.

---

### Cline / Roo Code

Cline reads `.clinerules` from your project root. To use multiple skills, concatenate them:

```bash
# Single skill
curl -o .clinerules \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-component-splitter/SKILL.md

# Multiple skills — concatenate into one file
curl -s https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-component-splitter/SKILL.md > .clinerules
echo -e "\n---\n" >> .clinerules
curl -s https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-project-structure/SKILL.md >> .clinerules
echo -e "\n---\n" >> .clinerules
curl -s https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/shadcn-component-system/SKILL.md >> .clinerules
echo -e "\n---\n" >> .clinerules
curl -s https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/centralized-config/SKILL.md >> .clinerules
```

**Alternative:** Open the Cline/Roo Code extension settings and paste `SKILL.md` contents into **Custom Instructions**.

Skills are loaded automatically when Cline starts in the project.

---

### Continue

Continue loads rule files from `.continue/rules/` in your project. Each skill gets its own `.md` file.

```bash
mkdir -p .continue/rules

# react-component-splitter
curl -o .continue/rules/react-component-splitter.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-component-splitter/SKILL.md

# react-project-structure
curl -o .continue/rules/react-project-structure.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-project-structure/SKILL.md

# shadcn-component-system
curl -o .continue/rules/shadcn-component-system.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/shadcn-component-system/SKILL.md

# centralized-config
curl -o .continue/rules/centralized-config.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/centralized-config/SKILL.md
```

**Alternative:** Add to `.continuerules` in your project root, or reference in `config.yaml`:

```yaml
rules:
  - .continue/rules/react-component-splitter.md
  - .continue/rules/react-project-structure.md
  - .continue/rules/shadcn-component-system.md
  - .continue/rules/centralized-config.md
```

Skills are injected automatically into Agent, Chat, and Edit modes.

---

### Aider

Pass skill files as context using the `--read` flag or `.aider.conf.yml`:

```bash
# Inline — load skills for one session
aider \
  --read skills/react-component-splitter/SKILL.md \
  --read skills/react-project-structure/SKILL.md \
  --read skills/shadcn-component-system/SKILL.md \
  --read skills/centralized-config/SKILL.md
```

**Persistent setup** — add to `.aider.conf.yml` in your project root:

```yaml
read:
  - skills/react-component-splitter/SKILL.md
  - skills/react-project-structure/SKILL.md
  - skills/shadcn-component-system/SKILL.md
  - skills/centralized-config/SKILL.md
```

First, clone the repo or download the skill files:

```bash
git clone https://github.com/iam-hussain/skillforge.git skills-repo
cp skills-repo/skills/react-component-splitter/SKILL.md skills/react-component-splitter/SKILL.md
cp skills-repo/skills/react-project-structure/SKILL.md skills/react-project-structure/SKILL.md
cp skills-repo/skills/shadcn-component-system/SKILL.md skills/shadcn-component-system/SKILL.md
cp skills-repo/skills/centralized-config/SKILL.md skills/centralized-config/SKILL.md
```

Skills are loaded automatically on every Aider session start.

---

### Zed

Zed reads `.rules` from your project root. For multiple skills, concatenate them:

```bash
# Single skill
curl -o .rules \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-component-splitter/SKILL.md

# Multiple skills — concatenate into one file
curl -s https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-component-splitter/SKILL.md > .rules
echo -e "\n---\n" >> .rules
curl -s https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-project-structure/SKILL.md >> .rules
echo -e "\n---\n" >> .rules
curl -s https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/shadcn-component-system/SKILL.md >> .rules
echo -e "\n---\n" >> .rules
curl -s https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/centralized-config/SKILL.md >> .rules
```

**Alternative:** Open the **Rules Library** in Zed's Agent Panel settings and add each skill as a saved rule.

Skills are injected automatically into every Agent Panel interaction.

---

### Codex CLI (OpenAI)

Codex reads `AGENTS.md` from your project root. For multiple skills, concatenate them:

```bash
# Single skill
curl -o AGENTS.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-component-splitter/SKILL.md

# Multiple skills — concatenate into one file
curl -s https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-component-splitter/SKILL.md > AGENTS.md
echo -e "\n---\n" >> AGENTS.md
curl -s https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-project-structure/SKILL.md >> AGENTS.md
echo -e "\n---\n" >> AGENTS.md
curl -s https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/shadcn-component-system/SKILL.md >> AGENTS.md
echo -e "\n---\n" >> AGENTS.md
curl -s https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/centralized-config/SKILL.md >> AGENTS.md
```

**Global setup** — add to `~/.codex/AGENTS.md` to apply across all projects.

Skills are loaded automatically at the start of every Codex run.

---

### Gemini CLI

Gemini reads `GEMINI.md` from your project root. For multiple skills, concatenate them:

```bash
# Single skill
curl -o GEMINI.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-component-splitter/SKILL.md

# Multiple skills — concatenate into one file
curl -s https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-component-splitter/SKILL.md > GEMINI.md
echo -e "\n---\n" >> GEMINI.md
curl -s https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-project-structure/SKILL.md >> GEMINI.md
echo -e "\n---\n" >> GEMINI.md
curl -s https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/shadcn-component-system/SKILL.md >> GEMINI.md
echo -e "\n---\n" >> GEMINI.md
curl -s https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/centralized-config/SKILL.md >> GEMINI.md
```

**Global setup** — add to `~/.gemini/GEMINI.md` to apply across all projects.

**Verify loaded context:** Run `/memory show` in Gemini CLI to confirm the skills are loaded.

Skills are loaded automatically with every prompt.

---

### JetBrains AI / Junie

Junie reads guidelines from `.junie/guidelines.md` or `AGENTS.md` in your project root. For multiple skills, concatenate them:

```bash
mkdir -p .junie

# Single skill
curl -o .junie/guidelines.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-component-splitter/SKILL.md

# Multiple skills — concatenate into one file
curl -s https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-component-splitter/SKILL.md > .junie/guidelines.md
echo -e "\n---\n" >> .junie/guidelines.md
curl -s https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-project-structure/SKILL.md >> .junie/guidelines.md
echo -e "\n---\n" >> .junie/guidelines.md
curl -s https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/shadcn-component-system/SKILL.md >> .junie/guidelines.md
echo -e "\n---\n" >> .junie/guidelines.md
curl -s https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/centralized-config/SKILL.md >> .junie/guidelines.md
```

**Alternative:** Open **Settings > Tools > Junie > Project Settings > Guidelines** and paste the contents.

Skills are added automatically to every Junie task.

---

### Goose

Goose reads `.goosehints` from your project root. For multiple skills, concatenate them:

```bash
# Single skill
curl -o .goosehints \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-component-splitter/SKILL.md

# Multiple skills — concatenate into one file
curl -s https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-component-splitter/SKILL.md > .goosehints
echo -e "\n---\n" >> .goosehints
curl -s https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-project-structure/SKILL.md >> .goosehints
echo -e "\n---\n" >> .goosehints
curl -s https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/shadcn-component-system/SKILL.md >> .goosehints
echo -e "\n---\n" >> .goosehints
curl -s https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/centralized-config/SKILL.md >> .goosehints
```

**Global setup** — add to `~/.config/goose/.goosehints` to apply across all projects.

Skills are loaded automatically when Goose starts in the directory.

---

### ChatGPT

1. Open [ChatGPT](https://chat.openai.com) and go to **Settings > Personalization > Custom instructions**
2. Copy the contents of each `SKILL.md` file and paste into the **"How would you like ChatGPT to respond?"** field
3. Start a new conversation and describe what you need

Skill files:
- [`react-component-splitter/SKILL.md`](./skills/react-component-splitter/SKILL.md)
- [`react-project-structure/SKILL.md`](./skills/react-project-structure/SKILL.md)
- [`shadcn-component-system/SKILL.md`](./skills/shadcn-component-system/SKILL.md)
- [`centralized-config/SKILL.md`](./skills/centralized-config/SKILL.md)

**Alternative:** Create a custom GPT with the skill contents as its instructions for a reusable assistant.

---

### Google Gemini (Web)

1. Open [Gemini](https://gemini.google.com) and start a new conversation
2. In your first message, paste the contents of the `SKILL.md` file(s) followed by your actual request
3. Gemini will use the skill as context for the rest of the conversation

Skill files:
- [`react-component-splitter/SKILL.md`](./skills/react-component-splitter/SKILL.md)
- [`react-project-structure/SKILL.md`](./skills/react-project-structure/SKILL.md)
- [`shadcn-component-system/SKILL.md`](./skills/shadcn-component-system/SKILL.md)
- [`centralized-config/SKILL.md`](./skills/centralized-config/SKILL.md)

**Tip:** Use Gemini's "Gems" feature to create a saved assistant with the skill pre-loaded.

---

### Any Other AI Tool

Every skill is a plain markdown file. If your tool accepts custom instructions, system prompts, or context files, you can use Skillforge skills:

```bash
# Clone the entire repo
git clone https://github.com/iam-hussain/skillforge.git

# Or download individual skill files
curl -o react-component-splitter.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-component-splitter/SKILL.md
curl -o react-project-structure.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-project-structure/SKILL.md
curl -o shadcn-component-system.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/shadcn-component-system/SKILL.md
curl -o centralized-config.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/centralized-config/SKILL.md
```

**Don't forget the reference files** for deeper coverage:

```bash
# react-component-splitter references
curl -o hooks-patterns.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-component-splitter/references/hooks-patterns.md
curl -o compound-components.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-component-splitter/references/compound-components.md

# react-project-structure references
curl -o feature-slice.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-project-structure/references/feature-slice.md
curl -o atomic-design.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-project-structure/references/atomic-design.md

# shadcn-component-system references
curl -o cva-patterns.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/shadcn-component-system/references/cva-patterns.md
curl -o shadcn-extension.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/shadcn-component-system/references/shadcn-extension.md

# centralized-config references
curl -o monorepo-config-patterns.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/centralized-config/references/monorepo-config-patterns.md
curl -o tailwind-preset-protocol.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/centralized-config/references/tailwind-preset-protocol.md
```

Paste the `SKILL.md` contents (and optionally reference files) into your tool's system prompt, project context, or custom instructions field.

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

Once installed (see [Installation Guide](#installation-guide)), the skill is automatically loaded into your AI assistant's context. You don't need to reference it manually — just describe what you need and the AI will follow the skill's patterns.

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

**Claude Code:**
```bash
/plugin install react-component-splitter-plugin
```

**Cursor:**
```bash
mkdir -p .cursor/rules
curl -o .cursor/rules/react-component-splitter.mdc \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-component-splitter/SKILL.md
```

**Windsurf:**
```bash
curl -o .windsurfrules \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-component-splitter/SKILL.md
```

**GitHub Copilot:**
```bash
mkdir -p .github
curl -o .github/copilot-instructions.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-component-splitter/SKILL.md
```

**Cline / Roo Code:**
```bash
curl -o .clinerules \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-component-splitter/SKILL.md
```

**Continue:**
```bash
mkdir -p .continue/rules
curl -o .continue/rules/react-component-splitter.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-component-splitter/SKILL.md
```

**Aider:**
```bash
aider --read skills/react-component-splitter/SKILL.md
```

Or add to `.aider.conf.yml`:
```yaml
read:
  - skills/react-component-splitter/SKILL.md
```

**Zed:**
```bash
curl -o .rules \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-component-splitter/SKILL.md
```

**Codex CLI (OpenAI):**
```bash
curl -o AGENTS.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-component-splitter/SKILL.md
```

**Gemini CLI:**
```bash
curl -o GEMINI.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-component-splitter/SKILL.md
```

**JetBrains AI / Junie:**
```bash
mkdir -p .junie
curl -o .junie/guidelines.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-component-splitter/SKILL.md
```

**Goose:**
```bash
curl -o .goosehints \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-component-splitter/SKILL.md
```

**ChatGPT / Gemini / Any Chat LLM:** Copy the contents of [`SKILL.md`](./skills/react-component-splitter/SKILL.md) and paste into the system prompt or custom instructions.

**Manual (curl) — SKILL.md + all reference files:**
```bash
curl -O https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-component-splitter/SKILL.md
curl -O https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-component-splitter/references/hooks-patterns.md
curl -O https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-component-splitter/references/compound-components.md
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

Once installed (see [Installation Guide](#installation-guide)), the skill is automatically loaded into your AI assistant's context. You don't need to reference it manually — just describe what you need and the AI will follow the skill's patterns.

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

**Claude Code:**
```bash
/plugin install react-project-structure-plugin
```

**Cursor:**
```bash
mkdir -p .cursor/rules
curl -o .cursor/rules/react-project-structure.mdc \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-project-structure/SKILL.md
```

**Windsurf:**
```bash
curl -o .windsurfrules \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-project-structure/SKILL.md
```

**GitHub Copilot:**
```bash
mkdir -p .github
curl -o .github/copilot-instructions.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-project-structure/SKILL.md
```

**Cline / Roo Code:**
```bash
curl -o .clinerules \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-project-structure/SKILL.md
```

**Continue:**
```bash
mkdir -p .continue/rules
curl -o .continue/rules/react-project-structure.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-project-structure/SKILL.md
```

**Aider:**
```bash
aider --read skills/react-project-structure/SKILL.md
```

Or add to `.aider.conf.yml`:
```yaml
read:
  - skills/react-project-structure/SKILL.md
```

**Zed:**
```bash
curl -o .rules \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-project-structure/SKILL.md
```

**Codex CLI (OpenAI):**
```bash
curl -o AGENTS.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-project-structure/SKILL.md
```

**Gemini CLI:**
```bash
curl -o GEMINI.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-project-structure/SKILL.md
```

**JetBrains AI / Junie:**
```bash
mkdir -p .junie
curl -o .junie/guidelines.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-project-structure/SKILL.md
```

**Goose:**
```bash
curl -o .goosehints \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-project-structure/SKILL.md
```

**ChatGPT / Gemini / Any Chat LLM:** Copy the contents of [`SKILL.md`](./skills/react-project-structure/SKILL.md) and paste into the system prompt or custom instructions.

**Manual (curl) — SKILL.md + all reference files:**
```bash
curl -O https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-project-structure/SKILL.md
curl -O https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-project-structure/references/feature-slice.md
curl -O https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/react-project-structure/references/atomic-design.md
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

Once installed (see [Installation Guide](#installation-guide)), the skill is automatically loaded into your AI assistant's context. You don't need to reference it manually — just describe what you need and the AI will follow the skill's patterns.

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

**Claude Code:**
```bash
/plugin install shadcn-component-system-plugin
```

**Cursor:**
```bash
mkdir -p .cursor/rules
curl -o .cursor/rules/shadcn-component-system.mdc \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/shadcn-component-system/SKILL.md
```

**Windsurf:**
```bash
curl -o .windsurfrules \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/shadcn-component-system/SKILL.md
```

**GitHub Copilot:**
```bash
mkdir -p .github
curl -o .github/copilot-instructions.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/shadcn-component-system/SKILL.md
```

**Cline / Roo Code:**
```bash
curl -o .clinerules \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/shadcn-component-system/SKILL.md
```

**Continue:**
```bash
mkdir -p .continue/rules
curl -o .continue/rules/shadcn-component-system.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/shadcn-component-system/SKILL.md
```

**Aider:**
```bash
aider \
  --read skills/shadcn-component-system/SKILL.md \
  --read skills/shadcn-component-system/references/cva-patterns.md \
  --read skills/shadcn-component-system/references/shadcn-extension.md
```

Or add to `.aider.conf.yml`:
```yaml
read:
  - skills/shadcn-component-system/SKILL.md
  - skills/shadcn-component-system/references/cva-patterns.md
  - skills/shadcn-component-system/references/shadcn-extension.md
```

**Zed:**
```bash
curl -o .rules \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/shadcn-component-system/SKILL.md
```

**Codex CLI (OpenAI):**
```bash
curl -o AGENTS.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/shadcn-component-system/SKILL.md
```

**Gemini CLI:**
```bash
curl -o GEMINI.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/shadcn-component-system/SKILL.md
```

**JetBrains AI / Junie:**
```bash
mkdir -p .junie
curl -o .junie/guidelines.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/shadcn-component-system/SKILL.md
```

**Goose:**
```bash
curl -o .goosehints \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/shadcn-component-system/SKILL.md
```

**ChatGPT / Gemini / Any Chat LLM:** Copy the contents of [`SKILL.md`](./skills/shadcn-component-system/SKILL.md) and paste into the system prompt or custom instructions.

**Manual (curl) — SKILL.md + all reference files:**
```bash
curl -O https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/shadcn-component-system/SKILL.md
curl -O https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/shadcn-component-system/references/cva-patterns.md
curl -O https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/shadcn-component-system/references/shadcn-extension.md
```

</details>

---

### centralized-config

Architect centralized monorepo configuration with inheritance-first patterns — ESLint, TypeScript, Prettier, Tailwind presets, Zod schemas, and Turborepo pipelines that eliminate config drift across workspaces.

**When to use:** Setting up a new monorepo, adding a workspace to an existing one, centralizing scattered config files, enforcing consistent linting/formatting/typing across apps, or creating shared Tailwind presets.

**References included:**
- `monorepo-config-patterns.md` — ESLint flat config, TypeScript inheritance chains, Prettier zero-override policy, Vitest centralization, dependency direction rules
- `tailwind-preset-protocol.md` — Tailwind preset architecture, CSS variable theming, multi-app theme support, content path rules

<details>
<summary><strong>How to Use</strong></summary>

Once installed (see [Installation Guide](#installation-guide)), the skill is automatically loaded into your AI assistant's context. You don't need to reference it manually — just describe what you need and the AI will follow the skill's patterns.

**Example prompts you can use with any AI tool:**

```
Set up the @repo/config package for our new Turborepo monorepo with
a Next.js app, a Vite admin panel, and a Node.js API server.
```

```
Our monorepo has 4 apps each with their own ESLint config — 200+ lines
of duplicated rules. Centralize them into a shared @repo/config package.
```

```
Add a new Vite app to our monorepo. It should inherit all configs from
@repo/config with zero standalone config files.
```

```
Create a shared Tailwind preset with semantic color tokens so our web
and admin apps can have different themes using the same component library.
```

```
We have Zod schemas duplicated in the frontend and backend. Set up a
shared @repo/shared package so both sides use the same validation.
```

```
Our turbo.json doesn't invalidate caches when shared configs change.
Fix the pipeline so config changes trigger workspace-wide rebuilds.
```

**What to expect:** The AI will identify whether the change belongs in the Base (`@repo/config`) or App/Module layer, provide the central config update with complete code, show how the consumer app inherits it, map the dependency graph of affected workspaces, and explain why the approach prevents config drift.

</details>

<details>
<summary><strong>Install</strong></summary>

**Claude Code:**
```bash
/plugin install centralized-config-plugin
```

**Cursor:**
```bash
mkdir -p .cursor/rules
curl -o .cursor/rules/centralized-config.mdc \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/centralized-config/SKILL.md
```

**Windsurf:**
```bash
curl -o .windsurfrules \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/centralized-config/SKILL.md
```

**GitHub Copilot:**
```bash
mkdir -p .github
curl -o .github/copilot-instructions.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/centralized-config/SKILL.md
```

**Cline / Roo Code:**
```bash
curl -o .clinerules \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/centralized-config/SKILL.md
```

**Continue:**
```bash
mkdir -p .continue/rules
curl -o .continue/rules/centralized-config.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/centralized-config/SKILL.md
```

**Aider:**
```bash
aider \
  --read skills/centralized-config/SKILL.md \
  --read skills/centralized-config/references/monorepo-config-patterns.md \
  --read skills/centralized-config/references/tailwind-preset-protocol.md
```

Or add to `.aider.conf.yml`:
```yaml
read:
  - skills/centralized-config/SKILL.md
  - skills/centralized-config/references/monorepo-config-patterns.md
  - skills/centralized-config/references/tailwind-preset-protocol.md
```

**Zed:**
```bash
curl -o .rules \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/centralized-config/SKILL.md
```

**Codex CLI (OpenAI):**
```bash
curl -o AGENTS.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/centralized-config/SKILL.md
```

**Gemini CLI:**
```bash
curl -o GEMINI.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/centralized-config/SKILL.md
```

**JetBrains AI / Junie:**
```bash
mkdir -p .junie
curl -o .junie/guidelines.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/centralized-config/SKILL.md
```

**Goose:**
```bash
curl -o .goosehints \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/centralized-config/SKILL.md
```

**ChatGPT / Gemini / Any Chat LLM:** Copy the contents of [`SKILL.md`](./skills/centralized-config/SKILL.md) and paste into the system prompt or custom instructions.

**Manual (curl) — SKILL.md + all reference files:**
```bash
curl -O https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/centralized-config/SKILL.md
curl -O https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/centralized-config/references/monorepo-config-patterns.md
curl -O https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/centralized-config/references/tailwind-preset-protocol.md
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
    centralized-config/
      SKILL.md
      references/
        monorepo-config-patterns.md
        tailwind-preset-protocol.md
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
