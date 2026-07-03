# Skill — /wrap

**Trigger:** `/wrap` (or Giahy says "wrap up")
**Location:** `.claude/commands/wrap.md`

---

## What It Does

Mechanizes the Session End Protocol. Runs the full 6-step close-out as a checklist instead of from memory: update Brain.md, sync Loose Ends, cross-check facts duplicated across files, append the daily note if one exists, log a Sushi Sea build entry if relevant, write the session note, commit, and push.

---

## Why It Exists

Before this skill, the protocol lived only as prose in CLAUDE.md and was run from memory each session. Of 9 session wraps, only 2 completed the full trio (Brain + Loose Ends + Session Note) — Loose Ends was skipped 5 times, one wrap produced no session note at all, and the same fact (trading restart month) went stale in two files simultaneously because nothing cross-checked it. That drift is what a `/prune` pass later has to clean up.

---

## How It Works

Reads `CLAUDE.md`'s Session End Protocol, walks each step explicitly, and reports what changed — including "nothing to do here" when a step genuinely has no work, so silence can't be mistaken for a skipped step.

---

## When to Use

Every session close. Also whenever Giahy says "wrap up."

---

## Example

```
/wrap
```

No arguments needed.
