# Sushi Sea — Full Dev Handoff for Fable 5

**Audience:** Fable 5, the model that will build Sushi Sea end-to-end.
**Author:** MIMIR, on Giahy's instruction.
**Status:** Handoff draft — pending Giahy's review before it becomes authoritative.

You are inheriting a fully-designed, zero-code Roblox project. The design is locked far enough to start building; the toolchain does not exist yet and you stand it up. This document is your single entry point. It does not replace the four design docs — it tells you how to read them, what order to build in, which tools to wire, and which protocols are non-negotiable.

Read this top to bottom once. Then keep the four design docs open and build to them.

---

## 0. The one-paragraph brief

Sushi Sea is a Roblox game: fish the open seas, process the catch, run a sushi restaurant. **The supply chain *is* the game** — there is no wholesale fish market, so every fish must flow through the kitchen to become gold. One shared persistent world; fishing outcomes are per-player client rolls, server-validated. F2P, cosmetics-and-convenience monetization, 18+ arcadey tone (*Dave the Diver* energy), mobile-compatible. The retention target is a 24–48h return rate, tuned primarily by fish perishability. Everything else is detail, and the detail is in the four docs below.

---

## 1. Source-of-truth map — read these, in this order

All four live in `Projects/Sushi Sea/`. **They govern; this handoff only routes you.**

1. **`GDD v2.md`** — full game design. Locked Decisions, the five v1 overrides, Design Voice, economy formula, build guidance. **Source of truth for *what the game is*.**
2. **`Architecture.md`** — Roblox service layout, RemoteEvent naming, DataStore schema, economy enforcement, offline bank math. **Build to this exactly. Any deviation needs Giahy's approval.**
3. **`Standards.md`** — Luau naming, module structure, type annotations, DataStore/pcall patterns, economy safety. **All code obeys this.**
4. **`Open Threads.md`** — live unresolved design items in resolution order. **Do not invent resolutions. Surface them and run grill-me.**

Plus `Implementation Status.md` — the living build checklist and Build Log. You update it every session (§9).

**Conflict rule:** GDD v2 supersedes everything older. Architecture supersedes your own instincts about layout. If two docs disagree, GDD v2 → Architecture → Standards, in that precedence. If you still can't reconcile, stop and ask Giahy.

---

## 2. Golden rules (violate none of these)

These are distilled from `Projects/Sushi Sea/CLAUDE.md` and the GDD. They are not suggestions.

- **Locked Decisions are settled.** Do not relitigate unless Giahy explicitly reopens one.
- **Open Threads are open.** Never fill one in unilaterally. Use grill-me, get the answer from Giahy, *then* build.
- **v2 supersedes v1 on all five overrides:** shared world, per-player rolls, aging-as-cash-out-timer, debt scoped to tutorial, Purchasing-not-Crafting. If something contradicts v2, trust v2.
- **No crafting exists.** All durable goods are *purchased* with gold. There is no assembly, no materials, no gathering.
- **Risk = opportunity cost, never punishment.** Nothing a player already owns can be destroyed by a mechanic. Failure forfeits a *potential gain*, never an existing asset. Every new risk surface must obey this.
- **The supply chain is the only path to gold.** No wholesale market. Ever.
- **Authored vs. computed discipline.** Large values are hand-authored lookups. Formula modifiers are clamped. Nothing large emerges from a chain of multiplications.
- **Manual before automatic.** Every system is performed manually by the player once before it can be automated. This is a literal code path, not a philosophy (§7.4).

---

## 3. Design Voice — the five principles that resolve most new questions

When a question isn't answered by the docs, resolve it against these before asking:

1. **Risk is always available, never forced, always trades for acceleration — and means opportunity cost, not punishment.** *You missed out* beats *you lost everything*.
2. **The supply chain is the only path to gold.** Both halves stay load-bearing by construction.
3. **Perform every system manually once before automating or expanding it.**
4. **Author the bands, clamp the multipliers.** Large numbers are hand-authored; modifiers are clamped.
5. **Experimentation lives in risk management (the dry-aging cash-out decision), not combinatorics (recipe mixing).**

If a principle resolves it, proceed and note the reasoning in your session log. If it doesn't, it's an Open Thread — surface it.

---

