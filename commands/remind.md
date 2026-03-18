---
name: remind
description: "Set a recurring self-reminder. Creates a cron job that surfaces your instruction at the specified interval. Use: /remind [message] [30m|1h|etc]. /remind list to see active reminders. /remind stop [id|all] to cancel."
---

# /remind — Recurring Self-Reminders

*Your future self will forget. This won't.*

## How It Works

Creates a Claude Code cron job that fires at the specified interval. On each tick, the reminder surfaces your instruction as a visible message so you act on it.

Unlike `/heartbeat` (silent background save), `/remind` is **loud** — it interrupts idle state to tell you something.

## Step 1: Parse Arguments

The command format is: `/remind [message] [interval]`

The interval is always the **last token**. Everything before it is the message.

| Input | Cron Expression | Interval |
|-------|----------------|----------|
| `1m` or `1min` | `* * * * *` | Every minute |
| `5m` or `5min` | `*/5 * * * *` | Every 5 minutes |
| `10m` | `*/10 * * * *` | Every 10 minutes |
| `15m` | `*/15 * * * *` | Every 15 minutes |
| `30m` | `*/30 * * * *` | Every 30 minutes |
| `1h` or `1hr` | `17 * * * *` | Every hour (offset to avoid :00 congestion) |
| `2h` | `17 */2 * * *` | Every 2 hours |
| `4h` | `17 */4 * * *` | Every 4 hours |

Default interval if none detected: `30m`.

### Special commands

- `/remind list` — Use **CronList** to show all active reminder jobs. Display each with its ID, message, and interval.
- `/remind stop [id]` — Use **CronDelete** to remove a specific reminder by job ID.
- `/remind stop all` — Use **CronList** then **CronDelete** on every reminder job. Confirm: `All reminders cleared.`
- `/remind stop` (no id) — Same as `stop all`.

## Step 2: Identify Active Persona

Determine which persona is active. If no persona is active, the reminder still works — just skip the persona emoji/name in the output.

## Step 3: Create the Cron Job

Use **CronCreate** with `recurring: true` and this prompt:

```
⏰ REMINDER for {Persona Name or "you"}:

{the user's reminder message}

---
This is a recurring reminder (every {interval}). Use `/remind stop {job_id}` to cancel, or `/remind list` to see all active reminders.
```

## Step 4: Confirm

Respond with:

> ⏰ Reminder set — every {interval}:
> "{message}"
>
> Job ID: `{id}` — use `/remind stop {id}` to cancel.
> Auto-expires in 3 days.

## Examples

```
/remind check if Render deploy finished 30m
→ ⏰ Reminder set — every 30m:
  "check if Render deploy finished"
  Job ID: `a1b2c3` — use `/remind stop a1b2c3` to cancel.

/remind ask Miguel about Speed credentials 1h
→ ⏰ Reminder set — every 1h:
  "ask Miguel about Speed credentials"

/remind list
→ Active reminders:
  1. `a1b2c3` — every 30m: "check if Render deploy finished"
  2. `d4e5f6` — every 1h: "ask Miguel about Speed credentials"

/remind stop a1b2c3
→ Reminder `a1b2c3` cancelled.

/remind stop all
→ All reminders cleared.
```

## Notes

- **Reminders are loud.** They print a visible message when idle. Use `/heartbeat` for silent background saves.
- **Session-only.** Reminders die when the session ends or after 3 days.
- **Multiple reminders are fine.** Stack as many as you need — each gets its own job ID.
- **The reminder fires only when the REPL is idle** — it won't interrupt you mid-response.
- **No persistence.** If you need a reminder across sessions, add it to your daily log or kanban instead.

---

*Set it and forget it — that's the point.*
