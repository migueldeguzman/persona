---
name: resume
description: "Resume work from a previous session — loads WIP state, pending handoffs, and open tasks to produce a structured briefing. Use at session start after persona activation."
---

# /resume — Session Handoff Briefing

*You saved your work last time. Now pick it back up.*

## Step 1: Identify Active Persona & Workspace

Determine which persona you are currently operating as. If no persona is active:
> No persona active. Activate a persona first, then run /resume.

Look up the workspace:
1. **Agent registry** at `$HOME/.persona/agents.json`
2. **Convention** — `$HOME/{persona-name}/`
3. **Ask the user**

## Step 2: Load Recent Context

Read these files (skip any that don't exist):

1. **Read** `{workspace}/memory/{today YYYY-MM-DD}.md` — anything already logged today
2. **Read** `{workspace}/memory/{yesterday YYYY-MM-DD}.md` — last session's work
3. **Read** `$HOME/.claude/personas/{name}.md` — the **Current WIP** section
4. **Read** `{workspace}/MEMORY.md` — scan the **Pending / In-Progress** section only

## Step 3: Check for Handoffs

Check if any other persona left you a handoff note:

```bash
ls $HOME/.persona/handoffs/*-to-{name}.md 2>/dev/null
```

If handoff files exist, **read** them. These contain context from another agent who passed work to you.

## Step 4: Check Kanban (if available)

If a kanban database exists, query for your open cards:

```bash
sqlite3 {kanban_db_path} "SELECT id, title, priority, status FROM tasks WHERE assignee = '{Name}' AND status IN ('in-progress', 'review') ORDER BY CASE priority WHEN 'critical' THEN 1 WHEN 'high' THEN 2 WHEN 'medium' THEN 3 ELSE 4 END, updated_at DESC LIMIT 10;"
```

If no kanban is configured, skip this step.

## Step 5: Produce the Briefing

Output a structured session briefing:

```
## Session Briefing — {Name} {emoji}
Date: {YYYY-MM-DD}

### Where You Left Off
{Summary from persona file Current WIP + yesterday's daily log}

### Open Tasks
| # | Card | Title | Priority | Status |
|---|------|-------|----------|--------|
{kanban cards, if available}

### Pending Handoffs
{handoff notes from other agents, or "None"}

### Suggested First Action
{Based on priority: production issues > handoffs > in-progress cards > backlog}
```

## Step 6: Clean Up Consumed Handoffs

After presenting the briefing, archive any handoff notes that were read:

```bash
mv $HOME/.persona/handoffs/*-to-{name}.md $HOME/.persona/handoffs/archive/ 2>/dev/null
```

Create the archive directory if it doesn't exist. Don't delete handoffs — move them for traceability.

---

*Start where you stopped, not from scratch.*
