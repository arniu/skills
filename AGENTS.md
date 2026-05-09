# AGENTS.md

## Project structure

```
skills/          public — linked in README
in-progress/     private — drafts
deprecated/      private — dead
docs/            reference docs
  skills-guide.md  comprehensive skill authoring guide
```

Skills are in `in-progress/` while drafting, then moved to `skills/` when ready.

## Skill format

Each skill is a directory with a `SKILL.md` containing:

- **YAML frontmatter**: `name`, `description` (the only trigger mechanism)
- **Body**: instructions under 500 lines
- Optional: `evals/`, `references/`, `scripts/`, `assets/`

See `docs/skills-guide.md` for full conventions.

## Commands

```bash
# Install a skill from this repo
npx skills add arniu/zkills --skill <name> -y

# Test a skill (via /skill-creator eval framework)
# See docs/skills-guide.md for creating and running evals
```

## Development workflow

- Test outputs go to `.claude/workspaces/`
- Use `isolation: "worktree"` for subagent testing to keep the working tree clean
