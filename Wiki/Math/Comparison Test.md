# Comparison Test

**One-liner:** Prove a series converges by bounding it above by a known convergent series, or prove it diverges by bounding it below by a known divergent series.

## Core Idea
**Direct Comparison Test:** If $0 \le a_n \le b_n$ for all n, then:
$$\sum b_n \text{ converges} \implies \sum a_n \text{ converges}$$
$$\sum a_n \text{ diverges} \implies \sum b_n \text{ diverges}$$

**Limit Comparison Test:** If $a_n, b_n > 0$ and $\displaystyle\lim_{n\to\infty} \frac{a_n}{b_n} = L$ with $0 < L < \infty$, then $\sum a_n$ and $\sum b_n$ both converge or both diverge.

The direct version requires an explicit inequality. The limit version only requires the two series to behave asymptotically alike — they shrink at the same rate.

## Why It Exists
Convergence tests like the [[Ratio Test]] or [[Integral Test]] are powerful but have failure modes (L = 1 is inconclusive for Ratio; integration must be tractable for Integral Test). The Comparison Test is the conceptually fundamental tool: if you can trap an unknown series between two known bounds, you inherit convergence/divergence from the bound. It also makes precise the intuition "this series looks like that known series."

## Real-World Applications
- **Numerical analysis — truncation error:** When approximating an infinite series by its first N terms, the tail error is bounded by comparison to a geometric or p-series. This is how calculators guarantee N digits of precision for special functions.
- **Probability — heavy-tailed distributions:** Proving that a distribution has finite mean E[X] = Σ n·P(X=n) often uses comparison to a p-series. Heavy-tailed distributions (power-law decay) may diverge; verifying this uses the Comparison Test.
- **Signal processing — Fourier series:** Proving that the Fourier series of a function converges often uses comparison: if |aₙ|, |bₙ| ≤ C/n², then Σ|aₙ| and Σ|bₙ| converge by comparison to Σ1/n², ensuring absolute convergence of the Fourier series.
- **Differential equations:** In power series solutions to ODEs (a key technique in EE for Bessel functions, Legendre polynomials), bounding the coefficients by a known convergent series proves the solution series converges everywhere needed.

## Intuition
**Direct Comparison:** Imagine two stacks of coins. If every coin in stack A is smaller than the corresponding coin in stack B, then if stack B reaches a finite total, stack A certainly does too. If stack A is already infinite, stack B (which is at least as large) must also be infinite.

**Limit Comparison:** Instead of requiring an exact term-by-term inequality, it asks: "do these two series grow at the same rate?" If $a_n/b_n \to L \in (0, \infty)$, then for large n, $a_n \approx L \cdot b_n$. Since multiplying every term by a finite constant doesn't change convergence, the two series share the same fate.

**The key skill** is recognizing which known series (geometric, p-series, etc.) your unknown series resembles, then making the comparison rigorous.

## Derivation
**Direct Comparison Test — Proof:**

Assume $0 \le a_n \le b_n$.

Let $S_N = \sum_{n=1}^N a_n$ and $T_N = \sum_{n=1}^N b_n$. Since all terms are non-negative, both are increasing sequences.

*Convergence case:* Suppose $\sum b_n = B < \infty$. Then $T_N \le B$ for all N. Since $a_n \le b_n$:
$$S_N \le T_N \le B$$
So $\{S_N\}$ is increasing and bounded above → converges by the Monotone Convergence Theorem. □

*Divergence case:* Suppose $\sum a_n$ diverges. Then $S_N \to \infty$. Since $b_n \ge a_n$, $T_N \ge S_N \to \infty$, so $\sum b_n$ diverges. □

**Limit Comparison Test — Proof:**

Suppose $a_n, b_n > 0$ and $a_n/b_n \to L$ with $0 < L < \infty$.

Choose $\varepsilon = L/2 > 0$. There exists N such that for all n > N:
$$\left|\frac{a_n}{b_n} - L\right| < \frac{L}{2}$$
$$\Rightarrow \frac{L}{2} < \frac{a_n}{b_n} < \frac{3L}{2}$$
$$\Rightarrow \frac{L}{2} b_n < a_n < \frac{3L}{2} b_n$$

If $\sum b_n$ converges: $a_n < \frac{3L}{2} b_n$, so $\sum a_n$ converges by direct comparison.

If $\sum b_n$ diverges: $a_n > \frac{L}{2} b_n$, so $\sum a_n$ diverges by direct comparison. □

## Worked Example
**Problem:** Does $\displaystyle\sum_{n=1}^{\infty} \frac{1}{n^2 + 3n + 2}$ converge?

**Step 1 — Identify the dominant behavior.** For large n, $n^2 + 3n + 2 \approx n^2$, so this behaves like $\sum 1/n^2$, a convergent p-series. Use Limit Comparison.

**Step 2 — Set up the limit.** Let $a_n = \dfrac{1}{n^2+3n+2}$ and $b_n = \dfrac{1}{n^2}$.

$$\frac{a_n}{b_n} = \frac{n^2}{n^2 + 3n + 2} = \frac{1}{1 + 3/n + 2/n^2} \xrightarrow{n\to\infty} 1$$

**Step 3 — Apply the test.** $L = 1 \in (0, \infty)$. Since $\sum \dfrac{1}{n^2}$ converges ([[p-Series]] with p = 2 > 1), so does $\displaystyle\sum \dfrac{1}{n^2 + 3n + 2}$.

**Bonus (direct comparison):** Note $n^2 + 3n + 2 > n^2$, so $\dfrac{1}{n^2+3n+2} < \dfrac{1}{n^2}$. This also works directly.

## See Also
- [[p-Series]] — the most common benchmark series to compare against
- [[Geometric Series]] — another standard comparison target
- [[Harmonic Series]] — common divergent benchmark for lower bounds
- [[Convergence]] — what convergence means formally
- [[Ratio Test]] — often used where Comparison is cumbersome (factorials)
- [[Integral Test]] — alternative for series matching integrable functions
- [[Alternating Series Test]] — for series with sign changes, Comparison applies to absolute values
- [[Taylor Series]] — power series coefficient bounds often use Comparison to prove convergence
