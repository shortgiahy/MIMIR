# Partial Sum

**One-liner:** The sum of the first N terms of a series — the finite approximation whose limit defines whether an infinite series converges.

## Core Idea
$$S_N = \sum_{n=1}^{N} a_n = a_1 + a_2 + \cdots + a_N$$
The partial sum $S_N$ is just the ordinary finite sum of the first N terms. The key insight: an infinite series $\sum_{n=1}^\infty a_n$ is defined to equal $\lim_{N\to\infty} S_N$. The partial sums form their own sequence $\{S_N\}$; if this sequence converges, the series converges. If $\{S_N\}$ diverges, so does the series.

## Why It Exists
You can't add infinitely many numbers directly — that operation isn't defined by ordinary arithmetic. Partial sums provide the bridge: replace "infinite sum" with "limit of a sequence of finite sums," which is a well-defined limit problem. This is the foundational trick that makes infinite series mathematically rigorous, not just a convenient fiction.

## Real-World Applications
- **Numerical computation:** Every software library that evaluates $e^x$, $\sin(x)$, or $\cos(x)$ computes a partial sum of the corresponding Maclaurin series (see [[Maclaurin Series]]). The software chooses N large enough for the desired precision.
- **Error estimation:** The [[Alternating Series Test]] gives an explicit bound: the error from stopping at $S_N$ is at most $|a_{N+1}|$. This is how you know how many terms your calculator needs.
- **Digital filters:** FIR (finite impulse response) filters in audio/comms are literally truncated convolution sums — partial sums of an infinite impulse response, kept finite for real-time computation.
- **Monte Carlo / ML:** Running averages in stochastic gradient descent accumulate partial sums of gradient estimates. See [[Gradient Descent]].
- **Physics simulations:** Multipole expansions truncate at some order — a partial sum of the full series. Used in N-body simulations for astrophysics and molecular dynamics.

## Intuition
Imagine filling a bucket. Each term $a_n$ is a splash of water. The partial sum $S_N$ is the total water after N splashes. The question "does the series converge?" translates to: "does the water level stabilize as you keep splashing?" If the splashes get smaller fast enough, the bucket fills to a finite level and stops overflowing. If the splashes don't shrink fast enough (like the [[Harmonic Series]]), the bucket overflows to infinity.

## Derivation
**Definition:** For a series $\sum_{n=1}^\infty a_n$:
$$S_N = \sum_{n=1}^N a_n$$

**Series convergence via partial sums:**
$$\sum_{n=1}^\infty a_n = S \iff \lim_{N\to\infty} S_N = S$$

**Telescoping partial sums:** When $a_n = f(n) - f(n+1)$:
$$S_N = f(1) - f(N+1)$$
because all intermediate terms cancel. This is why telescoping series are among the few for which we can find a closed-form $S_N$.

**Geometric series partial sum** (a rare explicit formula): For $a_n = ar^{n-1}$:
$$S_N = a\,\frac{1 - r^N}{1 - r}, \quad r \neq 1$$
Taking $N \to \infty$ with $|r| < 1$ gives $S = a/(1-r)$. See [[Geometric Series]].

**Relation to convergence tests:** Most convergence tests (Ratio, Integral, Comparison) don't compute $S_N$ — they only determine whether $\{S_N\}$ converges without finding the limit. Partial sums are usually only tractable for geometric, telescoping, or specially structured series.

## Worked Example
**Problem:** Find the partial sum $S_N$ for the geometric series $\sum_{n=0}^\infty \left(\frac{1}{2}\right)^n$ and verify convergence.

**Step 1 — Write $S_N$.**
$$S_N = 1 + \frac{1}{2} + \frac{1}{4} + \cdots + \frac{1}{2^N} = \sum_{n=0}^N \left(\frac{1}{2}\right)^n$$

**Step 2 — Use the geometric partial sum formula** ($a = 1$, $r = 1/2$):
$$S_N = \frac{1 - (1/2)^{N+1}}{1 - 1/2} = 2\left(1 - \frac{1}{2^{N+1}}\right) = 2 - \frac{1}{2^N}$$

**Step 3 — Verify:** $S_1 = 1 + 1/2 = 3/2 = 2 - 1/2$. ✓ $S_2 = 1 + 1/2 + 1/4 = 7/4 = 2 - 1/4$. ✓

**Step 4 — Take the limit:**
$$\lim_{N\to\infty} S_N = \lim_{N\to\infty}\left(2 - \frac{1}{2^N}\right) = 2 - 0 = 2$$

The series converges to 2.

## See Also
- [[Sequence]] — partial sums form a sequence; convergence is a sequence limit
- [[Series]] — a series is defined by its partial sums
- [[Convergence]] — the formal definition of series convergence
- [[Geometric Series]] — one of the few series with a closed-form partial sum formula
- [[Alternating Series Test]] — uses partial sums to bound approximation error
- [[Maclaurin Series]] — evaluated numerically via partial sums
