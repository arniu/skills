---
name: kanban
description: >-
  Kanban-style view of current TodoWrite task list.
  Trigger only on explicit user requests: /kanban [filter].
  Filter: all / todo / in-progress / wip / completed / done.
---

# Kanban View Skill

Display TodoWrite tasks in kanban board. Support filter to control which columns shown.

## Default

`/kanban` — show **In Progress** only.

## Filter Usage

| Command                                   | Result                         |
| ----------------------------------------- | ------------------------------ |
| `/kanban`                                 | In Progress only               |
| `/kanban all`                             | In Progress + Todo + Completed |
| `/kanban todo` / `pending` / `backlog`    | Todo only                      |
| `/kanban done` / `completed` / `finished` | Completed only                 |
| `/kanban in-progress` / `wip` / `doing`   | In Progress only               |

## Column Mapping

| TodoWrite Status | Column Name | Accepts                                      |
| ---------------- | ----------- | -------------------------------------------- |
| in_progress      | In Progress | in-progress, in_progress, wip, doing, active |
| pending          | Todo        | todo, pending, backlog, waiting              |
| completed        | Completed   | completed, done, finished, closed            |

## Workflow

### Step 1: Parse filter

1. No arg → In Progress only
2. `all` → all three columns
3. Single filter word → match to column, show that column only

### Step 2: Collect tasks

Get current TodoWrite task list. Group by status. Filter by requested column(s).

### Step 3: Render board

```
## Kanban

### In Progress                   | 2 items

- [~] Setup CI/CD pipeline
- [~] Database migration

### Todo                           | 1 item

- [ ] Write API docs

### Completed                     | 3 items

- [x] Upgrade deps
- [x] Fix login bug
- [x] Add unit tests
```

`[~]` = in progress, `[ ]` = todo, `[x]` = completed.

### Edge Cases

- **Empty column** → mark "(empty)"
- **All empty** → "No tasks yet"
- **Unrecognized filter** → list supported filters: all, todo, in-progress, wip, completed, done

## Output Principles

- Default show In Progress only.
- Item count in column header.
- Plain language, no emoji.
