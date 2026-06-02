# p-Series

**One-liner:** The family of series $\sum 1/n^p$, which converges if and only if $p > 1$ — the benchmark series for the Comparison Test.

## Core Idea
$$\sum_{n=1}^{\infty} \frac{1}{n^p} \begin{cases} \text{converges} & \text{if } p > 1 \\ \text{diverges} & \text{if } p \leq 1 \end{cases}$$
For $p = 1$, this is the [[Harmonic Series]] (diverges). For $p = 2$, $\sum 1/n^2 = \pi^2/6$ (Basel problem). For $p > 1$, the series converges; for $p \leq 1$, it diverges. The p-series is the canonical benchmark — when testing an unknown series by comparison, you compare it to a p-series.

## Why It Exists
The p-series is important because it provides a clean, one-parameter family that interpolates between "clearly convergent" ($p$ very large) and "clearly divergent" ($p = 1$). It gives us the simplest non-geometric convergent series, and its convergence is provable by the Integral Test — tying series theory directly to improper integrals. It's also the Riemann zeta function $\zeta(p)$ evaluated on the real line.

## Real-World Applications
- **Riemann zeta function / Number theory:** $\zeta(s) = \sum_{n=1}^\infty n^{-s}$ is the p-series generalized to complex $s$. It encodes information about the distribution of prime numbers (via the Euler product formula $\zeta(s) = \prod_p (1-p^{-s})^{-1}$). The Riemann Hypothesis is about zeros of $\zeta(s)$. One of the deepest unsolved problems in mathematics.
- **Physics — power law distributions:** Many natural phenomena follow power laws: earthquake magnitudes, city sizes, income distributions ($P(x) \sim x^{-p}$). Convergence of the p-series determines whether the mean or variance of these distributions is finite. Critical for risk assessment in EE (fading channels), ecology, and finance.
- **Signal processing — 1/f noise:** Many physical noise sources have power spectral density $\sim 1/f^\alpha$ for some $\alpha$. The total power $\int_0^\infty f^{-\alpha} df$ converges iff $\alpha > 1$ — directly analogous to the p-series condition.
- **Numerical analysis:** Convergence rates of many approximation schemes are $O(h^p)$, where understanding which p gives sufficient accuracy connects directly to p-series intuition.

## Intuition
Visualize the terms $1/n^p$ as bar heights in a histogram. For small $p$ (say $p = 1.001$), the bars drop off very slowly — the area under the "curve" of bars barely converges. For large $p$ (say $p = 3$), bars drop off steeply — clearly finite area.

The threshold at $p = 1$ is the Integral Test in disguise: $\int_1^\infty x^{-p}\, dx$ converges iff $p > 1$. Since the series and integral converge together (by the Integral Test), $p > 1$ is the exact boundary.

## Derivation
**Proof via Integral Test:**

Let $f(x) = 1/x^p = x^{-p}$. This is positive, continuous, and decreasing on $[1, \infty)$.

By the [[Integral Test]], $\sum_{n=1}^\infty 1/n^p$ converges iff $\int_1^\infty x^{-p}\, dx$ converges.

**Case $p \neq 1$:**
$$\int_1^\infty x^{-p}\, dx = \lim_{t\to\infty}\frac{x^{-p+1}}{-p+1}\Bigg|_1^t = \lim_{t\to\infty}\frac{t^{1-p} - 1}{1-p}$$

If $p > 1$: $1 - p < 0$, so $t^{1-p} \to 0$, giving $\dfrac{0 - 1}{1-p} = \dfrac{1}{p-1}$ — **converges**.

If $p < 1$: $1 - p > 0$, so $t^{1-p} \to \infty$ — **diverges**.

**Case $p = 1$ (harmonic series):**
$$\int_1^\infty \frac{1}{x}\, dx = \ln x\Big|_1^\infty = \infty \quad \text{— diverges}$$

**Summary:** $\sum 1/n^p$ converges $\iff$ $p > 1$, and when it converges, the value is $\zeta(p) = \sum_{n=1}^\infty n^{-p}$.

**Special values:** $\zeta(2) = \pi^2/6$, $\zeta(4) = \pi^4/90$, $\zeta(6) = \pi^6/945$.

## Worked Example
**Problem:** Does $\sum_{n=1}^\infty \dfrac{1}{n^{3/2}}$ converge?

**Step 1 — Identify p.** This is a p-series with $p = 3/2$.

**Step 2 — Check condition.** $p = 3/2 > 1$ → **converges**.

**Step 3 — Value (optional).** $\sum_{n=1}^\infty n^{-3/2} = \zeta(3/2) \approx 2.612$.

**Now test the boundary:**

Does $\sum_{n=1}^\infty \dfrac{1}{n}$ converge? $p = 1$: **diverges** ([[Harmonic Series]]).
Does $\sum_{n=1}^\infty \dfrac{1}{\sqrt{n}}$ converge? $p = 1/2 < 1$: **diverges**.

## See Also
- [[Integral Test]] — the proof technique for p-series convergence
- [[Harmonic Series]] — p-series with p=1; canonical divergent example
- [[Comparison Test]] — p-series is the benchmark for comparisons
- [[Convergence]] — p-series illustrates the p>1 threshold
- [[Divergence]] — p≤1 cases
- [[Taylor Series]] — zeta function values appear in Taylor series coefficients
