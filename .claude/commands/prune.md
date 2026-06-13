# /prune — Vault Lint & Maintenance

Run a full lint pass on the MIMIR vault. Identify decay, contradictions, stale data, and bloat. Propose a diff. Apply nothing without explicit approval.

---

## Phase 1 — Read

Read the following files in full before doing any analysis:

- `System/Brain.md`
- `System/Tasks.md`
- `System/Loose Ends.md`
- `System/Inbox.md`
- Last 10 files in `Session Notes/` (by date, most recent first)
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
- Brain.md says X is open; a session note says X was resolved
- Tasks.md and Loose Ends.md both track the same item with conflicting status
- A pattern documented in Brain.md that was updated in a session note but not reflected back

### 3. Orphaned Loose Ends
Any item in `Loose Ends.md` that:
- Has been open for 30+ days with no session note referencing it
- Is marked LOW priority and hasn't been touched since opening
- Was superseded by a decision logged elsewhere but never closed

### 4. Duplicate Tracking
Any item tracked in more than one file simultaneously with no clear ownership:
- Same task in both Tasks.md and Loose Ends.md
- Same project status in both Brain.md and a session note (with no clear "source of truth")
- Inbox items that duplicate something already in the vault — **flag only, never remove. Inbox is Giahy's quick-capture zone; he clears it himself.**

### 5. Bloat & Archive Candidates
Any content that is resolved, outdated, or no longer actionable:
- Brain.md sections describing a state that no longer exists
- Session notes older than 60 days with no referenced open items
- Protocol documentation duplicated between Brain.md and CLAUDE.md
- Brain.md "This Week" goals block if it's more than 2 weeks old

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
- **Delete** only confirmed stale/resolved content — never silently remove anything that might still matter
- **Archive** means move content to `System/Archive/YYYY-MM-DD <topic>.md`, not delete
- **Close** a loose end by moving it from Open to Closed in `Loose Ends.md` with today's date and a one-line resolution note
- **Update** means edit in-place with the corrected content
- After all changes are applied, output a one-line summary: `N changes applied across Y files.`
- Commit everything with message: `prune: <brief description of what was cleaned>`

---

## Rules

- Never apply changes before receiving approval.
- Never delete — archive or close instead.
- If an issue is ambiguous, flag it but default proposed action to "ask" not "delete."
- Brain.md should not exceed ~150 lines after pruning. If it does, flag additional archive candidates.
- After pruning, log a loose end: `Next prune — <date 60 days from today>` if one doesn't already exist.
