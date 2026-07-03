# Handoff — Setup Fixes

**Source:** `reflection-notes.md` (2026-07-03 diagnosis). Full evidence there; this is the work order.
**How to use:** One numbered block per session is fine. Order = leverage. Each block is independently shippable. Hand this file to a fresh session and say "do #N."

**Status as of 2026-07-03 (session: Vault Efficiency Cleanup, merged to main):** #1, #2, #4, #5 done. #3 partially done (Calendar wiring + Canvas drop landed; the scheduled trigger itself was denied on creation, reason not given). #6 was force-closed at Giahy's request rather than actually resolved — see `System/Loose Ends.md` note. Not deleting this file yet since #3's trigger is still outstanding.

---

## 1. ✅ DONE — Create `/grill-me` in-repo — unblocks Sushi Sea (~20 min) ⚡ do first, it's the cheapest

1. Write `.claude/commands/grill-me.md` from the spec in `System/Skills/Grill Me.md`.
2. Add to CLAUDE.md skill-location rule: skills live in `.claude/commands/` only — never `~/.claude/` from a cloud session (that's how `/swarm` was lost).
3. Delete `System/Skills/Swarm.md` and remove the stale `swarm` items from `System/Inbox.md` (built 06-12 into an ephemeral container, gone, never used — don't rebuild).
4. **Done when:** `/grill-me` invokable in a fresh session; then run it on the cook verb (Loose Ends, HIGH, blocking all game code since 06-16).

## 2. ✅ DONE — Stop sync clobbering (~15 min, desktop + phone in hand)

1. Add to `.gitignore`: `.obsidian/workspace.json` (minimum) or `.obsidian/` state files generally; `git rm --cached` them.
2. Obsidian Git settings on desktop AND mobile: pull-before-commit ON, lengthen auto-backup interval, don't auto-backup while a Claude session is open.
3. **Done when:** a test session note pushed from cloud survives the next desktop backup. (Reference incident: `Sources/2026-06-12 Roblox Game Trends.md`, deleted twice by backups `0dfe154`/`dff86c8`.)

## 3. ⚠ PARTIAL — Push-based routine — the big one (~1 session)

1. ✅ Done — CLAUDE.md Good Morning step 2 now points at the live Google Calendar MCP connector.
2. ❌ Not done — trigger creation was **denied** when attempted 2026-07-03, reason not given before session ended. Still needs: a daily notification-only trigger (Giahy's stated preference: push notification via the PushNotification tool, not a full autonomous daily-note draft — Telegram/WhatsApp requested but no connector installed). Next session: ask why it was denied before retrying blind.
3. ✅ Done — Canvas-pull promise dropped from CLAUDE.md + Brain.md weekly cadence. Playwright loose end deprioritized to LOW (then force-closed with everything else, see #6 note below).
4. **Done when:** one morning trigger has actually fired and sent a notification without Giahy initiating.

## 4. ✅ DONE — `/prune` now, not 08-12 (~30 min)

Fixed all listed targets except the trading-restart-month contradiction (Brain.md lines ~35/49/147 still disagree — needs Giahy's actual answer, still not given as of this session's end). Prune cadence shortened to ~30 days. Inbox intentionally left untouched (`/prune` rule: flag only, Giahy clears it himself).

## 5. ✅ DONE — Build `/wrap` skill (~30 min)

`.claude/commands/wrap.md` + `System/Skills/Wrap.md` exist, added to CLAUDE.md's skill list.

## 6. ⚠ NOT RESOLVED — force-closed instead

- Bill due-dates for `Recurring.md` — still all `?`. Not touched.
- SAT registration and oil change — Giahy asked to force-close **all** open Loose Ends this session (confirmed twice, once with explicit warning that these weren't actually done). They're marked closed in `System/Loose Ends.md` for bookkeeping but are **not actually resolved**. Flagged clearly in that file's closing note.

---

**Still open for next session:** trading-restart-month contradiction (needs Giahy's answer), the daily automation trigger (needs to know why it was denied, then rebuild). Everything else in this handoff is done — delete this file once those two are closed out.
