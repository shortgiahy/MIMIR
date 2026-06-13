# Sushi Sea — Claude Code Context

Roblox game project. Platform: Luau / Roblox Studio. Mobile-compatible. 18+ demographic.

## What to read first (in order)

1. **[[GDD v2]]** — full game design handoff. Locked Decisions, Design Voice, Build Guidance, v1 overrides. Source of truth.
2. **[[Architecture]]** — service layout, RemoteEvent naming, DataStore schema, economy formula enforcement, offline bank math. Build to this.
3. **[[Standards]]** — Luau naming, module structure, type annotations, DataStore patterns, economy safety rules. All code follows this.
4. **[[Open Threads]]** — live unresolved items. Do not invent resolutions; surface them.

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

## Agent Roster

Use the right agent for the task type. Don't write code before consulting the appropriate agent.

| Task | Agent |
|------|-------|
| Writing Luau services, modules, or game systems | `roblox-systems-scripter` |
| UX, onboarding, retention, monetization design | `roblox-experience-designer` |
| Post-write code review (before any commit) | `engineering-code-reviewer` |
| Milestone sign-off ("is this actually done?") | `testing-reality-checker` |
| Architecture decisions, system design | `engineering-software-architect` |

**Codex review:** after any significant code change, pipe the diff through Codex as a second opinion before marking the task complete. See [[Codex Workflow]] if the file exists; otherwise ask the user to set it up.

---

## Session workflow

Design sessions use the **grill-me** method: one question at a time, walk the dependency tree, attach a recommended answer to every question before the user decides. When a decision locks, update GDD v2 and remove / check off the item in Open Threads.

Implementation sessions: read Architecture.md and Standards.md first, pick the correct agent, write code, run Codex review, then commit.
