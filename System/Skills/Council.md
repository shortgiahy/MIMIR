# Skill — /council

**Trigger:** `/council <topic>`
**Location:** `.claude/commands/council.md`
**Also available as:** Personal skill (add via `+` in Skills tab)

---

## What It Does

Convenes a panel of 5 sub-agents, each arguing a topic from a distinct perspective, across **two rounds of debate**. MIMIR chairs the session and delivers a final ruling after the council has genuinely engaged with each other's challenges.

---

## The Council Seats

| Seat | Name | Lens |
|------|------|------|
| 1 | **The Analyst** | Data-driven — "what does the evidence say?" |
| 2 | **The Devil's Advocate** | Challenges assumptions — "what could go wrong?" |
| 3 | **The Visionary** | First-principles, big-picture — "what's the ideal outcome?" |
| 4 | **The Pragmatist** | Ground-level, resource-aware — "what can actually be done?" |
| 5 | **The Stress Tester** | Ruthlessly adversarial — "where does this break, and how badly?" |

**Note on The Stress Tester:** Replaced the Ethicist. Their job is not to be balanced or fair — it's to find the single most dangerous assumption in the plan, name the failure mode everyone else is ignoring, and pressure-test until something gives. If nothing breaks, that's also a result.

---

## How It Runs

**Phase 1 — Opening Statements**
5 subagents in parallel. Each gives a 3–5 sentence take from their seat's perspective, ending with a sharp question directed at a specific named council member.

**Phase 2 — Rebuttal Round**
5 subagents in parallel again, each receiving the full Phase 1 output. They respond to the sharpest challenge against their position (especially from The Stress Tester), then direct a follow-up challenge at another member. No repeating — they must actually engage and shift or sharpen.

**Cross-Examination**
MIMIR identifies the two sharpest unresolved tensions after the rebuttal round.

**Chairman's Ruling**
MIMIR's synthesis and position. Must address The Stress Tester's most dangerous finding directly. No fence-sitting.

---

## Output Structure

1. Opening Statements (5 panels)
2. Rebuttal Round (5 panels)
3. Cross-Examination (2 sentences)
4. Chairman's Ruling (4–6 sentences + directive)

---

## When to Use

- Evaluating a major decision with real tradeoffs
- Stress-testing a plan before committing
- Getting unstuck on a problem with competing considerations
- Any time "what do I think about X?" needs more than one angle — and needs the weakest points surfaced, not just the strongest ones

---

## Example

```
/council Should I prioritize the UC application cycle over SAT prep this summer?
```
