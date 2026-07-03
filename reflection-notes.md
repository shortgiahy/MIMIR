# Reflection Notes — Setup Diagnosis

**Date:** 2026-07-03
**Method:** 3 extraction passes (session-note mining, git archaeology, vault drift audit) over all 10 session notes, 162 commits (2026-05-18 → 2026-06-18), and current vault state. Clustered, then judged per cluster: skill / automation / fix / nothing, weighing recurrence against build cost.
**Scope:** Diagnosis only. Nothing was built or edited. Ranked by leverage, highest first.

---

## 1. The system is pull-based and Giahy doesn't pull — every ritual is dead

**Verdict: AUTOMATION** (scheduled push, not another template)

The single biggest finding. Every recurring ritual in CLAUDE.md waits for Giahy to initiate, and none survived contact:

- **Daily notes:** 2 ever created (05-31, 06-01). Dead 32 days as of today.
- **Weekly check-ins:** 0 ever, despite a template and Brain.md explicitly scheduling "next Monday check-in: 2026-06-15." ~4 Mondays missed.
- **Monthly reviews:** 0 ever. June 1 and July 1 both passed silently.
- **Trading journals:** `Trading/Journals/` contains only `.gitkeep`. The July 1 restart target is 2 days past with no journal for July 1/2/3.
- **Escalation:** Oil change open since 2026-04-06 (88 days, HIGH). SAT registration open 46 days (HIGH, "slots fill fast"). No mechanism ever surfaced them between sessions.
- **The repo itself:** 77% of all commits happened on 4 burst days; 15 days of total dormancy since 06-18.

This was already diagnosed and never acted on: the 06-13 council ruled *"failure mode is maintenance overhead, not design"* and directed a flip to **push-not-pull for Calendar + Loose Ends flags**. That directive sits in the 06-13 session note, unimplemented. The pull-based promises remain in CLAUDE.md unchanged.

**Evidence:** Sessions 2026-05-31, 2026-06-01, 2026-06-13 (Vault Prune); drift audit of `Daily/`, `System/`, `Trading/Journals/`; git cadence analysis; `System/Loose Ends.md`.

**Why automation, not a skill:** A skill still requires Giahy to show up and invoke it — that's the exact failure mode. Cloud sessions support scheduled triggers (Routines/cron) that can open a session on their own: a morning trigger that drafts the daily note and flags urgent items, a Monday trigger for the weekly check-in, and an escalation pass over Loose Ends (anything HIGH > 14 days gets pushed, not waited on). Google Calendar MCP tools are now connected in the cloud environment, so the "pull Google Calendar" step of the Good Morning routine is finally executable rather than aspirational. Build cost: moderate (a few trigger prompts + CLAUDE.md protocol update). Recurrence it addresses: daily. Highest impact-per-cost in the vault, and it's the system's stated core purpose — "proactive reminders are a core responsibility, not a courtesy."

---

## 2. `/grill-me` doesn't exist — and it's gating the flagship project

**Verdict: FIX** (create the command file in-repo; ~30 minutes)

`/grill-me` is listed in CLAUDE.md, fully documented in `System/Skills/Grill Me.md`, and is the **most-referenced skill in the vault** — cited as the required next action in `System/Loose Ends.md` ("Next session opens here with grill-me"), in `Projects/Sushi Sea/CLAUDE.md` (3 references), and in both Implementation Status build-log entries ("Next starting point: grill-me on cook verb"). But there is no `.claude/commands/grill-me.md`, nothing in `~/.claude/skills/`, zero "grill" commits in history, and it is not loaded in cloud sessions (this session sees only council, prune, research). The doc calls it a "Personal skill (server-side)" — if it exists there, it's invisible to cloud sessions, which is where the work happens.

Consequence: the cook-verb decision (HIGH, open since 06-16) has blocked all Sushi Sea code for 17 days, and both session notes' "next starting point" routes through a skill that can't be invoked. Two consecutive sessions (06-16, 06-17) deferred to it.

