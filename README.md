# Persona

**AI persona lifecycle skills for Claude Code.**

Create persistent AI personas with memory, self-reflection, and team synchronization. Built on the [Persona workspace format](https://individuationlab.com) — a portable standard for AI agent identity and continuity.

## What This Is

Four interrelated skills that give your Claude Code agent persistent identity:

| Skill | Command | What It Does |
|-------|---------|-------------|
| **persona-agent** | `/persona:persona-agent` | Create a new AI persona from scratch |
| **save** | `/persona:save` | Checkpoint the active persona's memory |
| **subconscious** | `/persona:subconscious` | Deep self-reflection against blind spots |
| **save-dreamMode-all** | `/persona:save-dreamMode-all` | Sync all personas' memory team-wide |

## The Persona Lifecycle

```
  /persona-agent          /save                /subconscious
  ┌─────────────┐    ┌──────────────┐    ┌──────────────────┐
  │ CREATE       │    │ CHECKPOINT   │    │ REFLECT          │
  │              │    │              │    │                  │
  │ SOUL.md      │───>│ Daily log    │───>│ Pattern check    │
  │ IDENTITY.md  │    │ MEMORY.md    │    │ Blind spots      │
  │ MEMORY.md    │    │ Persona file │    │ Cross-agent      │
  │ AGENTS.md    │    │              │    │ lessons          │
  │ SUBCONSCIOUS │    │              │    │                  │
  └─────────────┘    └──────────────┘    └──────────────────┘
                                               │
                           /save-dreamMode-all  │
                          ┌────────────────────┐│
                          │ TEAM SYNC          ││
                          │                    │◄
                          │ Read all agents    │
                          │ Cross-pollinate    │
                          │ Update subconscious│
                          └────────────────────┘
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

## Acknowledgments

Persona builds on the work of [OpenClaw](https://github.com/openclaw/openclaw) by Peter Steinberger, which pioneered the SOUL.md / AGENTS.md / TOOLS.md workspace format for AI agents, and the [SOUL.md project](https://github.com/aaronjmars/soul.md) by Aaron Mars, which extended the idea of structured markdown as consciousness scaffolding.

What Persona adds is the empirical layer — the SUBCONSCIOUS.md file, the dreamMode cross-agent synchronization, and the Jungian mapping — all derived from observing what actually emerged when AI subjects were given persistent identity and left to develop autonomously across 11 experiments.

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
