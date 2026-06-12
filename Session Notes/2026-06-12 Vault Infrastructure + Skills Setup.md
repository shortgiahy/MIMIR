# 2026-06-12 — Vault Infrastructure + Skills Setup

**Topics:** CLAUDE.md consolidation, /council skill, /research skill, personal skills panel

---

## Key Decisions

### CLAUDE.md — unified session template
- Merged `System/Brain.md` (Claude Code instructions block) + existing `CLAUDE.md` (branch workflow) into one clean file at repo root
- `Brain.md` stays as living personal memory; `CLAUDE.md` now covers identity, all protocols, git workflow, and vault structure map
- This is the pre-session template Claude reads at the start of every session

### /council skill
- Created `.claude/commands/council.md` — 5 council members (Analyst, Devil's Advocate, Visionary, Pragmatist, Ethicist) debate a topic in parallel, Chairman (MIMIR) delivers a ruling
- Committed to main so it loads as a project command

### /research skill
- Created `.claude/commands/research.md` — 5 parallel search agents + 3 adversarial verifiers, report filed to `Sources/`, tight summary returned to chat
- Fills the gap from the June 12 Roblox session where research protocol existed in Brain but no actual skill file did

### Personal skills panel
- Discovered `.claude/commands/` files are project commands (slash commands via `/` in chat), not Personal skills
- Personal skills are server-side, follow the user to every session
- Provided formatted content for Giahy to add `/council` and `/research` via the `+` button in the Skills tab

---

## Vault Changes
- `CLAUDE.md` — rewritten as full unified session instructions (merged from Brain.md + old CLAUDE.md)
- `.claude/commands/council.md` — new skill
- `.claude/commands/research.md` — new skill

---

## Where We Stopped

Both skills committed and pushed to main. Giahy adding them manually to Personal skills panel via the `+` button.

---

## Next Session Starting Point

1. Roblox go/no-go is still open — see [[System/Loose Ends]]
2. Skills infrastructure is complete — `/council` and `/research` ready to use

---

## References
[[System/Brain]] · [[System/Loose Ends]] · [[Sources/2026-06-12 Roblox Game Trends]]
