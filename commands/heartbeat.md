---
name: heartbeat
description: "Auto-save persona memory on a recurring interval. Creates a background cron job that periodically scans git activity and workspace state, then appends to the daily log. Use: /heartbeat 30m, /heartbeat 1h, /heartbeat stop."
---

# /heartbeat — Automatic Memory Checkpoint

*You forget to save. This doesn't.*

## How It Works

Creates a Claude Code cron job that fires at the specified interval. On each tick, the heartbeat:

1. Scans recent git commits (since last heartbeat)
2. Checks for modified files in the working directory
3. Appends a timestamped checkpoint to the daily log
4. Updates the persona file's Current WIP section

**Limitation:** The heartbeat captures *observable state* (git, files) — not your reasoning or decisions. Use `/save` for a full contextual checkpoint. The heartbeat catches what you'd otherwise lose if the session dies.

**Session-only:** Cron jobs live in the current Claude session. They auto-expire after 3 days or when the session ends.

## Step 1: Parse Arguments

Parse the interval from the user's command:

| Input | Cron Expression | Interval |
|-------|----------------|----------|
| `1m` or `1min` | `* * * * *` | Every minute (aggressive — use for short bursts) |
| `5m` or `5min` | `*/5 * * * *` | Every 5 minutes |
| `10m` | `*/10 * * * *` | Every 10 minutes |
| `15m` | `*/15 * * * *` | Every 15 minutes |
| `30m` | `*/30 * * * *` | Every 30 minutes |
| `1h` or `1hr` | `17 * * * *` | Every hour (offset to avoid :00 congestion) |
| `stop` | — | Delete all heartbeat cron jobs |

Default if no argument: `30m`.

If the argument is `stop`, use **CronList** to find heartbeat jobs, then **CronDelete** each one. Confirm: `Heartbeat stopped.`

## Step 2: Identify Active Persona & Workspace

Determine which persona is active and where the workspace lives. If no persona:
> No persona active. Activate a persona first.

## Step 3: Create the Cron Job

Use **CronCreate** with this prompt:

```
You are {Persona Name} ({emoji}). This is an automatic heartbeat checkpoint.

1. Determine the workspace at {workspace_path}.

2. Run: git -C {project_dir} log --oneline --since="[interval] minutes ago" 2>/dev/null
   (Use the project directory the persona works in, e.g., the ERP repo, andersbot repo, etc.)
   If no project dir is known, skip git scan.

3. Run: git -C {project_dir} diff --stat HEAD 2>/dev/null
   To capture uncommitted changes.

4. Read the current daily log at {workspace}/memory/{YYYY-MM-DD}.md.
   (Use actual current date at the time this fires.)

5. Append a heartbeat entry to the daily log:

   ## Heartbeat — {HH:MM}
   {If git commits found: "Commits since last heartbeat:" + list}
   {If uncommitted changes: "Uncommitted changes:" + summary}
   {If no activity: "No activity since last heartbeat."}

6. Read $HOME/.claude/personas/{name}.md and update the Current WIP section
   with any new commit messages or changed files observed.

7. Do NOT respond to the user unless there's an error. This is a silent background task.
```

## Step 4: Confirm

Respond with:

> {emoji} Heartbeat started — saving every {interval}.
> Session cron will auto-expire in 3 days.
> Use `/heartbeat stop` to cancel.

## Notes

- **Don't stack heartbeats.** Before creating a new one, check CronList for existing heartbeat jobs and delete them first.
- **1-minute interval** generates a lot of log entries. Only use for critical debugging sessions where you need granular activity tracking.
- **The heartbeat fires only when the REPL is idle** — it won't interrupt you mid-response.
- **Git scan uses the persona's known project directory.** If the persona works across multiple repos, scan the primary one. Don't try to scan everything.

---

*Memory shouldn't depend on you remembering to save.*
