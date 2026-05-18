# MIMIR_SESSION_LOG

> Session continuity log. MIMIR updates this at the end of every session. New sessions read the most recent entry first to resume context.

---

## How to use this file

**At session start:** Read the most recent entry. It tells you exactly where we left off and what the next step is.
**At session end:** Append a new entry with the date, what was covered, decisions made, open questions, and where to pick up next.

---

## 2026-05-18 — Grill Session: MIT Transfer Plan

**Session type:** Grillme skill — stress-testing the full plan  
**Branch this occurred on:** claude/code-review-zVyHt (edits lost — rebuilt from screenshots in this session)

### What was covered

**Q1–Q3: Transfer target framing**
- Transfer targets confirmed: MIT, Stanford, UC Berkeley, UCSD (all EE/ECE track, Fall 2027)
- Critical insight: two separate application cycles — UC apps (Berkeley + UCSD) due Nov 2026, MIT + Stanford due Mar 2027
- SAT must be taken Oct 3 to hit MIT Spring 2027 window AND to be done before UC app season opens
- Decision: No MIT Spring 2027 pursuit — targeting Fall 2027 only

**Q4: Spring vs Fall 2027**
- Confirmed: Fall 2027 only. Spring 2027 not being pursued.

**Q5: Robotics portfolio**
- Target project: Baymax Home Companion Robot (modeled on Disney Imagineer's Olaf)
- GPA: ~3.98 (A- in 1-credit spring lab — rounds to 4.0 everywhere, not a problem)
- Clarification: Claude wrote the detailed Baymax project notes — they are a design TARGET, not a description of current skills. Giahy is learning toward them.
- Phase 1 is software-only (Isaac Sim, Python, RL) — $0 cost
- Hardware Phase 2 is funding-dependent (3D printer, parts ~$8–10k total)

**Q6–Q7: Trading / financial situation**
- All 3 prop firm accounts blown May 2026
- Documented causes: overtrading, chasing moves, skipping pre-market setup — psychology under financial pressure
- Job income ($1,400/month) covers expenses + minimums only, no debt paydown, no hardware
- Additional income: Amazon Flex (TBD) + plasma donations 2x/week (TBD)
- June restart plan: 5 fresh accounts, small wins approach, patience over urgency
- Key rule established: Do NOT link Baymax funding to trading urgency — that creates the psychology that blows accounts
- Second possible project: portable warming massage bottle belt (friend's idea, commercial potential — needs outreach)

**Q8: Learning path**
- CS 1410 (Java OOP) this summer bridges to Python, but doesn't directly connect to RL
- Path: Python fluency (Jun–Jul) → NumPy/linear algebra (Jul–Aug) → basic RL concepts (Aug–Sep) → Isaac Sim setup (Sep–Oct)
- Desktop (RTX 3060 Super, i9 10th gen, 16GB RAM) runs Isaac Sim fine
- Laptop (i7-1355U, Iris Xe) for light learning/coding only

**Q8 pivot: Course correction**
- Major update: Giahy is NOT taking Multivariate Calc + Diff Eq this summer as MIMIR_TASKS had listed
- Actual summer courses: Retaking Calc 2 (MATH 1220) + Physics 1 (PHYS 2210) — had C grades in both
- Diff Eq/Linear Algebra → Fall/Spring respectively (per course plan screenshot)
- C grades matter significantly — all four target schools care about Calc 2 and Physics 1
- **OPEN QUESTION:** Does SLCC have grade replacement policy, or do both grades appear? Critical for transcript strategy.

**Schedule (finalized for summer):**
| Time | Mon | Tue | Wed | Thu | Fri |
|------|-----|-----|-----|-----|-----|
| 7:30–10:00 | Trading | Trading | Trading | Trading | Trading |
| 10:00–12:00 | Calc 2 | Calc 2 | Calc 2 | SAT prep | SAT prep |
| 1:00–3:00 | Physics 1 | Physics 1 | Physics 1 | Python/Robotics | Python/Robotics |
| 3:00–4:30 | SAT prep | SAT prep | SAT prep | Python/Robotics | Python/Robotics |
| 4:30–6:30 | IOP | IOP | IOP | IOP | — |
| 7:00–9:00 | Async CS/English | Async CS/English | Async CS/English | Natalie | Natalie |

IOP is Mon–Thu only (NOT Fridays — corrected from previous MIMIR entry)

**Q9: Laptop/hardware**
- Desktop clears Isaac Sim spec. Simulation work is not blocked by hardware.
- Amazon Flex + plasma blocks to be slotted once times are confirmed

**Q10: Fall 2026 course plan (session ended here)**
- Fall 2026 plan from screenshot: EE 1270 + PHYS 2220 (Physics 2/E&M) + CHEM 1210 + MATH 2210 (Multivariate Calc)
- Previous Claude was about to ask: "What are you currently planning to take alongside Linear Algebra in the fall, and what does your advisor say about Physics 2 in your sequence?"
- User attempted to share SLCC plan screenshot → API error 400 (cache_control on empty text block) → session died

### Decisions made
1. Targeting Fall 2027 entry at all four schools (not Spring 2027)
2. SAT on Oct 3 (primary) — not delayed to 2027
3. Baymax project: software-first, hardware later
4. Trading restart: June, small wins approach, no urgency dependency on project funding
5. IOP is Mon–Thu only (not Fridays)
6. Evenings (7–9 PM) are usable for async coursework Mon–Wed

### Open questions / unresolved from this session
1. SLCC grade replacement policy — critical, needs answer ASAP
2. Fall 2026 course plan details — Q10 was unanswered when session crashed
3. Amazon Flex + plasma schedule — to be confirmed and slotted
4. Trading journal folder structure — noted, separate session
5. Hard trading rules / post-mortem — to be done before June restart

### Where to pick up next session
**Resume at Q10:** Fall 2026 course plan. The question is whether EE 1270 + PHYS 2220 + CHEM 1210 + MATH 2210 is the right load, and whether Berkeley's requirements are satisfied by what's in the Fall plan.

After Q10, grill questions remaining:
- The weekly schedule doesn't have Amazon Flex or plasma blocks yet — needs to be finalized
- Trading post-mortem + hard rules session (separate)
- Trading journal folder build (separate)
