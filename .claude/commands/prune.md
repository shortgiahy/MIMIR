# /prune — Vault Lint & Maintenance

Run a full lint pass on the MIMIR vault. Identify decay, contradictions, stale data, and bloat. Propose a diff. Apply nothing without explicit approval.

---

## Phase 1 — Read

Read the following files in full before doing any analysis:

- `CLAUDE.md`
- `System/Brain.md`
- `System/Tasks.md`
- `System/Loose Ends.md`
- `System/Inbox.md`
- Any `Daily/YYYY-MM-DD.md` files from the last 14 days

Do not begin analysis until all files are loaded.

---

## Phase 2 — Lint

Scan across all loaded files. Flag every issue in one of five categories:

### 1. Stale Dates
Any entry that references a date in the past:
- Past due tasks still marked open
- Goals or deadlines that have already passed
- Weekly/monthly goal blocks that reference a prior week
- Events that have come and gone

### 2. Contradictions
Any case where two files say different things about the same fact:
- Tasks.md and Loose Ends.md both track the same item with conflicting status
- The same date, amount, or status differs between any two files

### 3. Orphaned Loose Ends
Any item in `Loose Ends.md` that:
- Has been open for 30+ days with no movement
- Is marked LOW priority and hasn't been touched since opening
- Was superseded by a decision logged elsewhere but never closed

### 4. Duplicate Tracking
Any item tracked in more than one file simultaneously with no clear ownership:
- Same task in both Tasks.md and Loose Ends.md
- Same project status in both Brain.md and a session note (with no clear "source of truth")
- Inbox items that duplicate something already in the vault — **flag only, never remove. Inbox is Giahy's quick-capture zone; he clears it himself.**

### 5. Bloat & Write Rules Violations
Any content that is resolved, outdated, or violates CLAUDE.md's Write Rules:
- Brain.md sections describing a state that no longer exists
- Facts duplicated across files (one home per fact — see CLAUDE.md Vault table)
- Narration, stamps, provenance notes, "see also" pointers, self-addressed notes
- Resolved content still sitting in a file (git is the archive — it should be deleted)

---

## Phase 3 — Report

Output a structured lint report using this format:

```
╔══════════════════════════════════════════════════════════════╗
║                     VAULT PRUNE REPORT                       ║
║                     <Today's Date>                           ║
╚══════════════════════════════════════════════════════════════╝

SUMMARY
  X issues found across Y files
  Categories: [list which categories have findings]

──────────────────────────────────────────────────────────────
  CATEGORY: <name>
──────────────────────────────────────────────────────────────

  [#]  FILE: <file>
       ISSUE: <clear description of the problem>
       PROPOSED ACTION: <specific change — what to delete, update, archive, or close>

[repeat for each issue]
```

Group issues by category. Number them sequentially across all categories (1, 2, 3...).

After the full report, output:

```
──────────────────────────────────────────────────────────────
APPLY OPTIONS
──────────────────────────────────────────────────────────────

  all         — apply every proposed change
  #,#,#       — apply only selected issue numbers
  none        — review only, apply nothing

What should I apply?
```

---

## Phase 4 — Apply

Wait for Giahy's response before touching any file.

When applying changes:
- **Delete** approved stale/resolved content outright — git history is the archive; no Archive/ folder
- **Close** a loose end by deleting its row
- **Update** means edit in-place with the corrected content
- After all changes are applied, output a one-line summary: `N changes applied across Y files.`
- Commit everything with message: `prune: <brief description of what was cleaned>`

---

## Rules

- Never apply changes before receiving approval.
- Inbox.md: flag only, never remove — Giahy clears it himself.
- If an issue is ambiguous, flag it but default proposed action to "ask" not "delete."
- Brain.md should not exceed ~100 lines after pruning. If it does, flag additional cut candidates.
- After pruning, log a loose end: `Next prune — <date 30 days from today>` if one doesn't already exist.
