# Harmonic Series

**One-liner:** The series 1 + 1/2 + 1/3 + 1/4 + ... — the most important divergent series, which grows without bound despite its terms approaching zero.

## Core Idea
$$\sum_{n=1}^{\infty} \frac{1}{n} = 1 + \frac{1}{2} + \frac{1}{3} + \frac{1}{4} + \cdots = \infty$$
The harmonic series diverges. This is the canonical example proving that $a_n \to 0$ is necessary but not sufficient for convergence. Terms shrink to zero, yet the partial sums grow — just incredibly slowly (like $\ln N$ for the $N$-th partial sum).

## Why It Exists
The harmonic series is not just a curiosity — it's the boundary case that separates the convergent p-series ($p > 1$) from the divergent ones ($p \leq 1$). Understanding why it diverges deepens intuition for all convergence/divergence theory. It also appears naturally in physics (1/r potential fields), music (harmonic overtones), and number theory.

## Real-World Applications
- **Music / Acoustics:** The harmonic overtone series is $f, 2f, 3f, 4f, \ldots$ — the frequencies of a vibrating string. The amplitudes of these harmonics determine timbre. The series name "harmonic" comes directly from acoustics. An instrument's "harmonic content" is literally a partial sum of this series.
- **Computer Science:** The coupon collector's problem: expected number of draws to collect all $n$ coupons is $n \cdot H_n \approx n\ln n$ where $H_n = \sum_{k=1}^n 1/k$ is the harmonic number. Appears in hash table analysis and randomized algorithms.
- **Physics — 1/r² force laws:** Summing contributions from an infinite line of point charges or masses involves a harmonic-like series; divergence is handled by physical cutoffs (finite system size).
- **Paradox / Intuition pump:** "Jenga paradox" — stacking $n$ blocks on a table edge, where each block extends $1/(2k)$ further than the one below. Total overhang $= \frac{1}{2}H_n \to \infty$. Infinitely many blocks can overhang by any distance. A physical consequence of harmonic divergence!

## Intuition
**The grouping argument (due to Oresme, ~1350):**

Group terms in blocks where each block's sum exceeds $1/2$:
$$\underbrace{1}_{=1} + \underbrace{\frac{1}{2}}_{=1/2} + \underbrace{\frac{1}{3} + \frac{1}{4}}_{>1/2} + \underbrace{\frac{1}{5}+\frac{1}{6}+\frac{1}{7}+\frac{1}{8}}_{>1/2} + \cdots$$

Each group of $2^k$ terms (for the $k$-th block) sums to more than $1/2$: there are infinitely many such blocks, each contributing at least $1/2$. So the total exceeds $1/2 \cdot \infty = \infty$.

The key insight: the terms shrink, but not fast enough. The "speed of shrinking" matters — $1/n^2$ shrinks fast enough (converges), $1/n$ does not.

## Derivation
**Formal divergence proof (Oresme's grouping):**

Group the terms:
$$S = 1 + \frac{1}{2} + \left(\frac{1}{3}+\frac{1}{4}\right) + \left(\frac{1}{5}+\frac{1}{6}+\frac{1}{7}+\frac{1}{8}\right) + \cdots$$

The $k$-th group (for $k \geq 1$) contains terms $1/n$ for $2^{k-1} < n \leq 2^k$. This group has $2^{k-1}$ terms, each $\geq 1/2^k$. So the group sum:
$$\sum_{n=2^{k-1}+1}^{2^k} \frac{1}{n} \geq 2^{k-1} \cdot \frac{1}{2^k} = \frac{1}{2}$$

Since there are infinitely many groups each contributing at least $1/2$:
$$\sum_{n=1}^\infty \frac{1}{n} = \infty \quad \square$$

**Growth rate of partial sums:** More precisely, it can be shown that:
$$H_N = \sum_{n=1}^N \frac{1}{n} = \ln N + \gamma + O(1/N)$$
where $\gamma \approx 0.5772$ is the Euler–Mascheroni constant. The series diverges, but only logarithmically — extremely slowly. To exceed 10, you need about $e^{10} \approx 22{,}000$ terms.

**Via Integral Test:** Since $f(x) = 1/x$ is positive, continuous, and decreasing:
$$\sum_{n=1}^\infty \frac{1}{n} \geq \int_1^\infty \frac{1}{x}\, dx = \ln x\Big|_1^\infty = \infty$$

## Worked Example
**Problem:** How many terms of the harmonic series does it take for $H_N > 5$?

**Using the approximation** $H_N \approx \ln N + 0.5772$:
$$\ln N + 0.5772 > 5 \implies \ln N > 4.4228 \implies N > e^{4.4228} \approx 83$$

**Check:** $H_{83} \approx \ln(83) + 0.5772 \approx 4.419 + 0.577 = 4.996 \approx 5$. ✓

To exceed 100: $N > e^{99.42} \approx 10^{43}$ terms. The divergence is real but extraordinarily slow.

## See Also
- [[p-Series]] — harmonic series is p-series with p=1; the boundary case
- [[Divergence]] — harmonic series is the key example that a_n → 0 is insufficient
- [[Divergence Test]] — the test that *doesn't* catch harmonic series divergence
- [[Integral Test]] — proves harmonic divergence via ∫1/x dx = ln x → ∞
- [[Comparison Test]] — can show harmonic divergence by comparison to grouped blocks
- [[Series]] — context for what "diverges" means
