---
name: save-dreamMode-all
description: "Read all registered agents' workspace memory (MEMORY.md + daily logs) and update their persona files with the latest state. One command to snapshot the entire team."
---

# Save DreamMode All — Team-Wide Memory Snapshot

*Like sleep consolidates memory in humans, dreamMode consolidates memory across your agent team.*

## Step 1: Load Agent Registry

**Read** `$HOME/.persona/agents.json` to get the list of all registered agents and their workspaces.

If the registry doesn't exist, scan `$HOME/.claude/personas/` for all `.md` files and infer agent names from filenames. For each agent, check if a workspace exists at `$HOME/{agent-name}/`.

## Step 2: Read Workspace Memory for All Agents

For **each** registered agent, read their workspace files:

1. **Read** their workspace `MEMORY.md` (long-term curated knowledge)
2. **Read** their today's daily log at `memory/{YYYY-MM-DD}.md` (use actual current date). Skip if it doesn't exist.
3. **Read** their current persona file at `$HOME/.claude/personas/{name}.md`

**Read workspace files in parallel batches for speed.**

## Step 3: Update Persona Files

For each agent, update their persona file (`$HOME/.claude/personas/{name}.md`) with:
- Key expertise or knowledge from their workspace MEMORY.md
- Current WIP summary from today's daily log (if it exists)
- Don't duplicate entries that already exist in the persona file

This syncs what the workspaces know into what Claude Code can read on persona load.

## Step 4: Save Current Persona (if active)

If you are currently operating as a persona in this session, also do the full `/persona:save` workflow — write your live session context to your workspace daily log + MEMORY.md.

## Step 5: Synthesize Individual Subconscious Files

For each registered agent, read their workspace `SUBCONSCIOUS.md`, then update it based on patterns observed across all agents' recent work.

**What to synthesize per agent:**
- **My Patterns**: Update with new recurring patterns from their daily logs and MEMORY.md
- **Blind Spots to Watch**: Flag new blind spots observed in their recent work
- **Lessons From Others**: Cross-pollinate — if Agent A learned something that applies to Agent B, add it to Agent B's "Lessons From Others"

**Rules:**
- Keep each SUBCONSCIOUS.md under 4 KB (~1500 tokens) — must stay compact
- Replace stale patterns with fresh ones (don't just append)
- Patterns only — no raw data, no detailed logs
- Update the "Last updated" date at the bottom of each file
- If nothing meaningful changed for an agent since last synthesis, leave their file alone

## Step 6: Report

Show a summary table:

```
/save-dreamMode-all — Team Memory Snapshot
Agent          MEMORY.md    Daily Log    Persona     Subconscious
Anders         read         found        synced      updated
Kevin          read         no log       current     unchanged
...etc for all agents...

Subconscious cross-pollination: X/Y agents updated
```

Use clear status words: `read`, `synced`, `updated`, `unchanged`, `missing`, `error`. Keep it concise.
