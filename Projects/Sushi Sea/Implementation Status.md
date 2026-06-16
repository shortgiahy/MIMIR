# Sushi Sea — Implementation Status

Living checklist. Updated at the end of every dev session. Reflects actual shipped code, not design intent.

Legend: ✅ Done · 🔨 In Progress · ⬜ Not Started · 🚫 Blocked (reason)

---

## Vertical Slice: Boat Loop

The minimum playable slice. Must close before any restaurant work begins.

| System | Status | Notes |
|--------|--------|-------|
| PlayerDataService (skeleton) | ⬜ | Schema in Architecture.md |
| FishTable (authored lookup) | ⬜ | ~10 species to start |
| FishingController (cast/reel) | ⬜ | |
| Catch validation (server-side) | ⬜ | |
| Spoilage tick (basic) | ⬜ | |
| **Cook verb** | 🚫 Blocked | Open Thread #1 — design undefined |
| **Serve verb (boat)** | 🚫 Blocked | Depends on cook verb |
| EconomyService (plate resolution) | ⬜ | Blocked until cook verb resolves |
| Basic UI (freshness, gold) | ⬜ | |

---

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

---

## Design Decisions Pending (from Open Threads)

| Thread | Blocks | Status |
|--------|--------|--------|
| #1 Cook verb | Boat loop, EconomyService, Omakase | 🚫 Open |
| #2 Menu variety / nigiri depth | Launch scope | 🚫 Open |
| #3 Economy model (5-row table) | Tuning | 🚫 Open |
| #4 Legendary phase structure | WeatherService | 🚫 Open |
| #5 Spoilage values | 24-48h return target | 🚫 Open |
| #6 Purchasing tiers, Yelp formula, etc. | Various | 🚫 Open |

---

## Build Log

*Append an entry at the end of every dev session. Most recent at the top.*

### 2026-06-16 — Project scaffolding + dev team setup
- Built: GDD v2, Architecture, Standards, Open Threads, CLAUDE.md, Implementation Status — full design infrastructure
- Agent used: N/A (design session, no code written)
- Status changes: None — all systems remain ⬜ or 🚫 Blocked
- Blockers hit: Cook verb (Open Thread #1) undefined — nothing in the vertical slice can move until this is resolved
- Next starting point: grill-me on cook verb → lock it → scaffold game code repo → begin BoatCookController
