# /wrap — Session End Protocol

Mechanize the Session End Protocol from `CLAUDE.md`. Run all steps in order — do not skip any because "nothing changed," confirm that explicitly instead.

---

## Steps

1. **Update `System/Brain.md`** — goals, patterns, active projects if anything changed this session. Stamp the "Last updated" header with today's date.
2. **Sync `System/Loose Ends.md`** — open anything new surfaced this session, close anything resolved (move to Closed table with today's date + one-line resolution).
3. **Cross-check facts that live in multiple files** — if this session touched a fact that also appears in `System/Tasks.md`, `System/Recurring.md`, or a project's own status file, verify they still agree. This is the specific failure mode that let "trading restart month" be wrong in two files at once. Fix any mismatch found; don't just note it.
4. **Append summary to today's `Daily/YYYY-MM-DD.md`** — only if that file exists. Don't create one from `/wrap`.
5. **If session touched Sushi Sea** — append a Build Log entry to `Projects/Sushi Sea/Implementation Status.md`, update status checkboxes.
6. **Create the session note** — `Session Notes/YYYY-MM-DD Topic.md` from `Session Notes/Session Template.md`. Lean: decisions made, vault changes, where we stopped, next starting point. Don't duplicate content that lives in dedicated files.
7. **Commit and push** — clear commit message, push to the current working branch. Never merge to `main` without explicit approval per the Git Workflow.

---

## Output

After all steps complete, report a one-line summary: which files changed, whether anything was flagged as unresolved, and the next starting point for the following session.

If a step finds nothing to do (e.g. no new loose ends), say so explicitly rather than silently skipping — that's what keeps this from drifting back into "done from memory, done partially."
