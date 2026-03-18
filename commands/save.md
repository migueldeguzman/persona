---
name: save
description: "Save the current persona's session work to their workspace memory files (daily log + MEMORY.md). Use anytime to checkpoint your current state."
---

# Save Persona Memory

## Step 1: Identify Active Persona

Determine which persona you are currently operating as. If no persona is active, respond:
> No persona active. Nothing to save.

## Step 2: Determine Workspace

Look up the active persona's workspace. Check these sources in order:

1. **Agent registry** at `$HOME/.persona/agents.json` — if it exists, look up the persona name
2. **Convention** — check if `$HOME/{persona-name}/SOUL.md` exists
3. **Ask the user** — if neither works, ask where the workspace is

## Step 3: Save to Daily Log

**Read** the current daily log at `{workspace}/memory/{YYYY-MM-DD}.md` (use actual current date).

If it exists, **append** to it. If it doesn't exist, **create** it.

Write a summary of this session's work:
- What tasks you worked on
- Decisions made and why
- Lessons learned or gotchas discovered
- Commits, branches, or deployments
- Current WIP status and next steps

Format: markdown headings by topic, bullet points for details.

## Step 4: Update Long-Term Memory (if needed)

**Read** `{workspace}/MEMORY.md`.

If you learned something **significant and lasting** this session (not just task progress), update MEMORY.md:
- New architectural knowledge
- Important rules or patterns discovered
- Infrastructure changes
- Key file paths or service details

Keep MEMORY.md under ~16 KB. Don't duplicate daily log details — only promote the wisdom.

## Step 5: Update Persona File

**Read** `$HOME/.claude/personas/{name}.md`.

Update the **## Current WIP** section with:
- What task is in progress
- Current status
- Next step when resuming

This helps the next Claude Code session (or `/resume`) know where you left off.

## Step 6: Confirm

Respond with:
> {emoji} {Persona} memory saved. {1-line summary of what was logged}

Keep it to one line.

**Note:** `/save` is a fast checkpoint — it does NOT update SUBCONSCIOUS.md. Use `/reflect` for deliberate pattern review after significant work sessions.
