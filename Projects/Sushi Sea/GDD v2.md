# Sushi Sea — Game Design Handoff (v2)

**A Roblox hybrid of *Dave the Diver*, *RuneScape*, *Fisch*, and a restaurant sim.**
Fish the open seas, process the catch, run a sushi restaurant. The supply chain *is* the game.

| Field | Value |
|---|---|
| Platform | Roblox, mobile-compatible, Luau |
| Demographic | 18+ |
| Tone | Upbeat, arcadey, modern (*Dave the Diver*). No dark or gritty. |
| Algorithm target | 24–48h return rate |
| Monetization | F2P; cosmetics and convenience only. Nobody is taxed for winning. |
| Architecture | **One shared, persistent world. Fishing outcomes roll per-player, client-side.** |

---

## How to use this document

This is a context handoff from an ongoing design process run with the **grill-me** method: interview one decision at a time, walk the dependency tree, and attach a recommended answer to every question. **Locked Decisions** are settled — do not relitigate unless the user reopens them. **Open Threads / Loose Ends** at the bottom are the live work, listed in resolution order.

This version (v2) supersedes the original handoff. Several Locked Decisions from v1 were **overridden** during the most recent session. Those overrides are called out loudly in the next section because they contradict the prior doc — if you are cross-referencing v1, trust v2.

This document is written to be detailed enough for **Claude Code to begin implementation**. Where something is *locked*, build to it. Where something is *recommended / first-pass*, treat it as a default to confirm, not gospel. Where something is an *open loose end*, do **not** invent a resolution — surface it to the user.

---

## ⚠️ Overrides from v1 (read first)

These five decisions changed and directly contradict the original handoff:

1. **World is one shared persistent world — NOT solo-instanced.** v1 said "Sea: solo-instanced with optional co-op invite." That is dead. The sea is a single shared world (Fisch-style). The skill-gate is preserved by a different mechanism (see below).
2. **Catches are per-player, client-side rolls.** Two players on the same boat cast into the same water; one may reel a salmon while the other hooks a Kraken. There is no shared catch, no contested encounter, no kill-stealing — these are *structurally impossible*, not balanced away.
3. **Aging is a cash-out timer, NOT a ruin timer.** v1 framed dry-aging as "past peak ruins entirely (total loss)." That is overruled. Aging carries **no total-loss state**. Risk in this game is **opportunity cost, not punishment** — game-wide stance (see Design Voice, Principle 1, revised).
4. **Debt is tutorial-and-early-goal only — NOT a permanent risk pillar.** v1 made opt-in debt a permanent financing system. It is now scoped to the onboarding loan and the early overarching goal. It is not a recurring late-game mechanic.
5. **"Crafting" skill is really "Purchasing."** There is **no crafting** in the game. All durable goods (rods, boats, equipment, capacity, restaurant tiers) are bought with gold. Rename the skill; drop all crafting/materials/gathering implications.

A sixth clarification (not a contradiction, but a tightening): **offline service is freshness-governed, not storage-governed.** Storage upgrades raise capacity *and* slow spoilage; the real cap on an offline coast is how fast stock spoils, not an abstract storage number.

---

## Design Voice (the throughline)

Five principles resolve most new questions on their own. **Principle 1 was revised this session.**

1. **Risk is always available, never forced, and always trades for acceleration — and risk means opportunity cost, not punishment.** Storm zones, the aging locker, the tutorial loan, and legendary hunts all express this. **Critically: failure forfeits a *gain you could have had*, never destroys something you already own.** A botched legendary costs nothing but the moment. An over-aged fish doesn't ruin — it just sits there not making money. This is the most on-tone reading of "upbeat, arcadey, no dark/gritty": *you missed out* beats *you lost everything*. New risk surfaces must obey this.
2. **The supply chain is the only path to gold.** There is no wholesale fish market. Every fish must flow through the kitchen to become money, so both halves stay load-bearing by construction.
3. **Perform every system manually once before it can be automated or expanded.** Manual cast before bite depth. Manual cook and manual serve on the boat before hiring cooks and servers at the restaurant. Manual sale before staff. Manual cook before the omakase ceiling.
4. **Author the bands, clamp the multipliers.** Large values are hand-authored lookups. Formula-driven modifiers are clamped. Nothing large is allowed to emerge from a chain of multiplications.
5. **Experimentation lives in risk management, not combinatorics.** The "how do I beat my current best" loop is the dry-aging cash-out decision, not recipe mixing.

---

## Locked Decisions

### Core thesis
Fish in open seas, process the catch, run a sushi restaurant. The supply chain is the game; neither half works alone. No wholesale market means the kitchen is the only door from fish to gold.

