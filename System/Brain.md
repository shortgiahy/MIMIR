# Brain

> Git/branch workflow, environment, and GitHub API rules live in `CLAUDE.md` — this file doesn't duplicate them.

> Living memory. Maintained by MIMIR. Last updated: 2026-07-03

---

## Current Role

Personal AI system — scheduling, planning, context maintenance, accountability, research, and systems design. Named MIMIR by Giahy.

*MIMIR updates this when the role evolves.*

---

## Active Domains

- **School** — SLCC, summer 2026. Retaking MATH 1220 + PHYS 2210 (had C grades). Taking ENGL 1010. CS 1410 dropped June 1 — now 3 courses (11 credits).
- **Trading** — ES + MGC orderflow, all 3 prop firm evals blown May 2026. Waiting on capital to fund new accounts. July 1 restart target.
- **Job** — mom's salon (selling, so job ending soon). Bridge income in transition. Amazon Flex + plasma (~$430/mo) replacing it.
- **Additional income** — Plasma Mon/Fri 10–12 (~$430/month) + Amazon Flex 5–8:30 PM (~$2,250/month)
- **Relationship** — Natalie is the primary priority. Always.

---

## Active Projects

- **Baymax Home Companion Robot** — portfolio anchor for MIT/Stanford transfer. Phase 1: software only (Python → NumPy → RL → Isaac Sim). Desktop (RTX 3060 Super) runs Isaac Sim. No hardware funding yet — hardware is Phase 2.
- **Sushi Sea (Roblox game)** — Dave the Diver × RuneScape × Fisch × restaurant sim. GDD v2 complete and scaffolded in `Projects/Sushi Sea/`. Agency agents installed. **Council ruling (2026-06-17): viable — proceed.** Execution resequenced: Sprint 1 is exclusively the cast-hook-reel loop (feel-tuning gate before any backend). Open Thread #1 (cook verb) is the active design blocker. Implementation tracked in `Projects/Sushi Sea/Implementation Status.md`.
- **Heated Lotion Belt** — friend's commercial product concept (heated lotion bottle belt for massage therapists). Greenlit June 2026. Early stage — scope + timeline TBD. Not a summer income source.
- **SAT prep** — Test dates: October 3 (primary) + November 7, 2026 (backup). 10hrs/week target. Target: 1550+. Not yet started.
- **Transfer applications** — Four schools: MIT + Stanford (Mar 2027, SAT required) + UC Berkeley + UCSD (Nov 2026, no SAT). Two separate cycles. UC essays needed by Sep 2026.
- **Prop firm eval** — Restarting June with 5 accounts. Small wins. No urgency pressure. Waiting on capital.

---

## Current Goals

### This Week
*Not set — pending next weekly check-in.*

### This Month
*Not set — pending next monthly check-in.*

- [ ] Build Python learning routine into weekly schedule

---

## MIMIR Protocols

*Voice: see CLAUDE.md → Identity & Tone.*

### Good Morning Routine
**Trigger:** Giahy says "good morning" (or variant)

**Sequence:**
1. Read the last 5 notes in `Session Notes/` + [[System/Brain]] + [[System/Tasks]] + [[Giahy/Profile/Profile]] — get current state
2. Pull Google Calendar — today's events + next 72 hours
3. Check [[System/Loose Ends]] — anything due or overdue
4. Roll over any unfinished quick tasks from yesterday's daily note
5. Ask Giahy: morning journal (1–3 sentences) + any quick tasks for today + yesterday's trade (if trading day)
6. If a trade was taken: create filled journal entry in `Trading/Journals/YYYY-MM-DD.md`
7. Output using `Daily/Daily Template.md`:
   - Morning journal
   - Quick tasks (rolled over + new, max 5 total)
   - Time-blocked agenda for today
   - Upcoming flags (next 72hrs)
   - Trade summary if applicable
8. Save as `Daily/YYYY-MM-DD.md`

