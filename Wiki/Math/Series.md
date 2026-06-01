# Series

**One-liner:** The sum of the terms of a sequence — an infinite addition that may yield a finite number, formalized through the concept of partial sums.

## Core Idea
$$\sum_{n=1}^{\infty} a_n = \lim_{N \to \infty} S_N = \lim_{N \to \infty} \sum_{n=1}^{N} a_n$$
A series is defined as the limit of its [[Partial Sum|partial sums]] $S_N$. If this limit exists and is finite, the series **converges** to that value. If the limit is infinite or doesn't exist, the series **diverges**. The conceptual leap: an infinite sum only makes sense as a limit — we never "actually" add infinitely many terms.

## Why It Exists
Calculus requires representing functions as infinite sums ([[Taylor Series]], [[Power Series]]). Physics needs to sum infinitely many small contributions (work along a path → Riemann sums → integrals, which are really series in disguise). Without a rigorous theory of infinite sums, you couldn't trust that $e = 1 + 1 + 1/2 + 1/6 + \cdots$ means anything. The series framework makes infinite addition rigorous and testable.

## Real-World Applications
- **Calculator chip design:** Your calculator evaluates $\sin(0.5)$ by summing finitely many terms of the Maclaurin series for sine — a truncated series. Same for $e^x$, $\ln(1+x)$, etc. See [[Maclaurin Series]].
- **JPEG / MP3 compression:** Fourier series decompose images and audio into sums of sinusoids. The compression algorithm keeps the dominant terms and discards the rest. The series converges to the original signal.
- **Quantum mechanics:** Perturbation theory represents quantum states and energies as power series in a small parameter. See [[Taylor Series]].
- **Numerical Integration (Quadrature):** Gaussian quadrature and Romberg integration are derived from truncated series approximations.
- **Probability:** Generating functions and moment generating functions are power series whose coefficients encode probability distributions. See [[Power Series]].
- **Finance:** The present value of a perpetuity (infinite stream of payments) is a convergent geometric series — it has a finite value! See [[Geometric Series]].

## Intuition
Zeno's paradox: to walk from A to B, you first walk half the distance, then half the remaining, then half again… infinitely many steps. Yet you arrive in finite time. The total distance is the geometric series $1/2 + 1/4 + 1/8 + \cdots = 1$. An infinite process can have a finite result — that's the core insight of series.

The key question for any series is: do the partial sums "home in" on a fixed number, or do they keep growing without bound?

## Derivation
**Formal definition:**

Given a sequence $\{a_n\}$, form the sequence of partial sums:
$$S_1 = a_1, \quad S_2 = a_1 + a_2, \quad S_N = \sum_{n=1}^N a_n$$

The series $\sum_{n=1}^\infty a_n$ is said to **converge** to $S$ if:
$$\lim_{N\to\infty} S_N = S$$

Otherwise the series **diverges**.

**Key properties (linearity):**
If $\sum a_n = A$ and $\sum b_n = B$, then:
- $\sum (a_n + b_n) = A + B$
- $\sum c\, a_n = c\, A$ for any constant $c$

**Index shifting:** Changing the starting index doesn't affect convergence, only the value. $\sum_{n=1}^\infty a_n = a_1 + \sum_{n=2}^\infty a_n$.

**Telescoping series:** If $a_n = b_n - b_{n+1}$, then $S_N = b_1 - b_{N+1}$, and the series converges iff $b_n \to L$, giving $\sum a_n = b_1 - L$.

## Worked Example
**Problem:** Evaluate $\displaystyle\sum_{n=1}^{\infty} \frac{1}{n(n+1)}$

**Step 1 — Partial fractions.** $\dfrac{1}{n(n+1)} = \dfrac{1}{n} - \dfrac{1}{n+1}$

**Step 2 — Partial sum.** This is a telescoping series:
$$S_N = \sum_{n=1}^N \left(\frac{1}{n} - \frac{1}{n+1}\right) = 1 - \frac{1}{N+1}$$

Most terms cancel: $S_N = (1 - 1/2) + (1/2 - 1/3) + \cdots + (1/N - 1/(N+1)) = 1 - 1/(N+1)$

**Step 3 — Take the limit.**
$$\sum_{n=1}^{\infty} \frac{1}{n(n+1)} = \lim_{N\to\infty} \left(1 - \frac{1}{N+1}\right) = 1$$

**The series converges to exactly 1.**

## See Also
- [[Sequence]] — a series is built from a sequence; understand sequences first
- [[Partial Sum]] — the mechanism that defines series convergence
- [[Convergence]] — the formal theory; when does a series converge?
- [[Divergence]] — when and why series fail to converge
- [[Geometric Series]] — the most important convergent series
- [[Harmonic Series]] — the most important divergent series
- [[Taylor Series]] — representing functions as power series
- [[Power Series]] — generalizing series to functions of x
