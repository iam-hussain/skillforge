# Skillforge

Claude Code skills for frontend and full-stack engineers.

Repository: https://github.com/iam-hussain/skillforge
Owner: Jakir Hussain (@iam-hussain)
Stack focus: React, Next.js, Vite, Node.js, TypeScript

---

## What is Skillforge?

Skillforge is a curated collection of Claude Code skills. Each skill
teaches Claude how to complete a specific engineering task in a
repeatable, production-grade way.

Skills in this repo are:
- Opinionated but practical (real patterns used in production)
- Stack-aware (Next.js App Router, Vite, Node.js, TypeScript)
- Composable (skills work together, not in isolation)
- Evergreen (updated as the ecosystem evolves)

---

## Repo Structure

```
skillforge/
  skills/                      ← one folder per skill
    component-splitter/
      SKILL.md                 ← main skill file
      references/              ← deep-dive reference docs
        atomic-design.md
        hooks-patterns.md
        compound-components.md
        feature-slice.md
    [next-skill]/
      SKILL.md
      references/
  .claude-plugin/
    marketplace.json           ← Claude Code marketplace catalog
  plugin.json                  ← plugin manifest
  CLAUDE.md                    ← this file (repo-level context)
  README.md                    ← public-facing documentation
  CONTRIBUTING.md              ← how to contribute a skill
  .github/
    ISSUE_TEMPLATE/
      new-skill.md
      bug-report.md
    workflows/
      validate-skills.yml      ← CI to validate SKILL.md structure
```

---

## Skill Quality Standards

Every skill in this repo must meet these standards before merging:

### Structure
- [ ] Has a SKILL.md with valid YAML frontmatter (name, description)
- [ ] Description clearly states when to trigger (starts with "Use when")
- [ ] SKILL.md is under 500 lines
- [ ] Reference files live in references/ subfolder
- [ ] Includes at least one before/after example

### Content
- [ ] Opinionated — gives a clear recommendation, not a list of options
- [ ] Stack-aware — covers Next.js and/or Vite where relevant
- [ ] TypeScript-first — all code examples use TypeScript
- [ ] Production-grade — patterns used in real codebases, not tutorials
- [ ] No placeholders — all code examples are complete and working
- [ ] LLM-agnostic — skill content works with any AI coding assistant

### Naming
- [ ] Skill folder name matches the name field in frontmatter
- [ ] Folder name is lowercase, hyphenated (component-splitter, not ComponentSplitter)
- [ ] Reference files are descriptive (hooks-patterns.md, not hooks.md)

---

## Skill Roadmap

### Frontend
- component-splitter     ✅ done
- design-tokens          ← CSS variables, Tailwind config, theming
- form-architecture      ← react-hook-form + zod patterns
- state-management       ← Zustand vs Context decision guide
- animation-patterns     ← Framer Motion, CSS transitions
- accessibility-audit    ← WCAG, aria patterns, keyboard nav

### Full-Stack
- api-design             ← REST + tRPC patterns, error handling
- auth-patterns          ← NextAuth, JWT, session management
- database-schema        ← Prisma schema design, relations
- error-handling         ← global error boundaries, API errors
- env-config             ← environment variables, config management

### DevOps & Tooling
- ci-pipeline            ← GitHub Actions for Node/React projects
- docker-setup           ← Dockerfile + compose for Node.js
- performance-audit      ← Core Web Vitals, bundle analysis

### Architecture
- monorepo-setup         ← Turborepo + shared packages
- microservice-patterns  ← service boundaries, communication

---

## marketplace.json Rules

The marketplace file lives at `.claude-plugin/marketplace.json`.
When adding a new skill, follow this schema:

```json
{
  "name": "skillforge",
  "owner": {
    "name": "Jakir Hussain",
    "github": "iam-hussain"
  },
  "version": "1.0.0",
  "description": "Claude Code skills for frontend and full-stack engineers",
  "plugins": [
    {
      "name": "skill-name-plugin",
      "source": "./skills/skill-name",
      "description": "One line — what it does and who it's for",
      "category": "engineering",
      "tags": ["react", "typescript", "relevant-tag"]
    }
  ]
}
```

Categories allowed: engineering, frontend, backend, architecture,
devops, tooling, testing

---

## plugin.json Rules

```json
{
  "name": "skillforge",
  "description": "Claude Code skills for frontend and full-stack engineers",
  "version": "1.0.0",
  "author": "Jakir Hussain",
  "license": "MIT",
  "repository": "https://github.com/iam-hussain/skillforge",
  "skills": [
    "component-splitter"
  ]
}
```

Always add the skill name to the skills array when a new skill is added.

---

## Writing a New Skill — Process

When asked to create a new skill for this repo:

1. Read the existing component-splitter/SKILL.md as the quality benchmark
2. Follow the same structure: frontmatter → overview → step-by-step →
   reference routing → examples → anti-patterns
3. Create the folder under skills/
4. Add reference files to references/ for any content over 100 lines
5. Update .claude-plugin/marketplace.json plugins array
6. Update plugin.json skills array
7. Update README.md skills table

---

## Tone & Style for Skill Writing

- Opinionated, not wishy-washy — pick a pattern and defend it
- Concise — no filler sentences, every line earns its place
- Code-first — show don't tell, real examples over long explanations
- Engineer-to-engineer — no hand-holding, assume TypeScript fluency
- Practical — patterns from real production codebases, not textbooks
- LLM-agnostic — write prompts that work with any AI assistant

---

## Install Command

```bash
# Add the marketplace
/plugin marketplace add iam-hussain/skillforge

# Install a specific skill
/plugin install component-splitter-plugin

# Or copy a skill manually
mkdir -p ~/.claude/skills/component-splitter
curl -o ~/.claude/skills/component-splitter/SKILL.md \
  https://raw.githubusercontent.com/iam-hussain/skillforge/main/skills/component-splitter/SKILL.md
```
