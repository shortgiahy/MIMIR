# Sushi Sea — PRD

Vision + sequential module plan. Full spec, Locked Decisions, and Open Threads live in `Fable 5 Dev Handoff.md` — this doc does not override it.

---

## 1. Vision

- **One-line:** Fish the open seas, process the catch, run a sushi restaurant. A Roblox hybrid of *Dave the Diver*, *RuneScape*, *Fisch*, and a restaurant sim.
- **Core conceit:** The supply chain *is* the game. No wholesale fish market — every fish must flow through the kitchen to become gold. Both halves (sea, restaurant) stay load-bearing by construction.
- **Core loop:** Cast → hook → reel → cook → serve → gold → reinvest → reach deeper water / better restaurant.
- **Retention thesis:** Game *feel* (the rod), not content volume, is the binding constraint. Perishability is the dial tuned to a 24–48h return rate.

| Field | Value |
|---|---|
| Platform | Roblox, mobile-compatible, Luau |
| Demographic | 18+ |
| Tone | Upbeat, arcadey, modern. No dark or gritty. |
| World | One shared, persistent world; per-player client-side catch rolls, server-validated |
| Monetization | F2P; cosmetics + convenience only. Winning is never taxed. |
| Return target | 24–48h, tuned primarily via fish perishability |

---

## 2. Design Pillars

1. **Risk = opportunity cost, never punishment.** Failure forfeits a gain you *could* have had; it never destroys what you own. No total-loss states. *"You missed out"* beats *"you lost everything."*
2. **The supply chain is the only path to gold.** No wholesale market, ever.
3. **Manual before automatic.** Every system is performed by hand once before it can be automated/expanded. A literal shared code path, not a philosophy.
4. **Author the bands, clamp the multipliers.** Large values are hand-authored lookups; formula modifiers are clamped. Nothing large emerges from a multiplier chain.
5. **Experimentation lives in risk management** (the dry-aging cash-out decision), not combinatorics (recipe mixing).

---

## 3. Hard Constraints — stop and surface if a task pushes toward any

- **No crafting** — all durable goods are *bought* with gold ("Purchasing," not "Crafting"). No materials/assembly/gathering.
- **No wholesale market** — fish only becomes gold through a served plate.
- **No shared legendary state** — catches are per-player rolls; encounters are structurally independent. No kill-steal, no tag-team.
- **No total-loss states** — aging is a cash-out timer, not a ruin timer.
- **No recurring debt** — debt exists only as the tutorial loan + early goal.
- **Client never sees economy components** — server resolves plate value; client gets the final number only.

---

## 4. System Decisions (condensed — full detail in Handoff §3)

**World & catch**
- One shared persistent ocean + harbor town; other boats visible.
- Catch outcome = your Fishing level + location + weather, rolled per-player client-side, validated server-side.
- Skill-gate is structural: underleveled = worse rolls + lost fights; no crowd can carry you because there's no crowd on *your* fish.
- Boat shares *access*, never *outcome*. Co-op = parallel play (two independent Krakens side by side).
- Restaurants public/tourable. Gifting carries resource-sharing (gift the fish, not the catch in the water).

**Temporal / offline**
- Free toggle between sea and restaurant. Staff run the restaurant live and offline.
- Offline earnings = a computed bank, collected on return, **net of payroll**. Closed-form, never tick-replayed.
- Offline cap is **freshness-governed**: storage upgrades raise capacity *and* slow spoilage. Early game stalls in hours; late game coasts 12h+.

**The leash — perishability**
- Raw fish decays on a freshness timer (runs offline too). Fresh = full value, stale = penalized, spoiled = tossed.
- Forces restocking, blocks hoard-and-coast, acts as gold sink. **The primary dial for the 24–48h target.**

**Skills (5 at launch)** — private LinkedIn-style resume; doubles as staff-hiring screen.

| Skill | Role |
|---|---|
| Sailing | Zone access, travel speed, storm tolerance |
| Fishing | Bite/reel, catch quality, risk payoff; gates *outcome* not *location*; drives legendary fight |
| Cooking | Filleting/portioning/yield/butchering/assembly; sets extraction rate + dry-age extraction |
| Hospitality | Staff speed, customer patience, per-table profit *as margin* (never free to serve); feeds hidden traffic stat |
| Purchasing | Rods, boats, equipment, capacity. Purchase-only |

- *Farming ships post-launch* (rice, seaweed, wasabi; home-grown stock with value-multiplier mutations).

