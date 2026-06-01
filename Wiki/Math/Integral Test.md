# Integral Test

**One-liner:** If a function f is positive and decreasing on [1,∞) with f(n) = aₙ, then the series Σaₙ and the improper integral ∫f(x)dx either both converge or both diverge.

## Core Idea
$$\text{If } f \text{ is positive, continuous, and decreasing on } [1,\infty)\text{, then}$$
$$\sum_{n=1}^{\infty} a_n \text{ and } \int_{1}^{\infty} f(x)\,dx \text{ both converge or both diverge}$$
The test works because each term aₙ = f(n) can be interpreted as the area of a rectangle of width 1 and height f(n). These rectangles over- and under-approximate the area under the curve, so the sum and integral are bounded in terms of each other. The test tells you convergence behavior, but not the exact sum.

## Why It Exists
Many series have terms that are easy to integrate but hard to sum directly. The p-series Σ1/nᵖ is the canonical example — there is no closed-form partial sum, but ∫1/xᵖ dx integrates cleanly. The Integral Test converts an unclear series question into a familiar calculus problem.

## Real-World Applications
- **p-Series classification:** The proof that Σ1/nᵖ converges for p > 1 and diverges for p ≤ 1 is the Integral Test applied to f(x) = 1/xᵖ. This classification underlies all of Fourier analysis and signal processing — the decay rate of Fourier coefficients determines the regularity of a signal.
- **Error estimation:** The Integral Test provides explicit remainder bounds. In numerical analysis, you can use ∫ f(x)dx from N to ∞ to bound the tail of a series — critical when computing physical constants or special functions to required precision.
- **Entropy in information theory:** Convergence of certain series involving log terms (e.g., related to entropy sums) is verified via the Integral Test.
- **Physics — density of states:** In statistical mechanics, sums over quantum energy levels sometimes reduce to integrals via the Integral Test, bridging discrete quantum systems to continuous thermodynamic quantities.

## Intuition
Draw the curve y = f(x) for x ≥ 1. Now draw rectangles of width 1 over each integer interval.

**Right-endpoint rectangles** (height f(2), f(3), …): each rectangle fits *under* the curve on its interval, so their total area ≤ ∫₁^∞ f(x)dx. This gives: Σₙ₌₂^∞ aₙ ≤ ∫₁^∞ f(x)dx.

**Left-endpoint rectangles** (height f(1), f(2), …): each rectangle *contains* the curve piece, so ∫₁^∞ f(x)dx ≤ Σₙ₌₁^∞ aₙ.

Chaining these: if the integral converges, the series is squeezed finite; if the integral diverges to ∞, the series is bounded below by a diverging quantity.

## Derivation
**Theorem:** Let f be continuous, positive, and decreasing on [1,∞) with f(n) = aₙ. Then Σaₙ converges iff ∫₁^∞ f(x)dx converges.

**Proof:**

Since f is decreasing, for each integer n ≥ 1 and x ∈ [n, n+1]:
$$f(n+1) \le f(x) \le f(n)$$

Integrate over [n, n+1]:
$$f(n+1) \le \int_{n}^{n+1} f(x)\,dx \le f(n)$$
$$a_{n+1} \le \int_{n}^{n+1} f(x)\,dx \le a_n$$

Sum the right inequality from n = 1 to N:
$$\sum_{n=1}^{N} \int_{n}^{n+1} f(x)\,dx = \int_{1}^{N+1} f(x)\,dx \ge \sum_{n=1}^{N} a_{n+1} = S_{N+1} - a_1$$

So $S_{N+1} \le a_1 + \int_1^{N+1} f(x)\,dx$.

If ∫₁^∞ f(x)dx = I < ∞, then $S_N \le a_1 + I$ for all N. Since Sₙ is increasing (all terms positive) and bounded above, $\sum a_n$ converges. ✓

Sum the left inequality from n = 1 to N:
$$\int_{1}^{N+1} f(x)\,dx \ge \sum_{n=1}^{N} a_{n+1} = S_N - a_1 + a_{N+1} \ge S_N - a_1$$

Wait — use the left inequality directly: $\int_1^{N+1} f(x)dx \le \sum_{n=1}^{N} a_n = S_N$.

If $\int_1^\infty f(x)dx = \infty$, then $S_N \ge \int_1^{N+1}f(x)dx \to \infty$, so the series diverges. ✓

## Worked Example
**Problem:** Does $\displaystyle\sum_{n=2}^{\infty} \frac{1}{n\ln n}$ converge or diverge?

**Step 1 — Verify conditions.** Let $f(x) = \dfrac{1}{x\ln x}$. For x ≥ 2: f is positive, continuous, and decreasing (both x and ln x increase). Conditions satisfied.

**Step 2 — Compute the integral.**
$$\int_{2}^{\infty} \frac{1}{x\ln x}\,dx$$

Substitute $u = \ln x$, $du = dx/x$:
$$= \int_{\ln 2}^{\infty} \frac{du}{u} = \Big[\ln u\Big]_{\ln 2}^{\infty} = \infty$$

**Step 3 — Conclude.** The integral diverges, so the series $\displaystyle\sum_{n=2}^{\infty} \dfrac{1}{n\ln n}$ **diverges**.

(Note: this diverges *slower* than the [[Harmonic Series]], illustrating that the boundary between convergence and divergence can be razor-thin.)

## See Also
- [[Convergence]] — what the test is detecting
- [[Divergence]] — the other outcome
- [[p-Series]] — the most important application of the Integral Test
- [[Comparison Test]] — alternative when integration is hard
- [[Harmonic Series]] — a divergent series also provable by the Integral Test
- [[Ratio Test]] — often better for series with factorials or exponentials
- [[Improper Integrals]] — the integral machinery the test relies on
