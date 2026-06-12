# Skill — /swarm

**Trigger:** `/swarm <task>`
**Location:** `~/.claude/skills/swarm/SKILL.md`

---

## What It Does

Orchestrates coordinated teams of sub-agents working in parallel on a shared task. Agents communicate via a shared filesystem workspace — MIMIR routes messages between them. Supports custom agent teams, multi-wave dependency chains, and the full Research → Council → Chairman intelligence pipeline.

---

## Modes

| Mode | When to use |
|------|-------------|
| `research-only` | Just run `/research` |
| `council-only` | Just run `/council` |
| `pipeline` | Full stack: Research informs Council, Council advises Chairman |
| `custom` | User-defined agent team with custom roles |

---

## How It Works

**Shared Workspace**

All agents read/write to `/tmp/swarm-<id>/`. Each agent has its own directory with `output.md`, `inbox/`, and `outbox/`. MIMIR reads outboxes between waves and routes messages to the right inboxes before launching the next wave. Agents never communicate directly.

**Custom swarms** — MIMIR proposes the agent team (role, type, mission, wave), waits for confirmation, then launches each wave in parallel. User defines roles if they want.

**Pipeline** — Research runs first (5 fan-out agents + 3 verifiers + synthesizer). Output is injected into each Council member's context. Council deliberates and produces a ruling. Chairman (MIMIR) delivers the final read to Giahy.

---

## Pipeline Flow

```
/research → Sources/YYYY-MM-DD <Topic>.md
                ↓
           handoff/research-output.md
                ↓
/council (members briefed with research findings)
                ↓
           handoff/council-output.md
                ↓
        Chairman's Ruling (MIMIR → Giahy)
```

---

## Agent Types

| Type | Use for |
|------|---------|
| `Explore` | Read-only codebase search, file discovery |
| `general-purpose` | Web research, open-ended investigation |
| `claude` | Code writing, editing, judgment-heavy work |
| `Plan` | Architecture, implementation planning |

---

## Constraints

- Max 8 agents per wave
- All independent agents in a wave launch in a single parallel message
- Each agent prompt is fully self-contained
- MIMIR is always the router and always the final recipient
- Chairman rules. Council advises. Research informs. Order never reverses.

---

## Example

```
/swarm Should I pursue the UC Berkeley application over MIT this cycle?
```

MIMIR runs the full pipeline: research the application landscape → council debates tradeoffs → Chairman rules.
