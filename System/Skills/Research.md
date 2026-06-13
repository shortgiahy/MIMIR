# Skill — /research

**Trigger:** `/research <topic>`
**Location:** `.claude/commands/research.md`
**Also available as:** Personal skill (add via `+` in Skills tab)

---

## What It Does

Deep, adversarially-verified research on any topic. Uses a multi-agent fan-out to cover multiple angles, then cross-checks all claims before synthesizing. Full report filed to `Sources/`; tight summary returned to chat.

---

## How It Works

**Phase 1 — Fan-Out (5 agents in parallel)**

| Agent | Focus |
|-------|-------|
| 1 | Core facts, definitions, current state |
| 2 | Data, statistics, quantitative signals |
| 3 | Expert opinions, debates, open questions |
| 4 | Counterarguments, risks, failure cases |
| 5 | Adjacent context — trends, comparisons, recent developments |

**Phase 2 — Adversarial Verification (3 verifiers in parallel)**

- Verifier A: checks for factual conflicts between agents
- Verifier B: flags speculative claims presented as fact
- Verifier C: identifies missing perspectives or blind spots

Claims are marked ✅ Confirmed / ⚠️ Contested / ❌ Refuted / 🔍 Needs more.
A claim survives if 2 of 3 verifiers pass it.

**Phase 3 — Synthesis**

Structured report: executive summary, key findings (surviving claims only), contested areas, recommendation for Giahy specifically, sources, verification scorecard.

**Phase 4 — Vault Filing**

Full report saved to `Sources/YYYY-MM-DD <Topic>.md`. Backlink added to session note. Loose end logged if an open decision surfaces.

---

## Output to Chat

Tight summary only — bottom line, key findings, confidence score, contested areas, file path. The full report is in `Sources/`.

---

## When to Use

- Any topic requiring more than a quick answer
- Market research, academic topics, decision research
- Anything where "I think" isn't good enough

---

## Example

```
/research ES futures orderflow strategies — what separates profitable setups from noise
```
