---
name: triage
description: "Prioritize your work queue — scan kanban, handoffs, and recent context to decide what to work on first. Use at session start or when juggling multiple tasks."
---

# /triage — Work Prioritization

*When everything feels urgent, triage tells you what actually is.*

## Step 1: Identify Active Persona & Workspace

Determine which persona you are and where your workspace lives.

## Step 2: Gather All Open Threads

Read these sources to build a complete picture of pending work:

### 2a: Kanban Cards (if available)

```bash
sqlite3 {kanban_db_path} "SELECT id, title, priority, status, notes FROM tasks WHERE assignee = '{Name}' AND status IN ('in-progress', 'review', 'backlog', 'tech-support') ORDER BY CASE status WHEN 'tech-support' THEN 0 WHEN 'in-progress' THEN 1 WHEN 'review' THEN 2 WHEN 'backlog' THEN 3 END, CASE priority WHEN 'critical' THEN 1 WHEN 'high' THEN 2 WHEN 'medium' THEN 3 ELSE 4 END LIMIT 15;"
```

### 2b: Pending Handoffs

```bash
ls $HOME/.persona/handoffs/*-to-{name}.md 2>/dev/null
```

If found, **read** each handoff file.

### 2c: Yesterday's WIP

**Read** `{workspace}/memory/{yesterday YYYY-MM-DD}.md` — scan for unfinished work.

### 2d: Persona File WIP

**Read** `$HOME/.claude/personas/{name}.md` — check **Current WIP** section.

## Step 3: Apply Priority Rules

Classify each open thread using this priority matrix:

| Priority | Category | Examples |
|----------|----------|----------|
| **P0** | Production down / customer-blocking | 500 errors, broken payments, data loss |
| **P1** | Tech support / handoff from another agent | Support tickets, cross-agent handoffs with context |
| **P2** | In-progress work (continuity) | Tasks you started yesterday, partially built features |
| **P3** | Review queue | Cards in review status, PRs to land |
| **P4** | New backlog items | Queued work not yet started |

**Rules:**
- P0 always goes first — no exceptions
- Handoffs from other agents (P1) beat continuity (P2) because someone is waiting
- Finishing in-progress work (P2) beats starting new work (P4) — WIP limits matter
- Within the same priority, prefer higher kanban priority (critical > high > medium > low)

## Step 4: Output the Triage

```
## Work Triage — {Name} {emoji}
Date: {YYYY-MM-DD}

| # | Priority | Source | Item | Notes |
|---|----------|--------|------|-------|
| 1 | P0 | kanban | [ID] Title | {why it's urgent} |
| 2 | P1 | handoff | From {Agent}: {task} | {key context} |
| 3 | P2 | WIP | {task from yesterday} | {what remains} |
| ... | | | | |

### Recommended: Start with #{1}
{One sentence explaining why this should be first}
```

## Step 5: Confirm with User

Ask:
> Start with #{1}, or do you want to reprioritize?

Do NOT start working until the user confirms. They may have context you don't — a Slack message, a customer call, a deadline change.

---

*Urgency is not importance. Triage tells you which is which.*
