# Sushi Sea — Claude Code Context

Roblox game project. Platform: Luau / Roblox Studio. Mobile-compatible. 18+ demographic.

## What to read first

1. **[[GDD v2]]** — full game design handoff. Contains all Locked Decisions, Design Voice principles, Build Guidance, and Overrides from v1. This is the source of truth.
2. **[[Open Threads]]** — live unresolved items, in resolution order. Do not invent resolutions; surface them.

## Golden rules for this project

- **Locked Decisions in GDD v2 are settled.** Do not relitigate unless the user explicitly reopens them.
- **Open Threads are open.** Never fill them in unilaterally — use grill-me and get answers from the user.
- **v2 supersedes v1 on all five overrides** (shared world, per-player rolls, aging as cash-out timer, debt scoped to tutorial, Purchasing not Crafting). If something contradicts v2, trust v2.
- **No crafting exists.** All durable goods are purchased with gold.
- **Risk = opportunity cost, never punishment.** Nothing a player already owns can be destroyed by a mechanic. Failure forfeits a potential gain, never an existing asset.

## Design Voice (quick ref — full version in GDD v2)

1. Risk = opportunity cost, never punishment.
2. Supply chain is the only path to gold (no wholesale market).
3. Perform every system manually once before automating.
4. Author the bands, clamp the multipliers.
5. Experimentation = risk management (aging), not combinatorics (recipes).

## Economy formula

`served_plate_value = species_base × cooking_extraction × freshness_polish × dry_age_mutation`

First-pass clamps: `freshness_polish ∈ [0.5, 1.5]`, `cooking_extraction ∈ [0, 1]`, `dry_age_mutation ∈ [1.0, ~2.5]`.

## Highest-priority open item

**The cook verb (#1 in Open Threads)** — what the player physically does when cooking on the boat. Blocks the entire vertical slice. Open every new design session here unless the user redirects.

## Session workflow

Design sessions use the **grill-me** method: one question at a time, walk the dependency tree, attach a recommended answer to every question before the user decides. When a decision locks, update GDD v2 and remove / check off the item in Open Threads.
