# Divergence

**One-liner:** The failure of a series or sequence to converge — the partial sums grow without bound, oscillate, or otherwise refuse to settle at a finite value.

## Core Idea
$$\sum_{n=1}^{\infty} a_n \text{ diverges} \iff \lim_{N\to\infty} S_N \text{ does not exist as a finite real number}$$
The Divergence Test (also called the $n$-th term test) provides a quick first check: if $\lim_{n\to\infty} a_n \neq 0$, then $\sum a_n$ diverges. Critical caveat: if $\lim_{n\to\infty} a_n = 0$, the test is **inconclusive** — the series might still diverge (see [[Harmonic Series]]).

## Why It Exists
Divergence is not a failure of mathematics — it's a real phenomenon. Some infinite processes don't stabilize, and we need to identify them reliably before attempting further calculations. Applying convergence tests or manipulations to a divergent series yields nonsense (the famous "1 + 2 + 3 + ... = -1/12" requires analytic continuation, not ordinary summation). Divergence theory sets the boundaries of what's valid.

## Real-World Applications
- **Engineering — Series Resonance:** In an undamped LC circuit, the response to a resonant input grows without bound — a physical manifestation of divergence. Engineers must avoid resonance in bridges, circuits, and aircraft (Tacoma Narrows bridge collapse).
- **Numerical stability:** Divergent iteration sequences in numerical methods (e.g., Newton's method with bad initial guess, gradient descent with too-large learning rate) cause computations to "blow up." Detecting divergence early is critical. See [[Gradient Descent]].
- **Signal Processing:** An unstable digital filter has poles outside the unit circle — its impulse response sequence diverges. Filter design (EE) requires placing poles inside the unit circle to guarantee convergence.
- **Harmonic series in acoustics:** The [[Harmonic Series]] diverges, explaining why you can't bound the total energy radiated by a point source summed over all harmonics without damping — a real problem in room acoustics modeling.

## Intuition
Think of divergence as a series that's "leaking." No matter how many terms you add, the partial sum never plateaus — it keeps climbing (or oscillating). The Divergence Test's intuition: if individual terms don't approach zero, you're forever adding non-negligible amounts, so the sum must grow without bound. But terms going to zero is merely necessary — not sufficient. The harmonic series is the canonical proof that small terms alone don't guarantee convergence.

**Types of divergence:**
1. **Divergence to infinity:** $S_N \to +\infty$ (e.g., harmonic series)
2. **Oscillatory divergence:** $S_N$ oscillates between values (e.g., $\sum (-1)^n$: partial sums alternate between 0 and 1)
3. **Divergence to $-\infty$**

## Derivation
**Divergence Test — proof:**

**Claim:** If $\sum_{n=1}^\infty a_n = S$ (converges), then $\lim_{n\to\infty} a_n = 0$.

**Proof:** Let $S_N = \sum_{k=1}^N a_k$. Since $\sum a_n$ converges to $S$, we have $S_N \to S$. Note that $a_n = S_n - S_{n-1}$. Therefore:
$$\lim_{n\to\infty} a_n = \lim_{n\to\infty} (S_n - S_{n-1}) = S - S = 0 \quad \square$$

**Contrapositive (the Divergence Test):** If $\lim_{n\to\infty} a_n \neq 0$ (or the limit doesn't exist), then $\sum a_n$ diverges.

**Why the converse fails:** $a_n = 1/n \to 0$, but $\sum 1/n$ diverges (see [[Harmonic Series]] for proof). The terms getting small is necessary but not sufficient — they must go to zero fast enough.

**Comparison approach to divergence:** If $\sum b_n$ diverges and $a_n \geq b_n \geq 0$ for all large $n$, then $\sum a_n$ diverges. See [[Comparison Test]].

## Worked Example
**Problem:** Does $\sum_{n=1}^\infty \dfrac{n}{2n + 1}$ converge or diverge?

**Step 1 — Apply the Divergence Test.** Compute $\lim_{n\to\infty} a_n$:
$$\lim_{n\to\infty} \frac{n}{2n+1} = \lim_{n\to\infty} \frac{1}{2 + 1/n} = \frac{1}{2} \neq 0$$

**Step 2 — Conclude.** Since the terms approach $1/2 \neq 0$, the series **diverges** by the Divergence Test.

**Interpretation:** We're forever adding approximately $1/2$ to the partial sum. Of course it diverges.

## See Also
- [[Convergence]] — the opposite; what it means for a series to converge
- [[Divergence Test]] — the first test to apply; quick necessary condition
- [[Harmonic Series]] — canonical example of divergence despite terms → 0
- [[Sequence]] — sequence divergence (terms → ∞ or oscillate)
- [[Comparison Test]] — prove divergence by comparison to known divergent series
- [[Ratio Test]] — L > 1 implies divergence
- [[p-Series]] — diverges when p ≤ 1
