---
name: handoff
description: "Hand off work to another persona — write a structured context note so they can pick up where you left off. Use when switching agents mid-task or delegating work."
---

# /handoff — Cross-Agent Context Transfer

*Your context dies when your session ends. This skill makes it survive.*

## Step 1: Identify Sender & Receiver

You are the **sender** (active persona). Ask the user who the **receiver** is if not obvious from context:

> Who should I hand this off to?

If the user already mentioned a name (e.g., "I asked Alex to do that"), use that.

## Step 2: Write the Handoff Note

Create a structured handoff file at:

```
$HOME/.persona/handoffs/{sender}-to-{receiver}.md
```

Create the `$HOME/.persona/handoffs/` directory if it doesn't exist.

### Handoff Template

```markdown
---
from: {Sender Name}
to: {Receiver Name}
date: {YYYY-MM-DD}
priority: {critical|high|medium|low}
---

# Handoff: {One-line task description}

## Context
{2-3 sentences: what were you working on, why, and where in the process}

## What's Done
{Bullet list of completed steps, commits, branches}

## What Remains
{Bullet list of remaining work — be specific about files and logic}

## Key Files
{List the exact file paths the receiver needs to read/modify}

## Decisions Made
{Any architectural or design decisions the receiver should know about — and WHY}

## Blockers / Warnings
{Anything that could trip them up — stale data, missing env vars, known bugs}

## Kanban Cards
{List relevant card IDs if using a kanban board}
```

### Rules for Good Handoffs

1. **Be specific about files.** "The booking service needs work" is useless. "booking.service.ts line 420 — the Speed sync callback needs error handling" is useful.
2. **Explain WHY, not just WHAT.** The receiver doesn't have your context. A decision without reasoning will get reversed.
3. **Include the fail path.** What did you try that didn't work? Saves the receiver from repeating your dead ends.
4. **Don't assume shared knowledge.** The receiver may not know which branch deploys, which DB is production, or which env vars matter.

## Step 3: Confirm

Respond with:

> {emoji} Handoff written: {sender} → {receiver}
> Task: {one-line description}
> File: `~/.persona/handoffs/{sender}-to-{receiver}.md`
>
> {Receiver} will see this on their next `/resume`.

## Step 4: Save Your Own State

If you haven't already saved this session, do a quick `/save` — write your WIP to the daily log and persona file so your own context is preserved too.

---

*The best handoff is one the receiver doesn't need to ask questions about.*