### World architecture
- **One shared, persistent world.** All players share the same ocean and the same harbor town. Other boats are visible on the horizon; the world feels populated. This is the *Fisch* model.
- **Fishing outcomes are independent per-player client-side rolls.** What you hook is determined by *your* roll (your Fishing level, your location, current weather), not by a shared spawn you compete for.
- **The skill-gate is structural.** Underleveled players simply roll worse outcomes and lose hard fights; they cannot be carried past the gate by a crowd because there is no crowd on *their* fish.
- **The boat shares *access*, never *outcome*.** A low-Sailing friend can ride a high-level player's storm-rated ship into a zone they couldn't reach alone — but they bring their *own* Fishing skill to the catch.
- **Co-op is parallel play, never shared performance.** Friends on one boat fight *two independent* Krakens side by side. No tag-team reeling of a single creature.
- **Restaurants are public and tourable.** No private instances.
- **Gifting carries the resource-sharing load.** You can gift the rare fish afterward — not the catch in the water.

### Temporal model
Player freely toggles between sea and restaurant. NPC staff run the restaurant live and while offline. Offline service is throttled and capped; the cap is governed by **freshness/spoilage**, with capacity scaling on the storage/restaurant upgrade line (early game stalls in a few hours; late game can coast 12h+). Offline earnings are a **computed bank** collected on return, **net of payroll**.

### The leash: perishability
Raw fish decays on a freshness timer. Forces restocking, blocks hoard-and-coast, acts as a gold sink, and is **the dial tuned to hit the 24–48h return target**. Fresh serves at full value; stale is penalized; spoiled is tossed. Spoilage runs while offline.

### Scoreboard and social
- **Restaurant prestige** (public Yelp app) and the **collection dex** are the public flex.
- **Skills** live on a private LinkedIn-style resume app, later doubling as the **staff-hiring screen**.

### Skills (5 at launch)

| Skill | Role |
|---|---|
| **Sailing** | Zone access, travel speed, storm tolerance. |
| **Fishing** | Bite/reel, catch quality, risk payoff in dangerous water. **Gates outcome, not location.** Drives the legendary fight. |
| **Cooking** | Absorbs filleting, portioning, yield, large-creature butchering, dish assembly. Sets the **extraction rate** and **dry-aging extraction rate**. |
| **Hospitality** | Staff speed, customer patience, per-table profit **as margin** (never makes a table free to serve). Feeds the hidden traffic stat. |
| **Purchasing** *(was "Crafting")* | Rods, boats, restaurant equipment, capacity. **Purchase-only — no crafting exists.** |

*Farming ships post-launch* (rice, seaweed, wasabi, garnishes; premium home-grown stock with random value-multiplier mutations).

### Access model
Fishing level gates *outcome*, not *location*. Anyone can sail into a storm; underleveled it wrecks you, high-level it pays. Ship upgrades are a parallel capability line (cargo, speed, storm survival, area access), bought via Purchasing.

### Customer simulation
- **Independent stream per restaurant** (no finite shared pool). Volume driven by the hidden traffic stat.
- **Six-stage lifecycle:** arrival/seating → ordering → fulfillment → serving/eating → payment → rating.
- **Difficulty distribution:** kitchen throughput is primary bottleneck (skill expression); stock is secondary (supply-chain pressure); seating is a buy-past soft cap.
- **Staff run the standard menu to a ceiling; the player, when present, runs an omakase counter** that lifts the ceiling, plus a modest boss-aura speed bonus to nearby staff.

### Rating
Single prestige number that accumulates toward 5 stars, **never drops**, public. A recoverable popularity layer is **shelved as a known expansion**. In-the-moment stakes lean on gold lost to walkouts.

### Onboarding & the boat → restaurant transition
- **The dinky sailboat is the first restaurant:** fish at the stern, cook midship, sell at the bow — the whole loop in one camera frame.
- **On the boat, cooking and serving are MANUAL player verbs.**
- **Brick-and-mortar is the earned upgrade** and the moment staff and autonomy unlock. **Once you own a restaurant, you hire cooks and servers (headcount) who perform cooking and serving automatically.**
- **Tutorial:** scripted tutorial wraps the boat loop; rails come off after the loop closes once.
- **Starter loan** is a narrative on-ramp that clears in a session or two — not a late-game pillar.

### Economy

**One faucet: a served dish.**

`served_plate_value = species_base × cooking_extraction × freshness_polish × dry_age_mutation`

| Component | Behavior |
|---|---|
| **Species base price** | Hand-authored per species. ~5–10 common, scaling with rarity. **The only large term.** |
| **Cooking extraction rate** | Cooking level = fraction of ceiling realized. Novice + legendary = mediocre plate. |
| **Freshness polish** | Clamped multiplier (target ~0.5×–1.5×). |
| **Dry-aging mutation** | High-variance percentage multiplier, rare by design. Percentage multipliers only — never orders of magnitude. |

**Sink stack:**
- **Ingredients per plate** — mandatory, scales with volume.
- **Staff wages** — mandatory, scales with headcount. Low early; real line item at brick-and-mortar tier.
- **Spoilage** — mandatory, drains inventory upstream of income.
- **Purchasing** (rods, boats, equipment, storage, restaurant tiers) — large, lumpy, player-initiated.
- **Cosmetics and expansion** — aspirational, uncapped, optional.

