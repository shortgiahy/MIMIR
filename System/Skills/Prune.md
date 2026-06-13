# Skill — /prune

**Trigger:** `/prune`
**Location:** `.claude/commands/prune.md`

---

## What It Does

Full lint pass on the MIMIR vault. Surfaces stale dates, contradictions between files, orphaned loose ends, duplicate tracking, and bloat. Outputs a numbered report of proposed changes. Applies nothing without explicit approval.

Inspired by Karpathy's LLM Wiki Lint operation — the formal maintenance layer that prevents a living knowledge base from degrading into noise over time.

---

## How It Works

**Phase 1 — Read**
Loads Brain.md, Tasks.md, Loose Ends.md, Inbox.md, last 10 Session Notes, and Daily notes from the past 14 days.

**Phase 2 — Lint**
Scans across all files for five issue categories:

| Category | What It Catches |
|----------|----------------|
| Stale Dates | Past-due tasks, expired deadlines, outdated goal blocks |
| Contradictions | Files that disagree about the same fact or status |
| Orphaned Loose Ends | Items open 30+ days with no movement or session reference |
| Duplicate Tracking | Same item tracked in multiple files with no clear owner |
| Bloat | Resolved/outdated content ready to archive; Brain.md over 150 lines |

**Phase 3 — Report**
Numbered list of issues with proposed actions. Ends with an apply prompt: `all`, specific issue numbers, or `none`.

**Phase 4 — Apply**
Waits for approval. Never deletes — archives to `System/Archive/` or closes in-place. Commits all changes with a `prune:` prefix message.

---

## Rules

- Nothing changes without Giahy's sign-off.
- Ambiguous issues default to "ask," not "delete."
- Brain.md target: ≤150 lines post-prune.
- After each run, a "Next prune" loose end is logged 60 days out.

---

## When to Use

- Brain.md is feeling heavy or hard to navigate
- Loose Ends has items from months ago with no movement
- Before a major planning session (weekly check-in, monthly review)
- Any time the vault feels like it's accumulating rather than compounding

Run it roughly every 60 days, or whenever entropy is noticeable.

---

## Example

```
/prune
```

No arguments needed. MIMIR reads the vault and reports back.
