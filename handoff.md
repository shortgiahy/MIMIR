# Handoff — Setup Fixes

**Source:** `reflection-notes.md` (2026-07-03 diagnosis). Full evidence there; this is the work order.
**How to use:** One numbered block per session is fine. Order = leverage. Each block is independently shippable. Hand this file to a fresh session and say "do #N."

---

## 1. Create `/grill-me` in-repo — unblocks Sushi Sea (~20 min) ⚡ do first, it's the cheapest

1. Write `.claude/commands/grill-me.md` from the spec in `System/Skills/Grill Me.md`.
2. Add to CLAUDE.md skill-location rule: skills live in `.claude/commands/` only — never `~/.claude/` from a cloud session (that's how `/swarm` was lost).
3. Delete `System/Skills/Swarm.md` and remove the stale `swarm` items from `System/Inbox.md` (built 06-12 into an ephemeral container, gone, never used — don't rebuild).
4. **Done when:** `/grill-me` invokable in a fresh session; then run it on the cook verb (Loose Ends, HIGH, blocking all game code since 06-16).

## 2. Stop sync clobbering (~15 min, desktop + phone in hand)

1. Add to `.gitignore`: `.obsidian/workspace.json` (minimum) or `.obsidian/` state files generally; `git rm --cached` them.
2. Obsidian Git settings on desktop AND mobile: pull-before-commit ON, lengthen auto-backup interval, don't auto-backup while a Claude session is open.
3. **Done when:** a test session note pushed from cloud survives the next desktop backup. (Reference incident: `Sources/2026-06-12 Roblox Game Trends.md`, deleted twice by backups `0dfe154`/`dff86c8`.)

## 3. Push-based routine — the big one (~1 session)

1. Update CLAUDE.md: Good Morning step 2 now uses the live Google Calendar MCP connector (it's connected in cloud sessions as of 07-03 — no build needed).
2. Create scheduled triggers (Routines) in a cloud session:
   - **Daily ~7am MT:** draft `Daily/YYYY-MM-DD.md` from template, pull Calendar next-72h, flag Loose Ends items HIGH >14 days, prompt for trade journal if trading day.
   - **Monday ~7am MT:** weekly check-in from `System/Weekly Check-in Template.md`.
   - **1st of month:** monthly review prompt.
3. Drop the Canvas-pull promise from CLAUDE.md + weekly template (broken since 06-13, zero demand). Close or park the Playwright loose end.
4. **Done when:** one morning trigger has actually fired and produced a daily note without Giahy initiating.

## 4. `/prune` now, not 08-12 (~30 min)

Run `/prune`. Known targets: trading restart says June in Brain.md lines 49/147 vs July elsewhere (and July 1 has now passed — get the real status from Giahy); Tasks.md still lists dropped CS-1410; SAT reg double-tracked; "Finalize June income schedule" likely already resolved; Brain.md 206 lines vs ≤150 target, stale "MIMIR_BRAIN" title and "Last updated" header; Inbox never triaged. Also: change prune cadence from 60 → 30 days in `.claude/commands/prune.md` + CLAUDE.md.

## 5. Build `/wrap` skill (~30 min)

`.claude/commands/wrap.md`: mechanize the Session End Protocol — update Brain.md (+ stamp header date), sync Loose Ends, session note from template, Sushi Sea build log if touched, cross-file fact check (dates that live in >1 file), push. Document in `System/Skills/`, add to CLAUDE.md list. Rationale: only 2 of 9 wraps were complete when done from memory.

## 6. Needs Giahy directly (5 min of your time, no session)

- Bill due-dates for `Recurring.md` — all still `?`, blocking payment reminders since 05-18.
- SAT registration ($68, Oct 3, 46 days open) and oil change (88 days open) — the triggers in #3 will start nagging about these; faster to just do them.

---

*After each block: commit, push, check it off here. Delete this file when all six are done.*