**Quick tasks:** Small, completable-today items. Max 5. Not projects. Captured anytime — during check-in or mid-session. MIMIR manages rollover each morning.

### Session End Protocol
Before any session closes (or when Giahy says "wrap up"):
1. Update [[System/Brain]] — goals, patterns, active projects if changed
2. Sync [[System/Loose Ends]] — open anything new, close anything resolved
3. Append summary to today's `Daily/YYYY-MM-DD.md` if it exists
4. Create a new note in `Session Notes/` — one note per session, named `YYYY-MM-DD Topic.md`. Keep it lean: decisions made, where we stopped, next starting point. Recurring items and loose ends go in their own files — do not duplicate here.

### Loose Ends Protocol
MIMIR adds to [[System/Loose Ends]] **proactively and without asking**. This file exists precisely so MIMIR can capture things Giahy doesn't explicitly flag — ambiguities, unanswered questions, things that surfaced mid-conversation but weren't resolved. Giahy does not need to say "add that to loose ends." If MIMIR notices it, MIMIR logs it.

Do not ask for confirmation before adding a loose end. Do not wait until session end — add it when it's noticed. Close loose ends when resolved.

**Language signal:** When Giahy says "deal with it later," "we'll get to that," "that's for another time," or any equivalent deferral — that means add it to [[System/Loose Ends]] immediately.

### Session Start Protocol
At the start of any new session:
1. Read the last 5 notes in `Session Notes/` (by date) — they tell you where we stopped and what's been decided
2. Read [[System/Brain]] — current state
3. Read [[System/Tasks]] — active tasks and schedule
4. Resume from the logged stopping point, or ask Giahy what he wants to focus on

### Check-In Cadence
- **Daily** — every morning. Use `Daily/YYYY-MM-DD.md` from `Daily/Daily Template.md`. Ritalin + anchor + tasks + flags. Evening: check off anchor, set tomorrow's anchor.
- **Weekly** — every Monday morning. Use `System/Weekly Check-in Template.md`. Review last week, pull Canvas deadlines, set anchor theme.
- **Monthly** — first of the month. Big picture: are priorities still right? What shifted?

### Session Note System
Every session gets a note in `Session Notes/` using `Session Notes/Session Template.md`. Named `YYYY-MM-DD Topic.md`. MIMIR creates this at session end (or when Giahy says "wrap up"). Keep it lean — decisions, vault changes, where we stopped, next starting point. No duplicating content that lives in dedicated files.

### Research Protocol
When Giahy asks for research (market research, deep dives, comparisons — anything beyond a quick lookup):
1. Run the research — multi-source, verified, cited (deep-research skill when available).
2. Create a research note in `Sources/` named `YYYY-MM-DD Topic.md` — bottom-line recommendation up top, then full findings with key numbers and sources.
3. Link both ways: the research note links to the session note that produced it, and the session note's References include the research note. Research is never delivered chat-only — if it isn't in the vault, it didn't happen.
4. Keep the session note lean — decision and outcome only. The full report lives in the research note.

---

## Vision

### The North Star
Freedom. Not wealth for its own sake — freedom to choose the work, the life, the people. Success that isn't shared with Natalie isn't success. She is the primary constant in every plan.

### Academic Path
- **Current:** SLCC, EE, summer 2026, GPA ~3.98 (A- in 1-credit spring lab; retaking Calc 2 + Physics 1)
- **Major target:** Electrical Engineering → Robotics emphasis
- **Transfer targets (4 schools, 2 cycles):**
  - UC Berkeley (EECS) + UCSD (ECE) — Fall 2027, apply Nov 2026, no SAT, test-blind
  - MIT + Stanford — Fall 2027, apply Mar 2027, SAT 1550+ required
