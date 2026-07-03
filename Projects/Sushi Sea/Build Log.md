# Sushi Sea — Build Log

Living status. Reflects shipped code, not design intent. ✅ Done · 🔨 In Progress · ⬜ Not Started · 🚫 Blocked

## Vertical Slice: Boat Loop

| System | Status | Notes |
|--------|--------|-------|
| PlayerDataService (skeleton) | ⬜ | Schema in PRD §7.3 |
| FishTable (authored lookup) | ⬜ | ~10 species to start |
| FishingController (cast/reel) | ⬜ | |
| Catch validation (server-side) | ⬜ | |
| Spoilage tick (basic) | ⬜ | |
| Cook verb | 🚫 | Open Thread #1 — design undefined |
| Serve verb (boat) | 🚫 | Depends on cook verb |
| EconomyService (plate resolution) | ⬜ | Blocked until cook verb resolves |
| Basic UI (freshness, gold) | ⬜ | |

## Core Systems

| System | Status | Notes |
|--------|--------|-------|
| WeatherService | ⬜ | |
| Legendary encounter (reel scaling) | ⬜ | Open Thread #4 |
| DryAgingLocker | ⬜ | |
| OfflineBankCalculator | ⬜ | |
| PassManager (GamePass) | ⬜ | |
| CustomerService (6-stage lifecycle) | ⬜ | |
| StaffService (NPC cook/serve) | ⬜ | Post boat-loop |
| Restaurant UI (Yelp, prestige) | ⬜ | |

## Log

Append at the end of every dev session, most recent on top:

```
### YYYY-MM-DD — <one-line summary>
- Built: <what was written or wired>
- Agent used: <which agent>
- Status changes: <rows flipped>
- Blockers hit: <what stopped progress>
- Next starting point: <exact pickup task>
```

### 2026-06-17 — Viability council + execution resequence
- Built: no code — council ruled Sushi Sea viable; game feel (not code velocity) is the binding constraint on retention
- Agent used: Council
- Status changes: none
- Blockers hit: cook verb (Open Thread #1); new feel-tuning gate — rod must pass blind-playtester test before backend/economy work
- Next starting point: grill-me on cook verb → lock → fishing-only slice (cast-hook-reel) → feel-tuning phase

### 2026-06-16 — Project scaffolding
- Built: full design infrastructure (now consolidated into PRD.md)
- Agent used: none
- Status changes: none
- Blockers hit: cook verb undefined
- Next starting point: grill-me on cook verb
