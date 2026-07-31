# AGENTS.md

## Project structure

```
skills/          public — promoted
in-progress/     private — drafts
research/        private — study material
docs/            reference docs
  skills-guide.md      authoring conventions
  skills-philosophy.md writing methodology
```

Skills graduate from `in-progress/` to `skills/` when their evals pass.

`README.md` lists exactly the `skills/` set, each entry linked. Every skill change — add, rename, move, remove, or description edit — updates `README.md` in the same change. Install commands live only in `README.md`.

## Skill format

A skill is a directory with a `SKILL.md`: `name` + `description` frontmatter (the only trigger mechanism), body under 500 lines. See `docs/skills-guide.md` for the full conventions — authoring, evals, tuning.

## Testing

- Run the `/skill-creator` eval framework — an installed skill, not part of this repo
- Test workspace root is `.agents/workspaces/<skill-name>/` — overrides skill-creator's sibling `<skill-name>-workspace/`; keep its `iteration-N/` layout inside
- Use `isolation: "worktree"` for subagent testing to keep the working tree clean

## Git

- One logical change per commit
- Conventional commits: `type(scope): lowercase summary` — `feat`, `fix`, `docs`, `chore`, `refactor`, `test`
