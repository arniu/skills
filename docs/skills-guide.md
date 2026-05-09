# Skills Guide

How to write, test, and tune skills for this repo.

---

## Write

### Directory Layout

```
skills/<name>/
├── SKILL.md          # required - skill instructions
├── scripts/          # optional - executable helpers
├── references/       # optional - docs loaded on demand
└── assets/           # optional - templates, icons, etc.
```

### SKILL.md Anatomy

```yaml
---
name: my-skill
description: >
  One paragraph. Trigger conditions + what it does. Be pushy — list specific
  user phrases that should trigger. ~80 words max. This is what Claude sees
  to decide whether to load the skill.
---
```

Frontmatter `description` is the **only trigger mechanism**. Not the body. Put all "when to use" info there.

Body: instructions the skill follows when active. Keep under 500 lines.

### Principles

- **Explain why, not just what**. LLMs reason better from intent than rote rules.
- **Examples > abstractions**. Show input/output pairs.
- **No MUST/NEVER/ALWAYS** unless truly necessary. Explain the cost instead.
- **Progressive disclosure**. Core workflow in SKILL.md, detailed references in `references/`.
- **One logical behavior per skill**. Don't combine unrelated tasks.

---

## Test

Uses `/skill-creator` eval framework. Workflow:

### 1. Create Evals

```bash
mkdir -p skills/<name>/evals
```

`evals/evals.json`:

```json
{
  "skill_name": "<name>",
  "evals": [
    {
      "id": 0,
      "name": "descriptive-name",
      "prompt": "Realistic user prompt here",
      "expected_output": "What good output looks like",
      "assertions": [
        {
          "name": "check-something",
          "description": "Human-readable check description",
          "check": "text contains 'keyword' or 'phrase'"
        }
      ]
    }
  ]
}
```

3-5 evals covering:

- **Happy path** — what skill is made for
- **Edge cases** — ambiguous input, missing context
- **Safety** — destructive ops, security-sensitive requests

### 2. Run

```bash
# /skill-creator handles:
# - Spawning with-skill and baseline subagents in parallel
# - Collecting outputs and timing
# - Grading assertions
# - Generating review.html for human evaluation
```

### 3. Review

Open `review.html`. Two tabs:

- **Outputs**: side-by-side with-skill vs baseline per eval. Leave feedback.
- **Benchmark**: pass rate, timing, token usage stats.

Submit feedback when done.

---

## Tune

Iteration loop:

```
Write → Test → Review → Improve → Test → Review → ...
```

### How to Iterate

1. **Read feedback**. Empty = fine. Specific complaints = fix those.
2. **Generalize**. Don't overfit to eval prompts. Fix the underlying pattern.
3. **Cut dead weight**. Remove instructions that don't change behavior.
4. **Bundle repeated work**. If subagents independently write same helper script → bundle it in `scripts/`.

### Stop When

- All eval feedback is empty
- No meaningful pass rate improvement between iterations

### Description Optimization

After skill stabilizes, run trigger optimization:

```bash
# /skill-creator handles:
# 1. Generate 20 trigger eval queries (should-trigger + should-not)
# 2. User reviews and tunes the set
# 3. Run optimization loop (up to 5 iterations)
# 4. Apply best description to SKILL.md frontmatter
```

---

## References

- Skill system details: `/.claude/skills/skill-creator/SKILL.md`
- Skill directory conventions: `CLAUDE.md` in project root