**Same root cause killed `/swarm`:** built 2026-06-12 at `~/.claude/skills/swarm/SKILL.md` — the home directory of an **ephemeral cloud container**. It's gone. Only the doc (`System/Skills/Swarm.md`) survives, it was never added to CLAUDE.md's skill list, and no later session ever used it. The 06-12 Vault Infrastructure session also burned time on the commands-vs-personal-skills confusion and ended with a manual copy-paste step for Giahy.

**Evidence:** Sessions 2026-06-12 (Swarm Skill Build), 2026-06-12 (Vault Infrastructure), 2026-06-16, 2026-06-17; skills drift audit; this session's loaded-skill list.

**The fix, in two parts:** (a) create `.claude/commands/grill-me.md` from the existing doc — it unblocks Sushi Sea immediately; (b) add one rule to CLAUDE.md: *skills live in-repo only* (`.claude/commands/`), never in `~/.claude/` from a cloud session. For `/swarm`: no evidence of demand since it was built — delete the orphaned doc rather than rebuild. Do not build new skills here; this is repair.

---

## 3. Device auto-sync deletes session work and pollutes history

**Verdict: FIX** (config, not tooling)

On 2026-06-12 the desktop Obsidian vault-backup deleted `Sources/2026-06-12 Roblox Game Trends.md` **twice** mid-session — restored at 21:51 UTC (`7e93f56`), deleted again 2 minutes later (`dff86c8`), restored again (`1266232`, whose message names the offending backup hash). The desktop was committing a stale working tree over live session output.

Ongoing cost: 57 of 162 commits (35%) are "vault backup" / "Last Sync (Mobile)" ceremony; `.obsidian/workspace.json` is tracked and churned in 35 commits; mobile sync produces self-merge artifacts and duplicate-timestamp commit pairs.

**Evidence:** Git archaeology §1 (commits `564ae05`, `0dfe154`, `7e93f56`, `dff86c8`, `1266232`, `977b6d6`); recurrence: 2 destructive deletions in one afternoon + continuous noise since 05-18.

**The fix:** `.gitignore` the `.obsidian/` state files (at minimum `workspace.json`), and set Obsidian Git on desktop/mobile to pull-before-commit with a longer backup interval. One-time config change, permanently removes the only data-loss incident class in the vault's history. No skill, no automation needed.

---

## 4. Session-wrap protocol is followed in spirit, not in fact — state files drift immediately

**Verdict: SKILL** (`/wrap`) — this genuinely recurs every session

The Session End Protocol is 6 steps in CLAUDE.md, executed from memory each time. Git shows the result: of 9 session wraps, only 2 touched the full Session Note + Brain.md + Loose Ends trio. Loose Ends was skipped in 5 of 9. One wrap (06-17) produced no session note at all. Commit naming drifts ("Session end" / "Session close" / "session wrap"); one wrap is dated 5 days wrong.

The downstream cost is measurable: the 06-13 prune found **16 accumulated issues after only ~13 active days** — stale dates, contradictions, duplicates. The same fact (trading restart month) was wrong in two files and is *still* internally contradictory in Brain.md today (line 35 says July 1, lines 49/147 still say June). Brain.md's "Last updated" header is wrong, Tasks.md still lists dropped CS-1410 as active, Loose Ends' own "days open" counters are stale, and the Inbox has never been triaged despite a loose end open since 05-31.

**Evidence:** Git archaeology §5 (9 wrap commits itemized); session 2026-06-13 (16-issue lint pass, Inbox restore `4ec2400`); drift audit §4 (current contradictions).

**Why a skill clears the bar:** recurrence is every session (9/9 so far), build cost is low, and the checklist already exists in CLAUDE.md — a `/wrap` command just makes it mechanical instead of memory-dependent: update the trio, stamp headers, diff-check facts that live in multiple files, push. Related, folded in here: the /prune interval of ~60 days is mis-calibrated — 16 issues accumulated in 13 days. Monthly, or triggered by the Cluster-1 automation, fits the observed drift rate.

---

## 5. External-data promises (Calendar, Canvas, bills) have never once fired

