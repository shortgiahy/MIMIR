# Ratio Test

**One-liner:** A convergence test that compares consecutive terms by their ratio — the go-to test for series involving factorials and exponentials.

## Core Idea
$$L = \lim_{n\to\infty} \left|\frac{a_{n+1}}{a_n}\right|, \quad \begin{cases} L < 1 & \Rightarrow \text{series converges absolutely} \\ L > 1 & \Rightarrow \text{series diverges} \\ L = 1 & \Rightarrow \text{inconclusive} \end{cases}$$
The Ratio Test examines whether consecutive terms shrink (L < 1), grow (L > 1), or stay roughly the same (L = 1). It's the workhorse test for series with $n!$, $n^n$, or $c^n$ because factorials and exponentials have predictable ratios.

## Why It Exists
Some series have terms that are difficult to compare directly to p-series or to integrate. But the ratio of consecutive terms is often easy to compute — especially for factorials ($(n+1)!/n! = n+1$) and exponentials ($c^{n+1}/c^n = c$). The Ratio Test exploits this by asking: "locally, does this series look like a geometric series?" A geometric series with ratio L < 1 converges; the test leverages this.

## Real-World Applications
- **Power series and radius of convergence:** The [[Radius of Convergence]] is found via the Ratio Test. Every time you work with a Taylor or power series, the Ratio Test determines where it converges.
- **Probability distributions:** The Poisson distribution involves $e^{-\lambda} \lambda^n / n!$; verifying it sums to 1 uses the Ratio Test (or the Maclaurin series for $e^\lambda$).
- **Algorithm analysis:** Some recurrence-based analysis of algorithms leads to series with factorial or exponential terms — convergence checked via Ratio Test.
- **Physics — perturbation theory:** Verifying that perturbative expansions in quantum mechanics converge (or at least have computable radius of convergence) uses the Ratio Test.

## Intuition
The Ratio Test says: "for large n, does this series locally behave like a geometric series?" If the ratio $|a_{n+1}/a_n| \to L$, then for large n, $|a_{n+1}| \approx L \cdot |a_n|$ — roughly geometric. A geometric series with $|r| = L < 1$ converges; with $|r| = L > 1$, it diverges.

The $L = 1$ failure mode: p-series all have $L = 1$ (since $\frac{1/(n+1)^p}{1/n^p} = \left(\frac{n}{n+1}\right)^p \to 1$), yet p-series can converge or diverge depending on $p$. So the Ratio Test cannot distinguish them.

**When to use:** Series containing $n!$, $n^n$, $c^n$ (constants raised to $n$), or products thereof. Almost never useful for pure rational functions or p-series (use Integral Test or Comparison Test instead).

## Derivation
**Theorem (Ratio Test):** Let $\sum a_n$ be a series with positive terms, and let $L = \lim_{n\to\infty} a_{n+1}/a_n$.

**Case $L < 1$:** Choose $r$ with $L < r < 1$. There exists $N$ such that for all $n > N$: $a_{n+1}/a_n < r$, meaning $a_{n+1} < r\, a_n$.

By induction: for $n > N$, $a_n < a_N \cdot r^{n-N} = C \cdot r^n$ where $C = a_N / r^N$.

Since $\sum C r^n$ is a convergent geometric series ($r < 1$), by the [[Comparison Test]], $\sum a_n$ converges. □

**Case $L > 1$:** There exists $N$ such that for $n > N$: $a_{n+1}/a_n > 1$, so $\{a_n\}$ is eventually increasing. Thus $a_n \not\to 0$, so $\sum a_n$ diverges by the [[Divergence Test]]. □

**Absolute convergence:** For series with mixed signs, apply the Ratio Test to $|a_n|$. If $L < 1$, then $\sum |a_n|$ converges, so $\sum a_n$ converges absolutely.

## Worked Example
**Problem:** Does $\sum_{n=0}^\infty \dfrac{3^n}{n!}$ converge?

**Step 1 — Set up the ratio.**
$$\frac{a_{n+1}}{a_n} = \frac{3^{n+1}/(n+1)!}{3^n/n!} = \frac{3^{n+1}}{3^n} \cdot \frac{n!}{(n+1)!} = 3 \cdot \frac{1}{n+1} = \frac{3}{n+1}$$

**Step 2 — Take the limit.**
$$L = \lim_{n\to\infty} \frac{3}{n+1} = 0 < 1$$

**Step 3 — Conclude.** $L = 0 < 1$ → the series **converges absolutely**.

**Bonus:** We recognize this as the Maclaurin series for $e^3$: $\sum_{n=0}^\infty 3^n/n! = e^3 \approx 20.09$.

## See Also
- [[Comparison Test]] — used in the proof of the Ratio Test
- [[Divergence Test]] — used to prove the L > 1 case
- [[Geometric Series]] — the series the Ratio Test locally approximates
- [[Radius of Convergence]] — found via the Ratio Test for power series
- [[Integral Test]] — better for p-series and rational terms where Ratio Test gives L=1
- [[Alternating Series Test]] — for alternating series where Ratio Test is inconclusive
- [[Maclaurin Series]] — the e^x series example above
- [[Learning Rate]] — the learning rate in gradient descent plays the same role as the ratio r: if it exceeds the convergence threshold (analogous to |r| ≥ 1), training diverges
- [[Power Series]] — ratio test is the standard method for finding the interval of convergence of a power series
