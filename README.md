# skillforge

Claude Code skills for frontend and full-stack engineers.

---

## What is Skillforge?

Skillforge is a curated collection of AI-powered coding skills — opinionated, production-grade prompts that teach AI assistants how to perform specific engineering tasks. Each skill is a standalone prompt that works with any LLM-based coding tool.

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

### Claude Code (plugin)

```bash
# Add the marketplace
/plugin marketplace add iam-hussain/skillforge

# Install a specific skill
/plugin install component-splitter-plugin
```

### Manual (any AI tool)

Copy the SKILL.md content into your AI tool's system prompt or project context:

```bash
# Clone the repo
git clone https://github.com/iam-hussain/skillforge.git

# Copy a skill to your Claude Code config
mkdir -p ~/.claude/skills/component-splitter
cp skillforge/skills/component-splitter/SKILL.md ~/.claude/skills/component-splitter/

# Or just grab the raw file
curl -o SKILL.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/component-splitter/SKILL.md
```

For other AI tools, paste the contents of `SKILL.md` and relevant `references/*.md` files into your system prompt or project instructions.

---

## Project Structure

```
skillforge/
  skills/                      ← one folder per skill
    component-splitter/
      SKILL.md                 ← main skill prompt
      references/              ← deep-dive reference docs
        atomic-design.md
        hooks-patterns.md
        compound-components.md
        feature-slice.md
  .claude-plugin/
    marketplace.json           ← Claude Code marketplace catalog
  plugin.json                  ← plugin manifest
  CLAUDE.md                    ← repo context
  CONTRIBUTING.md              ← contribution guide
```

---

## Skill Anatomy

Each skill follows this structure:

```
skills/<skill-name>/
  SKILL.md                     ← main prompt (under 500 lines)
  references/                  ← detailed reference docs
    pattern-name.md
```

The `SKILL.md` contains YAML frontmatter (`name`, `description`) and the full prompt. Reference files provide deep-dive documentation that the main prompt links to.

---

## Contributing

See [CONTRIBUTING.md](./CONTRIBUTING.md) for details on how to add a new skill.

---

## Roadmap

See the full roadmap in [CLAUDE.md](./CLAUDE.md#skill-roadmap).

**Coming soon:** design-tokens, form-architecture, state-management, api-design, auth-patterns, and more.

---

## License

MIT