> **Economy caution:** income has a *compounding* term (plates/hr × rising skill) while mandatory sinks are *flat or lumpy*. The two dials governing the curve are **spoilage rate** (the leash) and **next-tier pricing** (the surplus soak). Wages are a weak dial. Watch for the throughput cliff.

### Dry aging — the experimentation engine
Opt-in, equipment-gated, limited capacity. A fish placed in an aging locker **leaves the spoilage track and enters the aging track.**

**It is a cash-out timer, not a ruin timer.** Past peak, the fish does **not** ruin — it sits there making no money while occupying a scarce slot. Decision: *pull it now and bank it, or tie up the slot hoping it climbs.* Pure opportunity cost.

A full random mutation rolls on aging, with **percentage multipliers** (not orders of magnitude). Mutations must stay rare. The aging decision is a second return hook alongside restock and drives the 24–48h target directly.

### Legendary creatures & the encounter system
- **The fight is the bite/reel loop, scaled up.** Multi-phase, harder, longer, with the creature actively fighting back (tension spikes, "dive" phases, stamina on both sides). Not a bespoke combat system — "fishing turned up to 11."
- **Fishing level gates outcome.** Underleveled = windows too tight, line snaps, fish lost. High-level = same fight is landable and pays enormously.
- **No buy-in, no loss penalty.** Losing costs nothing but the moment.
- **Weather is the spawn table.** Legendaries are **weather-triggered**, not summonable. The weather app **broadcasts the event to everyone** ("an ink storm is passing through Sector X"), raising every player's independent odds of that storm's legendary in the affected zone.
- **Baseline odds are near-zero-but-nonzero.** Outside the right weather, hooking a legendary is epsilon.
- **The storm is a shared spectacle with private encounters.** Many boats converge; each angler in their own independent struggle. The *event* is shared; the *encounter* is yours.
- **Trophy mounts** are a pure public flex, decay-free, and serve as goal-markers for catches you cannot yet butcher. Legendary butchering is Cooking-gated.

> **Tuning note:** storm window must be long enough that *get the alert → sail to the sector → fight* is achievable. Hook rarity and storm duration are paired dials.

### Gifting
Rare-fish gifting is in, as a friend-boost and virality engine. Gifted legendaries can be mounted or eventually butchered (Cooking-gated) but **not cheaply liquidated** — no whale-to-alt gold pipe.

### Staff
Staff is an **upgrade line — headcount, nothing more complex.** Browse NPC applicants on the resume/LinkedIn app and recruit them. **Each staff member's Hospitality/Cooking skill levels up the longer you keep them.** Wages scale with headcount.

### Marketing
"Marketing" is **not** a player action. It is a **hidden traffic stat** computed from **Yelp prestige + cosmetics + Hospitality**. Do not build a marketing minigame.

### Debt
Debt exists **only as the tutorial loan and the early overarching goal.** Not a permanent, recurring risk system.

---

## Two parked reconciliations to honor when building
1. **Hospitality per-table profit must improve margin, never remove variable cost.** A table is never free to serve.
2. **Gifted/unprocessable legendaries route to trophy mount or wait for the Cooking level — never to cheap liquidation.**

---

## Post-launch slots (decided, deferred)
- **Farming skill** (premium grocery line with mutation multipliers).
- **Popularity layer** (recoverable traffic number beneath the prestige rating).
- **Scored-attribute open mixing**, only if nigiri ever needs more depth.
- **Hybrid customer model** (guaranteed base stream plus shared hub-traffic skim for top restaurants).
- **Co-op shared-performance raids** — only if the user reopens the per-player-roll architecture.

---

## Build guidance for Claude Code

- **Authored-vs-computed discipline (Principle 4).** Species base prices live in a hand-authored lookup table. Everything else is a clamped multiplier. Suggested first-pass clamps: `freshness_polish ∈ [0.5, 1.5]`; `cooking_extraction ∈ [0, 1]`; `dry_age_mutation ∈ [1.0, ~2.5]`.
- **Catch resolution is client-authoritative-feeling but server-validated.** Per-player rolls must feel instant; validate server-side to prevent spoofed legendary hooks. Weather state is server-authoritative and broadcast.
- **Offline earnings are a computed bank, not a live sim.** On logout: snapshot stock + freshness timestamps + headcount + wage rate. On return: compute plates served = f(throughput cap, stock available as it spoils over elapsed time), minus wages, capped by freshness leash.
- **Customer stream is per-restaurant and independent** — a spawn process driven by the hidden traffic stat, with the six-stage lifecycle as a state machine per customer. No global customer pool.
- **Skill-gate needs no networking logic.** No kill-steal/tag/loot-priority systems.
- **Manual-then-automate is a literal code path.** The boat's cook/serve verbs are player-driven; the restaurant's are NPC-driven running the *same* underlying conversion. Build the conversion once; swap the driver at the boat→restaurant transition.
- **Weather is the legendary spawn system AND the FOMO push.** One system: server picks weather event + zone + duration, broadcasts to all clients, applies elevated legendary odds to affected rolls.

---

*End of GDD v2. Current session opens on [[Open Threads]] — Loose End #1, the cook verb, using the grill-me method.*