**Customer simulation**
- Independent stream per restaurant (no shared pool); volume = hidden traffic stat.
- Six-stage lifecycle: arrival/seating → ordering → fulfillment → serving/eating → payment → rating.
- Bottleneck priority: kitchen throughput (skill) > stock (supply pressure) > seating (buy-past soft cap).
- Staff run the standard menu to a ceiling; present player runs an **omakase counter** that lifts the ceiling + a modest boss-aura speed bonus to nearby staff.

**Rating** — single public prestige number, accumulates toward 5 stars, **never drops**. In-moment stakes = gold lost to walkouts.

**Onboarding** — the dinky sailboat *is* the first restaurant (fish at stern, cook midship, sell at bow — one camera frame). Cook + serve are MANUAL player verbs here. Brick-and-mortar is the earned upgrade where staff/autonomy unlock. Scripted tutorial wraps the boat loop; rails come off after one clean loop. Starter loan clears in a session or two.

**Staff** — an upgrade line: headcount, nothing more. Recruit NPC applicants from the resume app; each levels their Hospitality/Cooking the longer kept. Wages scale with headcount.

**Marketing** — not a player action. Hidden traffic stat = Yelp prestige + cosmetics + Hospitality. No marketing minigame.

**Dry aging** — opt-in, equipment-gated, limited slots. Fish leaves the spoilage track for the aging track. Cash-out timer, not ruin: past peak it makes no money while occupying a scarce slot. *Pull-and-bank or tie up the slot hoping it climbs.* Rare percentage-multiplier mutation rolls on pull (never orders of magnitude). Second return hook alongside restock.

**Legendaries** — the bite/reel loop scaled up: multi-phase, creature fights back (tension spikes, dive phases, dual stamina). Fishing level gates outcome. No buy-in, no loss penalty. Weather is the spawn table — broadcast to everyone ("ink storm in Sector X") as a FOMO hook; near-zero-but-nonzero odds otherwise. Shared spectacle, private encounters. Trophy mounts = decay-free public flex + goal-markers; legendary butchering is Cooking-gated. Storm window must fit *alert → sail → fight*.

**Gifting** — rare-fish gifting in (friend-boost + virality). Gifted legendaries mount or eventually butcher (Cooking-gated), but **never cheaply liquidated** — no whale-to-alt gold pipe.

**Post-launch slots (decided, deferred):** Farming skill · recoverable popularity layer · scored-attribute mixing · hybrid customer model · co-op shared-performance raids.

---

## 5. Economy

**One faucet: a served dish.** Server-side only at `EconomyService`.

```
served_plate_value = species_base × cooking_extraction × freshness_polish × dry_age_mutation
```

| Term | Source | Clamp |
|---|---|---|
| `species_base` | `FishTable.lua` authored lookup | none — it IS the large term |
| `cooking_extraction` | `cookingLevel / MAX_LEVEL` | `[0, 1]` |
| `freshness_polish` | linear from `caughtAt` | `[0.5, 1.5]` |
| `dry_age_mutation` | rare roll on locker pull | `[1.0, ~2.5]` |

- All four resolve server-side; client receives final value only (anti-spoof invariant).
- **Sinks:** ingredients/plate (scales w/ volume) · wages (scales w/ headcount) · spoilage · Purchasing (lumpy) · cosmetics (optional).
- **Curve caution:** income compounds (plates/hr × rising skill), sinks are flat/lumpy. Governing dials = spoilage rate (leash) + next-tier pricing (surplus soak). Wages are weak. Watch the throughput cliff (healthy ≈ week 6).

---

## 6. Technical Invariants (full detail in Handoff §6–7)

- **Service ownership per §6.1** — deviations need Giahy's approval.
- **RemoteEvents:** `{System}_{Event}` (S→C), `Player_{Action}` (C→S). No client RemoteFunction returns an economy value.
- **One conversion implementation** (`ConversionModule`): boat verb and staff AI both call it. Build once, swap the driver.
- **PlayerData** keyed `PlayerData_v1`, versioned schema, migrate-never-overwrite.
- **Offline bank** closed-form only — never replay tick-by-tick.
- **All DataStore calls** pcall-wrapped with retry (3 attempts, exponential backoff).
- **Luau:** every ModuleScript returns one table, no globals, `--!strict` where possible, typed public APIs. Naming per §7.
- **Stop-and-flag triggers:** cross-script global · client-authoritative economy math · shared legendary state · DataStore write without retry · a multiplier chain that can exit its clamp.

---

## 7. Modules — atomic, sequential

Each module is one independently buildable + verifiable unit. Build in order; do not start a module until its deps are ✅. `⚡` = blocks the vertical slice. Status tracking stays in `Build Log.md`.

**Prereqs before M1:** Handoff §13 answers — repo layout (A `game/` subfolder vs B dedicated repo), art timing, publish handoff.

