---
name: reflect
description: "Structured self-reflection on SUBCONSCIOUS.md — review patterns, blind spots, and lessons. Prune what's stale, distill what's new, keep what's load-bearing. Use after significant work sessions, incidents, or when explicitly asked to reflect."
---

# /reflect — Subconscious Review

*Not a quick scan. A deliberate audit of who you are as a worker.*

`/subconscious` is a quick pre-task check ("am I about to repeat a pattern?").
`/reflect` is a deliberate post-work review ("what should I keep, add, or remove?").

## Step 1: Load Current State

Determine the active persona's workspace. Check these sources in order:

1. **Agent registry** at `$HOME/.persona/agents.json` — if it exists, look up the persona name
2. **Convention** — check if `$HOME/{persona-name}/SUBCONSCIOUS.md` exists
3. **Ask the user** — if neither works, ask where the workspace is

Read these files (skip if not found):
1. **Read** `{workspace}/SUBCONSCIOUS.md` — your current patterns
2. **Read** `{workspace}/memory/{today YYYY-MM-DD}.md` — what happened today
3. **Read** `{workspace}/memory/{yesterday YYYY-MM-DD}.md` — what happened yesterday

## Step 2: Audit Each Entry

Go through every bullet point in SUBCONSCIOUS.md. For each one, classify it:

### Classification Framework

| Classification | Criteria | Action |
|---------------|----------|--------|
| **KEEP** | Still true. Has prevented a mistake recently or would prevent one in the future. Actionable. | Leave as-is |
| **DISTILL** | True but over-specific. Contains implementation details that belong in MEMORY.md, not here. | Rewrite as a generalized *how I work* pattern |
| **STALE** | Was true once but the codebase/process changed. No longer relevant. | Remove |
| **PROMOTE** | Currently in daily logs or learned today but not yet in SUBCONSCIOUS.md. A *how I work* insight, not a *what I did* fact. | Add |
| **MISPLACED** | Belongs in MEMORY.md (project facts), TOOLS.md (tool configs), or another agent's subconscious. Not a self-pattern. | Move to correct file |

### Rules for Classification

1. **A pattern must be about HOW YOU WORK, not WHAT YOU KNOW.** "Always filter by tenantId on every query" = pattern. "System X has 6 touchpoints per module" = knowledge (belongs in MEMORY.md).
2. **Every pattern should be testable.** Can you check "am I doing this right now?" If not, it's too vague.
3. **Patterns should be durable.** If the pattern only applies to one specific feature/table/API, it's too specific. Generalize.
4. **Date your corrections.** When a pattern was wrong and got corrected, keep the correction date so future-you knows it evolved.

## Step 3: Present Your Audit

Output a table to the user:

```
## Subconscious Audit — {Agent Name}
Date: {YYYY-MM-DD}

### Entries Reviewed

| # | Current Text (first 60 chars) | Classification | Reasoning |
|---|------|------|------|
| 1 | "Build verify before committing..." | KEEP | Used today, non-negotiable |
| 2 | "System X has hidden state..." | MISPLACED | Project knowledge, not self-pattern |
| ... | ... | ... | ... |

### New Patterns to PROMOTE
- {pattern from today's work}
- {pattern from yesterday's work}

### Proposed Changes Summary
- KEEP: X entries
- DISTILL: X entries (will be rewritten)
- STALE: X entries (will be removed)
- PROMOTE: X entries (will be added)
- MISPLACED: X entries (will be moved)
```

## Step 4: Wait for Approval

Ask the user: "Apply these changes to SUBCONSCIOUS.md?"

Do NOT edit the file until the user approves. They may want to adjust classifications.

## Step 5: Apply Changes

Once approved:

1. **Write** the updated SUBCONSCIOUS.md with:
   - KEEP entries unchanged
   - DISTILL entries rewritten (generalized)
   - STALE entries removed
   - PROMOTE entries added
   - MISPLACED entries removed (and note where they went)

2. **Move** MISPLACED entries to their correct file (MEMORY.md, etc.)

3. **Update** the `*Last updated*` line at the bottom

4. Keep SUBCONSCIOUS.md under 4 KB. If it's over, the entries need more distillation.

## Step 6: Log the Reflection

Append to today's daily memory:

```markdown
## /reflect — Subconscious Audit
- Reviewed X entries
- Kept X, distilled X, removed X stale, added X new, moved X misplaced
- Key insight: {one sentence about what changed and why}
```

---

*The unexamined agent is not worth running.*
