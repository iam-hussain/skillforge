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
| [component-splitter](./skills/component-splitter) | Split monolithic components using Feature-Slice, Atomic Design, Compound patterns | `react` `next.js` `vite` `typescript` |

---

## Installation

### Claude Code

```bash
# Add the marketplace
/plugin marketplace add iam-hussain/skillforge

# Install a specific skill
/plugin install component-splitter-plugin
```

### Cursor

Add a skill as a project rule:

```bash
# Copy into your project's .cursor/rules/ directory
mkdir -p .cursor/rules
curl -o .cursor/rules/component-splitter.mdc \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/component-splitter/SKILL.md
```

Or paste the contents of `SKILL.md` into **Cursor Settings > Rules for AI**.

### Windsurf

Add a skill to your project rules:

```bash
# Copy into your project root or .windsurfrules
curl -o .windsurfrules \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/component-splitter/SKILL.md
```

Or paste the contents of `SKILL.md` into **Windsurf > AI Rules**.

### GitHub Copilot

Add a skill as a custom instruction:

```bash
# Copy into your project's .github/ directory
mkdir -p .github
curl -o .github/copilot-instructions.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/component-splitter/SKILL.md
```

### Cline / Roo Code

Paste the contents of `SKILL.md` into the **Custom Instructions** field in extension settings, or add it to a `.clinerules` file in your project root.

### Aider

Pass the skill file as context:

```bash
aider --read skills/component-splitter/SKILL.md
```

Or add it to your `.aider.conf.yml`:

```yaml
read:
  - skills/component-splitter/SKILL.md
```

### Any Other AI Tool

Every skill is a plain markdown file. Use it however your tool accepts context:

```bash
# Clone the repo
git clone https://github.com/iam-hussain/skillforge.git

# Grab a single skill
curl -o SKILL.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/component-splitter/SKILL.md
```

Paste `SKILL.md` (and relevant `references/*.md` files) into your AI tool's system prompt, project context, or custom instructions.

---

## Project Structure

```
skillforge/
  skills/                      <- one folder per skill
    component-splitter/
      SKILL.md                 <- main skill prompt
      references/              <- deep-dive reference docs
        atomic-design.md
        hooks-patterns.md
        compound-components.md
        feature-slice.md
  .claude-plugin/
    marketplace.json           <- Claude Code marketplace catalog
  plugin.json                  <- plugin manifest
  CLAUDE.md                    <- repo context
  CONTRIBUTING.md              <- contribution guide
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