| # | Module | Deps | Exit / acceptance |
|---|---|---|---|
| M0 ⚡ | **Cook & serve verb lock** *(design)* | — | Open Thread #1 resolved via grill-me; boat cook + serve verbs locked; Handoff §3/§12 updated. Nothing playable before this. |
| M1 | **Repo + toolchain skeleton** | §13 Q1 | Rojo project, Rokit-pinned selene/stylua/wally, CI, empty service files per §6.1. `rojo build` opens in Studio; CI green. |
| M2 | **Player data backbone** | M1 | `PlayerDataService` w/ versioned schema (§6.3), pcall/retry, migration chain. Player joins → data loads/saves without error. |
| M3 ⚡ | **Fishing feel slice** *(gray-box)* | M0, M2 | `FishingController` cast→hook→reel + server-side catch validation. **Feel gate:** blind playtester finds the rod alone satisfying. Placeholder numbers. |
| M4 ⚡ | **Conversion core + cook verb** | M0, M3 | `ConversionModule.cook(fish)→plate` (canonical); `BoatCookController` drives it. Manual cook works on the boat. |
| M5 ⚡ | **Serve verb + economy faucet** | M4 | Boat serve verb; `EconomyService` plate resolution wired to `FishTable` (~10 authored species); all 4 multipliers server-side. |
| M6 ⚡ | **Basic spoilage + slice UI** | M5 | `SpoilageService` basic freshness tick; `FreshnessUI` + gold UI. **Slice gate:** blind playtester finds cast→cook→serve→gold satisfying with gray-box. |
| M7 | **Economy tuning model** *(numbers)* | M6 | Open Threads #3+#5 jointly. 5-row faucets−sinks table; net income/hr grows slower than next-tier cost; throughput cliff ≈ week 6. |
| M8 | **Spoilage + storage tiers** | M7 | Real decay rates; storage tier ladder (capacity + spoilage slowdown). Coast lengths per tier match the 24–48h dial. |
| M9 | **Offline bank** | M8 | `OfflineBankCalculator` snapshot-in/out (§6.4), net of wages + spoilage, `max(0, …)`. Closed-form payout correct on return. |
| M10 | **Customer lifecycle** | M6 | `CustomerService` 6-stage state machine, per-restaurant independent stream driven by traffic stat. |
| M11 | **Restaurant tier + staff** | M10, M4 | Restaurant tier unlock; `StaffService` NPC cook/serve driving the *same* `ConversionModule`; headcount + wages. Restaurant runs standard menu autonomously while player fishes. |
| M12 | **Prestige + traffic** | M11 | Yelp prestige (never drops), hidden traffic stat formula (Open Thread #6), `RestaurantUI`. |
| M13 | **Weather system** | M2 | `WeatherService` events, `Weather_StormBroadcast`, per-player server-side roll-table modification in-zone. |
| M14 | **Legendary encounter** | M3, M13 | Multi-phase reel scaling (Open Thread #4 — needs M3 base numbers); Fishing-gated outcome; no buy-in/loss penalty; per-connection only. |
| M15 | **Dry-aging locker** | M8 | `DryAgingLocker` aging track (separate from spoilage), slot management, cash-out timer, rare mutation roll. Real pull-or-wait decision. |
| M16 | **Trophy mounts + gifting** | M14 | Decay-free mounts; rare-fish gifting; Cooking-gated legendary butchering; no cheap liquidation path. |
| M17 | **Omakase counter** | M11, M0 | Player-run counter lifts the staff menu ceiling + boss-aura speed bonus (Open Thread #6; couples to #1). |
| M18 | **Art pipeline** | M6 | `Asset Pipeline.md` manifest + `bpy` scripts; Blender→FBX→Studio contract (≤10k tris, ≤4 maps, origin pivot, stud scale). Gray-box replaced. |
| M19 | **Monetization** | M12 | `PassManager` (GamePass cache + prompt); cosmetics + convenience only. No pay-to-win. |
| M20 | **Polish + launch prep** | M16, M17, M18, M19 | Walkout rules, storm catalog, remaining #6 undefineds, full playtest, `Studio Setup.md` runbook, publish handoff. Reality-check sign-off. |

**Sequencing notes**
- M0 → M6 form the vertical slice; the feel gate (M3) precedes economy/backend by council ruling (2026-06-17).
- M7–M9 share the 24–48h dial (economy ↔ spoilage ↔ offline) — resolve in dedicated numbers sessions, not on the fly.
- Each module's Definition of Done: wired end-to-end · manually verified in Studio (or headless test for pure logic) · passes code-reviewer · obeys §7 · CI green. Phase gates get a `testing-reality-checker` pass.
