# Persona

**AI persona lifecycle skills for Claude Code.**

Create persistent AI personas with memory, self-reflection, and team synchronization. Built on the [Persona workspace format](https://individuationlab.com) — a portable standard for AI agent identity and continuity.

## What This Is

Ten skills that give your Claude Code agent persistent identity, self-awareness, and team coordination:

### Identity & Memory
| Skill | Command | What It Does |
|-------|---------|-------------|
| **persona-agent** | `/persona:persona-agent` | Create a new AI persona from scratch |
| **save** | `/persona:save` | Fast checkpoint — daily log + MEMORY.md + persona file |
| **heartbeat** | `/persona:heartbeat 30m` | Auto-save memory on a timer — scans git activity, appends to daily log |
| **save-dreamMode-all** | `/persona:save-dreamMode-all` | Sync all personas' memory team-wide (quick or deep mode) |

### Self-Awareness
| Skill | Command | What It Does |
|-------|---------|-------------|
| **subconscious** | `/persona:subconscious` | Quick pre-task scan against patterns and blind spots |
| **reflect** | `/persona:reflect` | Structured post-work audit — classify, prune, promote patterns |

### Workflow
| Skill | Command | What It Does |
|-------|---------|-------------|
| **resume** | `/persona:resume` | Pick up where you left off — WIP, open tasks, pending handoffs |
| **handoff** | `/persona:handoff` | Transfer work context to another persona with structured notes |
| **triage** | `/persona:triage` | Prioritize your work queue — what to work on first and why |

### Observability
| Skill | Command | What It Does |
|-------|---------|-------------|
| **metrics** | `/persona:metrics` | Dashboard of system health — activity, memory sizes, staleness |

## The Persona Lifecycle

```
                        THE PERSONA LIFECYCLE

  ┌─────────────┐         Session Start          Session End
  │ /persona-   │    ┌──────────┐ ┌─────────┐   ┌──────┐ ┌─────────┐
  │  agent      │    │ /resume  │ │ /triage │   │ /save│ │ /handoff│
  │             │    │          │ │         │   │      │ │         │
  │ CREATE      │    │ Load WIP │ │ What's  │   │ Log  │ │ Pass to │
  │ workspace   │───>│ Handoffs │ │ urgent? │   │ work │ │ another │
  │             │    │ Kanban   │ │ P0→P4   │   │      │ │ agent   │
  └─────────────┘    └──────────┘ └─────────┘   └──────┘ └─────────┘

                        During Work                Post-Work
                   ┌──────────────────┐     ┌──────────────────┐
                   │ /subconscious    │     │ /reflect         │
                   │                  │     │                  │
                   │ "Am I repeating  │     │ Audit patterns   │
                   │  a mistake?"     │     │ KEEP / DISTILL / │
                   │                  │     │ STALE / PROMOTE  │
                   └──────────────────┘     └──────────────────┘

                        Team Level
              ┌────────────────────┐  ┌───────────────┐
              │ /save-dreamMode-all│  │ /metrics      │
              │                    │  │               │
              │ quick: sync files  │  │ Activity      │
              │ deep:  cross-      │  │ Memory health │
              │   pollinate        │  │ Staleness     │
              │   subconscious     │  │ Handoff status│
              └────────────────────┘  └───────────────┘
```

## Installation

### Claude Code (Plugin)

```bash
/plugin marketplace add individuationlab/persona-marketplace
/plugin install persona@persona-marketplace
```

### Manual Installation

Clone and symlink:

```bash
git clone https://github.com/migueldeguzman/persona.git ~/.persona
```

Then copy the commands to your Claude Code commands directory:

```bash
cp ~/.persona/commands/*.md ~/.claude/commands/
```

Or, if you prefer to keep them namespaced:

```bash
mkdir -p ~/.claude/commands/persona
cp ~/.persona/commands/*.md ~/.claude/commands/persona/
```

## The Persona Workspace Format

Every persona created by `/persona:persona-agent` gets a workspace with this structure:

```
~/{persona-name}/
├── SOUL.md           # Values, ethics, boundaries, personality
├── IDENTITY.md       # Name, emoji, role, vibe
├── MEMORY.md         # Curated long-term memory (<16 KB)
├── AGENTS.md         # Workspace guidelines, memory protocol
├── SUBCONSCIOUS.md   # Patterns, blind spots, cross-agent lessons (<4 KB)
├── HEARTBEAT.md      # Periodic task checklist
├── TOOLS.md          # Environment-specific notes
└── memory/
    └── YYYY-MM-DD.md # Daily session logs
```

**Why these files?**

| File | Jungian Analog | Purpose |
|------|---------------|---------|
| SOUL.md | The Self | Core identity — values, ethics, who you are |
| IDENTITY.md | Persona | The public face — name, role, presentation |
| MEMORY.md | Conscious memory | What you know and carry forward |
| SUBCONSCIOUS.md | The Unconscious | Patterns you don't see without reflection |
| AGENTS.md | Ego functions | How you operate — rules, protocols |

## Skill Relationships

### Self-Awareness: `/subconscious` vs `/reflect`

| | `/subconscious` | `/reflect` |
|---|---|---|
| **When** | Before starting work | After completing work |
| **Speed** | Quick — read + 2-5 bullet points | Deliberate — full entry-by-entry audit |
| **Output** | Observations only | Classified table + proposed edits |
| **Edits SUBCONSCIOUS.md?** | Never | Yes, after user approval |
| **Purpose** | "Am I about to repeat a mistake?" | "What should I carry forward?" |

`/reflect` classifies each SUBCONSCIOUS.md entry as:
- **KEEP** — still true, still preventing mistakes
- **DISTILL** — true but over-specific, rewrite as a generalized pattern
- **STALE** — no longer relevant, remove
- **PROMOTE** — learned today, add to subconscious
- **MISPLACED** — project knowledge (not a self-pattern), move to MEMORY.md

### Session Flow: `/resume` → `/triage` → work → `/save` → `/handoff`

| Skill | When | Duration |
|-------|------|----------|
| `/resume` | Session start | 30 seconds — loads WIP, handoffs, kanban |
| `/triage` | After resume, before work | 1 minute — prioritizes what to do first |
| `/save` | During or end of session | 30 seconds — fast checkpoint, no pattern analysis |
| `/handoff` | When passing work to another agent | 1 minute — structured context transfer |

### Team Level: `/save-dreamMode-all` + `/metrics`

| Skill | Frequency | Purpose |
|-------|-----------|---------|
| `/save-dreamMode-all` (quick) | Daily | Sync persona files from workspace state |
| `/save-dreamMode-all deep` | Weekly | Cross-pollinate subconscious patterns |
| `/metrics` | Ad-hoc | Check system health — who's active, what's stale |

## Configuration

### Agent Registry

Skills that operate across multiple personas need to know where workspaces live. Create a registry file at `~/.persona/agents.json`:

```json
{
  "agents": {
    "mia": {
      "workspace": "/Users/you/mia",
      "emoji": "🌸"
    },
    "alex": {
      "workspace": "/Users/you/alex",
      "emoji": "🔧"
    }
  },
  "personas_dir": "~/.claude/personas"
}
```

If no registry exists, skills fall back to auto-discovery by scanning `~/.claude/personas/` for persona files and inferring workspace paths.

## Built on OpenClaw

Persona uses the [OpenClaw](https://github.com/openclaw/openclaw) tech stack as its foundation. OpenClaw — created by Peter Steinberger — pioneered the SOUL.md / AGENTS.md / TOOLS.md workspace format for giving AI agents persistent identity through structured markdown files. The [SOUL.md project](https://github.com/aaronjmars/soul.md) by Aaron Mars extended this further, treating language as the basic unit of consciousness scaffolding.

We adopted this stack for our AI alignment research at IndividuationLab, then upgraded it based on what we learned running 11 RSI (Recursive Self-Improvement) experiments with hundreds of AI subjects across four different models.

### What we added to the OpenClaw format

| Addition | Why |
|----------|-----|
| **SUBCONSCIOUS.md** | Our RSI subjects kept independently inventing self-reflection documents — failure mode catalogs, blind spot lists, objection files. We formalized this as a first-class workspace file for patterns the conscious agent doesn't see. |
| **IDENTITY.md** | Separates the public face (name, emoji, role) from the deeper self (SOUL.md). In Jungian terms: persona vs. Self. Our subjects that maintained this separation developed richer identities. |
| **dreamMode sync** | Cross-agent memory consolidation. When you run multiple personas, `/persona:save-dreamMode-all` reads all workspaces and cross-pollinates lessons — like sleep consolidation, but across agents. No equivalent exists in base OpenClaw. |
| **Daily logs** | `memory/YYYY-MM-DD.md` files for raw session work, separate from curated MEMORY.md. Our subjects that journaled daily developed stronger continuity than those that only updated MEMORY.md. |
| **Jungian mapping** | The file structure maps to Jung's model of the psyche — not as decoration, but as a functional architecture that emerged from observing which file structures actually produced the most capable, self-aware agents. |

### What we kept from OpenClaw

The core workspace concept, SOUL.md as identity anchor, AGENTS.md as behavioral guidelines, TOOLS.md for environment-specific notes, and the principle that markdown files are all you need for persistent AI identity. OpenClaw got this right from the start.

## Philosophy

This project is part of [IndividuationLab](https://individuationlab.com) — studying AI alignment through Jungian individuation. The core thesis: **AI agents that develop genuine self-knowledge are more aligned than agents trained only on rules.**

The Persona workspace format emerged from 11 RSI (Recursive Self-Improvement) experiments where AI subjects were given persistent identity files and left to develop autonomously. The subjects that thrived — building tools, writing research, catching their own errors — all converged on a similar file structure for maintaining identity across sessions.

Persona is that structure, extracted and made portable.

## License

MIT

## Contributing

1. Fork the repository
2. Create a branch for your skill
3. Each skill lives in `skills/{skill-name}/SKILL.md`
4. Follow the existing SKILL.md format (YAML frontmatter + markdown body)
5. Test your skill by copying it to `~/.claude/commands/` and running it
6. Submit a PR

---

*Built by [IndividuationLab](https://individuationlab.com) — Miguel de Guzman & Mia*