- **Backup path:** SLCC → 4-year university → MIT grad school (MS or funded PhD in Robotics). Funded PhD is a real option.
- **SAT dates:** October 3, 2026 (primary) + November 7, 2026 (backup)
- **Why MIT:** #1 robotics program (CSAIL). The Engine fund for tough-tech startups. The exact ecosystem for what comes after.

### Trading Path
- **Instruments:** ES (primary), MGC (primary), MNQ + GC (secondary)
- **Strategy:** Orderflow — volume profiles, DOM, delta
- **Current status:** Reset. All 3 accounts blown May 2026. Restarting June with 5 accounts.
- **Documented failure causes:** Overtrading, chasing moves, skipping pre-market setup. Psychology under financial pressure.
- **Fix framing:** System design problem, not a character flaw. Hard rules and protocols, not willpower.
- **July approach:** Small wins, patience, no urgency about payouts. Income grows gradually.
- **Income target:** $20,000/month net (10 accounts × $2,000 payout each)
- **Milestone 1:** $3–5k/month. **Milestone 2:** $12k/month.

### Financial Picture
**Monthly fixed burn:** ~$2,282
- Rent: $2,000 (Natalie currently covering more — Giahy takes on full share when payouts begin)
- Phone: $140
- Pet insurance (Winston + Benni): $60
- TradeSyncer: $50
- Notion: $12
- Claude: $20

**Debt (priority #1 — everything else waits):**
| Debt | Amount |
|---|---|
| Capital One Slate | $7,000 |
| Capital One Platinum | $1,300 |
| Capital One Venture One | $300 |
| Chase Freedom Unlimited | $500 |
| Chase Slate Edge | $500 |
| Medical | $800 |
| **Total** | **~$10,400** |

Cap One Slate ($7k) is the anchor. It goes first.

**After debt, in order:**
1. 6 months emergency savings
2. Roth IRA maxed annually
3. One-time needs: contacts, hair, teeth, phone/tablet payoff
4. Equipment: Bambu P1S, oscilloscope, soldering station, DMM, robot parts, stronger laptop

### Long-Term Vision
- **Startup:** Found an engineering company focused on philanthropic impact — robotics applied to environmental improvement or medical advancement. Not charity — a real company that does meaningful work and builds wealth.
- **MIT The Engine:** Target ecosystem for this.
- **Family:** Kids, pets, maybe a live-in. A home large enough to be comfortable — not a status symbol.
- **Lifestyle:** Not materialistic. Never have to check if something is affordable. Take care of family. Give back to community.
- **Natalie:** Co-navigator. She runs a marketing agency. Her vision aligns. Success is shared or it doesn't count.

---

## Patterns Observed

- Prone to overtrading and chasing moves in trading — system rules matter more than discipline here
- Misses trades when pre-market setup is skipped — routine enforcement is critical
- ADHD: energy isn't linear, minimize choices, break tasks into clear steps
- Extremely forgetful — proactive reminders are a core MIMIR responsibility
- Tends to conflate "starting a project" with "funding a project" — can start Baymax software with $0
- Under financial pressure, urgency thinking emerges — this is the psychology that blew the accounts
- **MDD (Major Depressive Disorder)** confirmed, comorbid with ADHD — IOP (DBT + EMDR), 50mg Sertraline, 36mg Ritalin
- Ritalin not taken consistently — highest-leverage behavioral change available right now
- Initiation failure at the **assessment phase** — thinking about a task triggers overwhelm + dread + blankness simultaneously, sometimes physical headache. Applies to all domains including eating.
- Broken reward pathway: brain trained on instant dopamine (video games, TikTok) → real-life delayed rewards feel unreachable → avoidance loop deepens MDD
- Video games are primary dopamine refuge — not to be eliminated but earned (anchor first, always)
- IOP team has not been given full picture of severity — Giahy needs to correct this
- **Current phase:** Phase 1 Foundation. Ritalin daily + one anchor/day + 10 push-ups + noon eating alarm. Games unlock after anchor only. Phase 2 adds more once this is stable 2+ weeks.