## 4. The economy formula (memorize; enforce server-side only)

```
served_plate_value = species_base × cooking_extraction × freshness_polish × dry_age_mutation
```

| Term | Source | Clamp |
|------|--------|-------|
| `species_base` | `FishTable.lua` authored lookup | none — it IS the large term |
| `cooking_extraction` | `cookingLevel / MAX_LEVEL` | `[0, 1]` |
| `freshness_polish` | linear from `caughtAt` timestamp | `[0.5, 1.5]` |
| `dry_age_mutation` | rare roll on locker pull | `[1.0, ~2.5]` — never orders of magnitude |

**All four multipliers apply server-side only.** The client receives the final resolved value for display, never the components. This is the anti-spoof invariant — do not break it for convenience.

---

## 5. Connected apps & toolchain

You will operate three external tools. None are wired yet; you set them up. Where a tool has no MCP connector in this environment, the instructions below tell you what to do by hand / hand back to Giahy.

### 5.1 Roblox Studio — the build target

Sushi Sea ships to Roblox. Studio is where the place is assembled, tested, and published. You do not have direct Studio automation from this environment, so the workflow is **file-first, synced into Studio**:

- **Author all game code as `.lua`/`.luau` files in the repo**, laid out to mirror `Architecture.md`'s service ownership tree. Never treat Studio as the source of truth — the repo is. Studio is a view.
- **Use [Rojo](https://rojo.space) to sync the repo into Studio.** Stand up a `default.project.json` that maps the repo's `src/` tree onto Roblox services (`ServerScriptService`, `ServerStorage`, `ReplicatedStorage`, `StarterPlayerScripts`, `StarterGui`) exactly as `Architecture.md` specifies. Rojo is the bridge; the filesystem stays canonical.
- **Toolchain manager:** use [Rokit](https://github.com/rojo-rbx/rokit) (or Aftman) with a pinned `rokit.toml` for `rojo`, `wally`, `selene` (lint), and `stylua` (format). Pin versions so builds are reproducible in ephemeral containers.
- **Package manager:** [Wally](https://wally.run) with `wally.toml` if third-party Luau packages are ever needed. Prefer zero dependencies until one is justified.
- **Testing:** author unit tests against pure modules (economy resolution, offline bank math, spoilage) so they run headless in CI without Studio. Studio playtests are for feel and integration only; logic is testable without it.
- **Publishing:** building the `.rbxl`/`.rbxlx` and publishing to a Roblox experience is a **Giahy action** — it requires his Roblox account and Creator Hub access. You produce the synced, buildable project; you hand Giahy the exact `rojo build` command and publish steps. Never assume you can publish.
- **Studio-only assets** (terrain, lighting, some UI positioning, physical model placement) that Rojo can't round-trip cleanly get documented in a `Projects/Sushi Sea/Studio Setup.md` runbook so Giahy (or a future session) can reproduce them by hand.

> If a Roblox/Rojo MCP connector becomes available in a later session, revisit this — direct sync would remove the manual publish handoff. As of this handoff, assume file-first + manual publish.

### 5.2 GitHub — version control & review

- **Repo:** `shortgiahy/MIMIR` (this vault). See §6 for where game code lives inside it.
- **Access:** use the `mcp__github__*` MCP tools for all GitHub operations — PRs, comments, CI, file reads. The `gh` CLI is **not** available in cloud sessions.
- **Branch discipline (hard rule):** never commit to `main` directly. Work on a feature branch: `git checkout -b claude/<description>`. Current designated branch for this handoff work is `claude/sushi-game-dev-doc-ufh154`. For game-build work, open fresh `claude/sushi-<feature>` branches.
- **Commit cadence:** commit with clear, descriptive messages as you go — one logical change per commit. Not one giant end-of-session dump.
- **Push, never auto-merge.** Push the branch. Merging into `main` requires Giahy's explicit approval — present a summary and ask. Only after "yes": `git checkout main && git merge --no-ff <branch> && git push && delete the branch`.
- **PRs:** do **not** open a PR unless Giahy asks. If he does, check for a PR template first and mirror it.
- **Push retry:** on network failure, retry up to 4× with exponential backoff (2s → 4s → 8s → 16s). Always `git push -u origin <branch>`.
- **CI:** stand up GitHub Actions that run `selene` (lint), `stylua --check` (format), and the headless unit tests on every push. Green CI is part of Definition of Done (§10).
- **Ephemeral containers:** web sessions run in throwaway containers — the repo is cloned fresh and wiped on inactivity. **Anything not pushed is gone.** Push before wrapping up, every time.

### 5.3 Blender — 3D asset pipeline

Fish, boats, the restaurant, props, and trophy mounts are 3D assets. Blender is the authoring tool; Roblox is the target.

- **No Blender MCP connector exists in this environment.** You cannot model interactively. Your role is to **specify and script**, not to sculpt by hand.
- **What you produce:** an asset manifest in `Projects/Sushi Sea/Asset Pipeline.md` — every model the game needs, its poly budget, pivot/origin convention, scale (Roblox studs), texture/material spec, and naming. Plus, where useful, **Blender Python (`bpy`) scripts** that generate or batch-process assets programmatically (procedural fish scaling, LOD decimation, batch FBX/OBJ export).
- **Export contract:** Blender → `.fbx` (or `.obj` for static props) → imported to Roblox via Studio's 3D importer or the Asset Manager. Meshes must be Roblox-legal: ≤10k tris per `MeshPart`, ≤4 texture maps, correct pivot at model origin, real-world stud scale. Document these limits in the manifest so nothing round-trips broken.
- **Handoff to Giahy:** actual modeling in Blender and importing meshes to Roblox are **Giahy actions** (or a human artist's). You hand him the manifest, the `bpy` scripts, and per-asset acceptance criteria. Placeholder primitives (Roblox `Part` blockouts) are fine for prototyping the vertical slice — ship gray-box first, art later. Do not block gameplay on final art.

> **Sequencing:** the vertical slice uses gray-box primitives only. Blender assets are a later phase (§8, Phase 5). Do not model fish before the fishing loop feels good.

---

## 6. Repository & project layout — where the game code lives

**Decision flagged for Giahy (see Handoff Questions, §12):** This vault is an Obsidian design vault. The game's Luau code needs a home. Two options:

- **(A) Recommended — `game/` subfolder in this repo.** Put the Rojo project and all Luau under `Projects/Sushi Sea/game/`. Keeps design docs and code co-located, one repo, one branch workflow, matches the existing single-repo cloud setup. Obsidian ignores it; Rojo roots there.
- **(B) Dedicated Roblox repo.** Cleaner for Roblox tooling and CI, standard for game projects, but splits design from code across two repos and two branch workflows. More correct long-term, more overhead now.

I recommend **(A)** until the codebase outgrows it, then extract to **(B)**. **This is Giahy's call — do not scaffold code until he picks.** Proposed layout under option (A):

```
Projects/Sushi Sea/game/
  default.project.json          # Rojo: maps src/ onto Roblox services per Architecture.md
  rokit.toml                    # pinned rojo, wally, selene, stylua
  wally.toml                    # deps (empty to start)
  selene.toml  stylua.toml      # lint + format config
  src/
    server/                     # → ServerScriptService/ServerStorage
    shared/                     # → ReplicatedStorage
    client/                     # → StarterPlayerScripts
    gui/                        # → StarterGui
  tests/                        # headless unit tests (economy, offline bank, spoilage)
  .github/workflows/ci.yml      # selene + stylua --check + tests
```

Map `src/` subfolders onto the exact service tree in `Architecture.md` §"Roblox Service Ownership". Do not improvise a different tree.

---

## 7. Architecture you build to (digest — full spec in `Architecture.md`)

Read `Architecture.md` in full before writing a line. This is the digest so you know the shape.

### 7.1 Services (ServerScriptService)
`WeatherService` · `PlayerDataService` · `EconomyService` · `StaffService` · `CustomerService` · `SpoilageService`, wired by a logic-free `Init.server.lua`.

### 7.2 RemoteEvent naming
- Server→Client: `{System}_{Event}` (e.g. `Weather_StormBroadcast`, `Economy_PlateResolved`).
- Client→Server: `Player_{Action}` (e.g. `Player_CastLine`, `Player_ServePlate`).
- **No client RemoteFunction returns an economy-affecting value.** Economy resolves server-side only.

### 7.3 DataStore
Key `PlayerData_v1`. Full schema in `Architecture.md` — skills, inventory (spoilage track), agingLocker (separate track), restaurant, economy, meta. **Schema-versioned; migrate, never overwrite.** All DataStore calls wrapped in the pcall + retry pattern from `Standards.md`.

### 7.4 The manual-then-automate code path (build this correctly the first time)
There is **one** `ConversionModule` that turns a fish into a plate. On the boat, the *player* drives it (manual verb). At the restaurant, *staff AI* drives the same module. **Build the conversion once; swap the driver.** Do not duplicate the logic across boat and restaurant.

### 7.5 Offline bank
Closed-form computation on login, never a tick-by-tick replay. Snapshot on logout; on return compute `platesServed`, subtract wages, cap by the freshness leash, `netBank = max(0, gross - wages)`. Full math in `Architecture.md`.

### 7.6 What is NOT in this game
No wholesale market. No crafting. No shared legendary encounters. No total-loss spoilage state. No recurring debt system. If a task pushes toward any of these, **stop and surface it.**

---

## 8. Build roadmap — sequenced phases, zero to complete

This is the order of operations. Each phase has an exit gate; do not start the next phase until the current gate passes a reality check (§10). **Phase 0 is blocked on an Open Thread — that block is real.**

### Phase 0 — Unblock the cook verb *(design, not code)*
The single highest-priority item. **Open Thread #1: what the player physically does when cooking on the boat is undefined, and it blocks the entire vertical slice.** Also blocks the serve verb and the Omakase counter (#6).
- **Action:** run grill-me with Giahy on the cook verb and the boat serve verb. Lock them. Update GDD v2, check off Open Thread #1, flip the blocked rows in Implementation Status.
- **Exit gate:** cook + serve verbs are locked in GDD v2. Nothing playable is built before this.

### Phase 1 — Toolchain + skeleton *(no gameplay)*
- Giahy picks repo layout (§6, §12). Then: Rojo project, Rokit toolchain, selene/stylua config, CI, empty service files matching the Architecture tree, `PlayerDataService` skeleton with the versioned schema + pcall/retry.
- **Exit gate:** `rojo build` produces a place; it opens in Studio; CI is green; a player joins and data loads/saves without error.

### Phase 2 — Fishing feel slice *(gray-box, no backend)*
Per the 2026-06-17 council ruling, **game feel is the binding constraint on retention** — the fishing rod must pass a blind-playtester test before any economy/backend work. Build cast → hook → reel with gray-box art and placeholder numbers. Tune until it *feels* good.
- `FishingController` (cast/reel input), server-side catch validation, the locked cook + serve verbs, basic spoilage tick, `EconomyService` plate resolution wired to `FishTable` (~10 authored species).
- **Exit gate:** a blind playtester finds the core loop (cast → catch → cook → serve → gold) satisfying with gray-box art. This is a *feel* gate, not a *feature* gate.

### Phase 3 — Economy first-pass + tuning
Resolve Open Thread #3 (the 5-row faucets-minus-sinks table) and #5 (spoilage/offline-coast values) jointly — they share the same dial as the 24–48h target. `SpoilageService` real rates, `OfflineBankCalculator`, storage tier ladder.
- **Exit gate:** the 5-row economy table shows net income/hr growing slower than next-tier cost in a healthy curve; the throughput cliff lands ~week 6, not day 1.

### Phase 4 — Restaurant tier + staff automation
The boat→restaurant transition. `CustomerService` (6-stage lifecycle state machine, per-restaurant independent stream), `StaffService` driving the *same* `ConversionModule` (§7.4), Yelp prestige + hidden traffic stat (Open Thread #6), Restaurant UI. Manual boat verbs now have their automated counterparts.
- **Exit gate:** a player can earn a restaurant, hire staff, and the restaurant runs the standard menu autonomously while the player fishes; offline bank pays out correctly on return.

### Phase 5 — Weather, legendaries, dry-aging, art pass
`WeatherService` (weather events + legendary spawn broadcast, the shared-spectacle/private-encounter model), legendary multi-phase reel (Open Thread #4, needs base reel numbers from Phase 2), `DryAgingLocker` (cash-out timer, mutation roll), trophy mounts. Blender asset pipeline replaces gray-box (§5.3). Omakase counter (#6).
- **Exit gate:** a storm broadcasts, players converge, each fights an independent legendary; dry-aging presents a real pull-now-or-wait decision; final art is in.

### Phase 6 — Monetization, polish, launch prep
`PassManager` (GamePass ownership cache + prompts — cosmetics/convenience only, nobody taxed for winning), cosmetics, walkout rules, remaining §6 smaller-undefineds, full playtest, `Studio Setup.md` runbook, publish handoff to Giahy.
- **Exit gate:** reality-check sign-off on a complete, publishable experience.

> Phases 3–5 have real cross-dependencies (economy ↔ spoilage ↔ offline). Resolve their Open Threads in dedicated numbers sessions with Giahy, not on the fly.

---

## 9. Session workflow & mandatory logging

### Design session
Use grill-me: one question at a time, walk the dependency tree, attach a *recommended* answer before Giahy decides. When a decision locks: update GDD v2 → check off the item in Open Threads → update the blocked rows in Implementation Status.

### Implementation session
Read `Architecture.md` + `Standards.md` first → pick the correct agent (§11) → write code → run the code-reviewer agent → commit. After any significant change, get a second-opinion review before marking a task complete.

### Session end — the Build Log entry is mandatory
At the end of **every** implementation session, append to the Build Log in `Implementation Status.md`, most-recent-on-top:

```
### YYYY-MM-DD — <one-line summary>
- Built: <what was written or wired>
- Agent used: <which agent>
- Status changes: <rows that flipped ⬜→🔨 or 🔨→✅>
- Blockers hit: <anything that stopped progress>
- Next starting point: <exact task to pick up>
```

Then update the status checkboxes to match reality. **Do not mark ✅ unless the system is wired end-to-end and manually verified — not "file exists."** Push all changes before the container is reclaimed.

---

## 10. Definition of Done & reality-check gates

A system is ✅ only when **all** hold:
- Wired end-to-end (not just authored — actually called by the running game).
- Manually verified in a Studio playtest (or headless test for pure logic).
- Passes the code-reviewer agent.
- Obeys `Standards.md` (types, pcall/retry, server-side economy, clamps, no globals).
- CI green (selene + stylua + tests).

At each phase exit gate, run the **testing-reality-checker** agent with the question "is this actually done?" It defaults to "needs work" and demands evidence — that is intentional. A green checklist is not a shipped feature.

---

## 11. Agent roster — use the right one, don't code before consulting it

| Task | Agent |
|------|-------|
| Writing Luau services, modules, game systems | `roblox-systems-scripter` |
| UX, onboarding, retention, monetization design | `roblox-experience-designer` |
| Post-write code review (before any commit) | `engineering-code-reviewer` |
| Milestone sign-off ("is this actually done?") | `testing-reality-checker` |
| Architecture decisions, system design | `engineering-software-architect` |

MIMIR-level project skills also available: **`/grill-me`** (clarify before any major plan — don't skip to be fast), **`/council`** (5-perspective debate on a major decision), **`/research`** (deep research with adversarial verification), **`/prune`** (vault maintenance).

---

## 12. Handoff questions for Giahy (answer before Phase 1)

These are genuine decisions I won't make for you:

1. **Repo layout (§6):** subfolder `game/` in this vault (A, recommended) or a dedicated Roblox repo (B)?
2. **Cook verb (Open Thread #1):** this is the true blocker. Ready to run grill-me on it, or do you want to seed a direction first?
3. **Art timing:** confirm gray-box-first through Phase 4, Blender pipeline at Phase 5 — or do you want an artist involved earlier?
4. **Publishing:** confirm you handle the Roblox Creator Hub publish step and I hand you a buildable project + runbook, rather than expecting automated publishing.

---

## 13. What to do right now (Fable 5's first move)

1. Read the four source-of-truth docs in full (§1).
2. Get Giahy's answers to §12.
3. **Open on Phase 0:** run grill-me on the cook verb (Open Thread #1). Do not scaffold code until it's locked and Giahy has picked the repo layout.
4. Log the session and push before the container dies.

Build to the docs. Surface the open threads. Don't relitigate what's locked. Move fast on everything that can't be undone in ten seconds only after asking.

*— MIMIR*
