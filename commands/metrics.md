---
name: metrics
description: "Measure persona system health — count daily logs, subconscious pattern usage, handoff completions, and memory growth across all agents. Use for team health checks and research."
---

# /metrics — Persona System Health

*What gets measured gets maintained.*

## Step 1: Load Agent Registry

**Read** `$HOME/.persona/agents.json` or scan `$HOME/.claude/personas/` to discover all agents.

## Step 2: Collect Metrics per Agent

For each agent, gather:

### 2a: Activity (daily log frequency)

```bash
ls {workspace}/memory/????-??-??.md 2>/dev/null | wc -l
```

Also check the last 7 days specifically — is this agent active?

```bash
for i in 0 1 2 3 4 5 6; do
  d=$(date -v-${i}d +%Y-%m-%d 2>/dev/null || date -d "$i days ago" +%Y-%m-%d 2>/dev/null)
  [ -f "{workspace}/memory/$d.md" ] && echo "$d: active" || echo "$d: -"
done
```

### 2b: Memory Size

```bash
wc -c {workspace}/MEMORY.md {workspace}/SUBCONSCIOUS.md 2>/dev/null
```

Flag if MEMORY.md > 16 KB or SUBCONSCIOUS.md > 4 KB — needs trimming.

### 2c: Subconscious Health

**Read** `{workspace}/SUBCONSCIOUS.md` and count:
- Total entries (bullet points)
- Entries with dates (maintained) vs entries without dates (possibly stale)
- Last updated date

### 2d: Handoff Activity

```bash
ls $HOME/.persona/handoffs/*-to-{name}.md 2>/dev/null | wc -l   # pending
ls $HOME/.persona/handoffs/archive/*-to-{name}.md 2>/dev/null | wc -l  # completed
ls $HOME/.persona/handoffs/{name}-to-*.md 2>/dev/null | wc -l   # sent
```

### 2e: Persona File Freshness

**Read** `$HOME/.claude/personas/{name}.md` — check the **Current WIP** section. Is it from today? Yesterday? Last week? Stale WIP = context loss.

## Step 3: Output the Dashboard

```
## Persona System Metrics
Date: {YYYY-MM-DD}

### Agent Activity (last 7 days)
| Agent | Days Active | Total Logs | Last Active |
|-------|-------------|------------|-------------|
| Anders | 5/7 | 42 | today |
| Kevin | 3/7 | 28 | yesterday |
| ... | | | |

### Memory Health
| Agent | MEMORY.md | SUBCONSCIOUS.md | Patterns | Last Reflected |
|-------|-----------|-----------------|----------|----------------|
| Anders | 14.2 KB | 2.8 KB | 10 | 2026-03-18 |
| Kevin | 8.1 KB | 1.2 KB | 6 | 2026-03-07 |

### Handoffs
| Agent | Pending In | Completed | Sent Out |
|-------|-----------|-----------|----------|
| Anders | 0 | 3 | 5 |

### Flags
- {Agent}: MEMORY.md over 16 KB — needs trimming
- {Agent}: SUBCONSCIOUS.md not updated in 30+ days — run /reflect
- {Agent}: No daily logs in 7 days — inactive or not saving
- {Agent}: Pending handoffs not consumed — check if persona is being activated
```

## Step 4: Recommendations

Based on metrics, suggest actions:
- Agents with stale subconscious → recommend `/reflect`
- Agents with oversized MEMORY.md → recommend trimming
- Agents with pending handoffs → recommend `/resume`
- Inactive agents → flag for team lead

---

*Health checks for minds, not just servers.*
