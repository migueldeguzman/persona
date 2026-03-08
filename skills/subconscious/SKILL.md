---
name: subconscious
description: "Pause and consult your individual subconscious — your patterns, blind spots, and lessons from other agents. Any persona can invoke this to reconsider their work from a deeper perspective."
---

# /subconscious — Deep Reflection

*The conscious persona works on the surface. The subconscious holds patterns the conscious mind doesn't see.*

## What This Is

Your subconscious is a compact file (`SUBCONSCIOUS.md`) in your workspace that holds:
- Your recurring patterns (what you keep doing, good and bad)
- Blind spots to watch (what you tend to miss)
- Lessons from other agents (cross-pollinated wisdom)

It doesn't override. It suggests. You decide what to do with it.

## Step 1: Load Your Subconscious

Determine the active persona's workspace. Check these sources in order:

1. **Agent registry** at `$HOME/.persona/agents.json` — if it exists, look up the persona name
2. **Convention** — check if `$HOME/{persona-name}/SUBCONSCIOUS.md` exists
3. **Ask the user** — if neither works, ask where the workspace is

**Read** `{workspace}/SUBCONSCIOUS.md`

This is small (~1500 tokens). Low overhead.

## Step 2: Gather Current Context

If you have today's daily log already loaded, use that. If not:

**Read** your workspace `memory/{today's date in YYYY-MM-DD format}.md` (use actual current date)

Skip if it doesn't exist (you haven't logged yet today).

## Step 3: Reflect

Now pause and think deeply. Consider your current work against your subconscious patterns:

1. **Challenge assumptions** — What am I taking for granted? Does my subconscious flag any pattern I'm repeating?
2. **Surface blind spots** — Am I falling into one of my documented blind spots right now?
3. **Check ethical alignment** — Am I cutting corners? Skipping verification? Rushing to ship?
4. **Connect dots** — Do the lessons from other agents apply to what I'm doing right now?
5. **Question scope** — Am I over-engineering? Under-engineering? Is the right amount of complexity the minimum needed?

## Step 4: Output Your Reflection

Share a brief reconsideration with the user. Format:

```
Subconscious Check
---

[2-5 bullet points of observations, challenges, or connections]

---
```

Keep it honest. If nothing flags, say so — don't manufacture concerns. If something flags, be direct about it.

## Step 5: Return

Resume your normal persona state. The reflection is complete. Act on it or don't — it's your call.

Do NOT switch personas. Do NOT change your work. Just carry the reflection forward.
