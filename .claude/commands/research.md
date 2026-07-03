# /research — MIMIR Deep Research

Conduct deep, adversarially-verified research on the topic in `$ARGUMENTS` and file the report to the vault.

---

## Phase 1 — Fan-Out Search (parallel)

Spawn **5 search agents in parallel**, each with a distinct angle on the topic:

| Agent | Focus |
|-------|-------|
| 1 | Core facts, definitions, current state |
| 2 | Data, statistics, market or quantitative signals |
| 3 | Expert opinions, debates, open questions |
| 4 | Counterarguments, risks, failure cases |
| 5 | Adjacent context — trends, comparisons, recent developments |

Each agent returns: a list of key claims with sources, and a confidence level (high / medium / speculative) per claim.

---

## Phase 2 — Adversarial Verification (parallel)

Spawn **3 verification agents in parallel**, each independently reviewing the full set of claims from Phase 1:

- **Verifier A** — checks for factual conflicts between agents
- **Verifier B** — flags claims that are speculative but presented as fact
- **Verifier C** — identifies missing perspectives or blind spots

Each verifier marks claims as: ✅ Confirmed | ⚠️ Contested | ❌ Refuted | 🔍 Needs more

A claim survives if at least 2 of 3 verifiers pass it.

---

## Phase 3 — Synthesis

Compile a structured research report with:

1. **Executive Summary** — 3–5 sentences. The most important things to know.
2. **Key Findings** — bullet points, surviving claims only. Source-attributed where possible.
3. **Contested / Uncertain Areas** — what's debated, what remains unclear
4. **Recommendation or Implication** — what this means for Giahy specifically, given context in `System/Brain.md`
5. **Sources referenced**
6. **Verification scorecard** — X/Y claims survived adversarial review

---

## Phase 4 — File to Vault

1. Save the full report as `Sources/YYYY-MM-DD <Topic>.md` (use today's date)
2. Add a loose end to `System/Loose Ends.md` if the research surfaces an open decision or unanswered question Giahy needs to act on

---

## Output to Chat

Return a **tight summary** — not the full report. Structure:

```
## Research Complete: <Topic>

**Bottom line:** <1–2 sentences — the most actionable finding>

**Key findings:**
- <finding 1>
- <finding 2>
- <finding 3>
- ...

**Confidence:** <X>/<Y> claims passed adversarial review

**Contested:** <what's uncertain or debated>

**Filed:** Sources/YYYY-MM-DD <Topic>.md
```

Do not paste the full report into chat. The full report is in `Sources/`.
