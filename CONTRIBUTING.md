# Contributing to Skillforge

Thanks for your interest in contributing a skill to Skillforge!

---

## How to Add a New Skill

### 1. Create the Skill Folder

```bash
mkdir -p skills/<your-skill-name>/references
```

Folder name must be lowercase, hyphenated: `form-architecture`, not `FormArchitecture`.

### 2. Create SKILL.md

Every skill must have a `SKILL.md` with YAML frontmatter:

```markdown
---
name: your-skill-name
description: "Use when [trigger condition]. [What it does]."
---

# Skill Title

[Skill content — the actual prompt]
```

**Requirements:**
- Valid YAML frontmatter with `name` and `description`
- Description starts with "Use when" to clearly define the trigger
- Under 500 lines total
- At least one before/after example
- All code examples in TypeScript
- LLM-agnostic — no references to specific AI tools in the prompt content

### 3. Add Reference Docs

Place deep-dive markdown files in `references/`:

```
skills/your-skill/
  SKILL.md
  references/
    pattern-a.md
    pattern-b.md
```

Move content to a reference file when it exceeds ~100 lines. Keep
`SKILL.md` as the orchestration doc that links to references.

Reference file names should be descriptive: `hooks-patterns.md`, not `hooks.md`.

### 4. Register the Skill

**.claude-plugin/marketplace.json** — add an entry to the `plugins` array:

```json
{
  "name": "your-skill-name-plugin",
  "source": "./skills/your-skill-name",
  "description": "One line — what it does",
  "category": "frontend",
  "tags": ["react", "typescript"]
}
```

Categories: `engineering`, `frontend`, `backend`, `architecture`, `devops`, `tooling`, `testing`

**plugin.json** — add the skill name to the `skills` array:

```json
{
  "skills": ["react-component-splitter", "react-project-structure", "your-skill-name"]
}
```

**README.md** — add a row to the skills table:

```markdown
| [your-skill-name](./skills/your-skill-name) | Description | `tag1` `tag2` |
```

**CLAUDE.md** — update the roadmap to mark the skill as done.

### 5. Submit a Pull Request

Use the "New Skill" issue template to track your contribution, then submit a PR.

---

## Quality Checklist

Before submitting, verify:

- [ ] SKILL.md has valid YAML frontmatter (name + description)
- [ ] Description starts with "Use when"
- [ ] SKILL.md is under 500 lines
- [ ] Reference files in `references/` subfolder
- [ ] At least one before/after example
- [ ] All code examples use TypeScript
- [ ] Opinionated — picks a pattern and defends it
- [ ] Stack-aware — covers Next.js and/or Vite where relevant
- [ ] No placeholders — all code examples are complete
- [ ] LLM-agnostic — works with any AI assistant
- [ ] Folder name matches frontmatter `name`
- [ ] `references/` contains at least one `.md` file
- [ ] README.md skill section includes both **How to Use** and **Install** `<details>` blocks
- [ ] Registered in .claude-plugin/marketplace.json, plugin.json, README.md

---

## Style Guide

- **Opinionated** — pick a pattern and defend it, don't list options
- **Concise** — every line earns its place, no filler
- **Code-first** — show don't tell, real examples over explanations
- **Engineer-to-engineer** — assume TypeScript fluency
- **Practical** — patterns from production codebases, not textbooks

---

## Versioning

Skillforge uses semantic versioning for both the top-level package
(`marketplace.json` and `plugin.json`) and individual plugins
(`marketplace.json` plugin entries).

| Change type | Version bump | Examples |
|-------------|-------------|----------|
| Typo fix, comment update, README tweak | **patch** (1.0.0 → 1.0.1) | Fix wording, update URLs |
| New skill, new CI check, new reference file | **minor** (1.0.0 → 1.1.0) | Add `form-architecture` skill |
| Breaking schema change, rename skill folder | **major** (1.0.0 → 2.0.0) | Rename `react-component-splitter` → `component-decomposition` |

**When to bump:**
- Adding a new skill → bump top-level **minor** in both `marketplace.json` and `plugin.json`
- Updating an existing skill → bump that plugin's **patch** version in `marketplace.json`
- Structural/breaking change → bump top-level **major** everywhere

---

## Validation

The CI workflow (`validate-skills.yml`) automatically checks:
- SKILL.md exists in each skill folder
- SKILL.md has valid YAML frontmatter with `name` and `description`
- Description starts with "Use when"
- Frontmatter `name` matches the folder name
- SKILL.md is under 500 lines
- Each skill folder has a `references/` directory with at least one `.md` file
- `marketplace.json` and `plugin.json` list the same skills
- Every plugin `source` path in `marketplace.json` exists on disk