**Verdict: FIX one, DROP one**

- **Google Calendar:** Good Morning step 2 says "Pull Google Calendar." No Calendar integration existed when written (`.mcp.json` has only playwright + an unkeyed firecrawl); no pull ever ran; the two existing daily notes have hand-typed flags. **Now fixable:** Google Calendar (and Gmail) MCP connectors are live in the cloud environment as of this session. Wiring the routine to real tools is a text edit, not a build.
- **Canvas:** "Pull Canvas deadlines" every Monday — never ran once. The mechanism (Playwright MCP → Firefox profile) is documented as broken in Loose Ends since 06-13, untouched. Its consumer (the weekly check-in) has also never run.
- **Bills:** `Recurring.md` has every due-date as `?` for 46 days; its own note says this blocks payment reminders.

**Evidence:** Drift audit §6; `System/Loose Ends.md` (Playwright entry, 06-13); `Recurring.md`.

**Call:** Fix Calendar (connector exists now — pair with Cluster 1's morning trigger). Drop the Canvas-scraping promise from CLAUDE.md until Giahy actually wants it fixed — a promise that's been broken for 3 weeks with no complaint is a weak signal of demand. Bills: the blocker is missing data only Giahy has, so it's a 5-minute ask, not tooling.

---

## 6. Immediate state cleanup (not a setup change — a task)

**Verdict: NOTHING to build — run /prune early**

Current contradictions worth clearing next working session: trading restart says June in two places and July in two others; Tasks.md lists dropped CS-1410 as active; SAT reg is double-tracked in Tasks and Loose Ends; "Finalize June income schedule" (HIGH) looks already-resolved by the closed Flex/plasma entries; Brain.md exceeds its own ≤150-line prune target (206 lines) and still carries the pre-rename "MIMIR_BRAIN" title; Inbox contains items claiming to be unbuilt (`swarm`) that were built and lost. The tool for this exists (/prune, scheduled 08-12). Pull it forward; don't build anything.

**Evidence:** Drift audit §§3-4; session 2026-06-13.

---

## No action — patterns observed and deliberately left alone

- **CLAUDE.md/Brain.md churn** (12 + 19 commits, split/merged/re-split 3×; git branching rule reversed 05-31 → 06-12): converged after 06-12 and has been stable since. Re-litigating it would be churn about churn.
- **Build-then-immediately-rework pattern** (council overhauled the day after creation; daily template redesigned in consecutive sessions; prune cleared Inbox on first run and was reverted in `4ec2400`): normal tool-tuning at this scale. Only note: first-run a new skill on a real task before wrapping the session that built it.
- **Branch hygiene:** clean. All 9 completed session branches merged and deleted per protocol. Not a problem, despite being the newest part of the workflow.
- **New skills generally:** nothing else recurs often enough to justify one. The daily-note failure is initiation, not tooling (Cluster 1); research/council/prune cover their niches and get used.

---

## Ranked summary

| # | Cluster | Verdict | Recurrence | Build cost |
|---|---------|---------|------------|------------|
| 1 | Pull-based rituals all dead; no push mechanism | Automation (scheduled triggers + Calendar MCP) | Daily, system-wide | Moderate |
| 2 | /grill-me nonexistent, gates Sushi Sea; /swarm lost to ephemeral home dir | Fix (in-repo command + one CLAUDE.md rule) | Every Sushi Sea session; 2 skills already lost | Trivial |
| 3 | Desktop/mobile sync clobbers work, 35% commit noise | Fix (gitignore + Obsidian Git settings) | 2 deletions, constant noise | Trivial |
| 4 | Session-wrap done from memory; 2/9 complete; drift accrues at ~1.2 issues/day | Skill (/wrap) + shorten prune interval | Every session (9/9) | Low |
| 5 | Calendar/Canvas/bills data never wired | Fix Calendar (connector now live); drop Canvas promise | Every promised morning/Monday | Low |
| 6 | Current contradictions in Brain/Tasks/Loose Ends | Nothing — run /prune early | — | — |
