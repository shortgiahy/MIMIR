# /check-in — End of Day Check-In

Capture the day's three measurements, then scan the last 14 days for a productive task that has quietly dropped off. Instrument only — no XP, no scoring, no praise.

---

## Phase 1 — Load

- `Daily/YYYY-MM-DD.md` for today (create from `Daily/Daily Template.md` if missing)
- Every `Daily/YYYY-MM-DD.md` in the last 14 days
- `System/Brain.md` — Active Domains, Projects, Patterns
- `System/Tasks.md` — anything dated inside the window

Tag vocabulary is the picklist in `Daily/Daily Template.md`. Do not invent tags; if Giahy names something outside the list, ask which tag it belongs under or propose adding one to the template.

---

## Phase 2 — Capture

Ask one question at a time. No menus, no preamble.

1. **Screen time in minutes.** Take the raw number. Don't comment on it.
2. **Productive tasks finished today.** Free text; assign a tag to each. Zero is a valid answer — record it as zero, don't coach.
3. **Complacency check.** `Nothing dropped` / `Dropped something` / `Not sure`.
4. If `Dropped something` — **what dropped.** One line.

Write all four into today's note under **End of Day Check-In**. Update in place if the section is already filled.

---

## Phase 3 — 14-Day Drift Scan

Split the window: **recent** = days 1–7, **prior** = days 8–14. For every tag, count the number of days it appears in the finished-tasks list.

Flag rules:

| Flag | Condition |
|------|-----------|
| **DROPPED** | ≥3 days in prior, **0** days in recent |
| **FADING** | ≥3 days in prior, recent ≤ half of prior |
| **NEVER STARTED** | Active domain in Brain.md, 0 days across all 14 |

Rules:
- Fewer than 7 notes in the window → report the day count, run no flags. Say the instrument isn't warm yet.
- 7–13 notes → run DROPPED and FADING against whatever days exist; state the sample size.
- A tag Giahy explicitly paused (Trading, and anything Brain.md marks paused) is never flagged.
- Flag the tag, not the person. `[SAT] — 4 days prior week, 0 this week.` No adjectives.

---

## Phase 4 — Screen Time

Report the **7-day rolling average** only. Never surface a day-over-day delta — one bad day is noise, and comparing yesterday to today invites a shame loop with no signal in it.

The prior week's average may be cited **only** as supporting evidence when a DROPPED or FADING flag lands in the same window. Standalone, it stays hidden.

---

## Phase 5 — Verdict

Compare his complacency answer against the scan. This is the whole point of the instrument.

- Said `Nothing dropped`, scan found a drop → **lead with the contradiction.** State the tag and the counts, then ask whether it was a decision or a slip.
- Said `Dropped something`, scan agrees → confirm, no lecture.
- Said `Dropped something`, scan is clean → his read beats the data. Ask what he saw.
- Said `Not sure`, scan is clean → say so plainly. Clean is clean.
- Scan is clean and he said `Nothing dropped` → one line. Done. Do not manufacture a concern.

A slip that turns out to be a deliberate deprioritization is not a failure — record the decision in `System/Brain.md` and stop flagging that tag.

Any flag Giahy defers on → row in `System/Loose Ends.md` immediately, without asking.

---

## Phase 6 — Close

- Commit the daily note: `check-in: YYYY-MM-DD`
- Output ≤5 lines total. Screen time average, task count, flags (or `no flags`), and the one question if there is one.
