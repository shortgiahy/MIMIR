# Alternating Series Test

**One-liner:** An alternating series Σ(−1)ⁿaₙ converges if its terms decrease monotonically to zero — and the error from stopping early is no worse than the first omitted term.

## Core Idea
**Leibniz Criterion:** If $\{a_n\}$ satisfies:
1. $a_n > 0$ (positive terms)
2. $a_n \ge a_{n+1}$ (decreasing)
3. $\lim_{n\to\infty} a_n = 0$

Then $\displaystyle\sum_{n=1}^{\infty} (-1)^{n+1} a_n = a_1 - a_2 + a_3 - a_4 + \cdots$ converges.

**Error bound:** If $S$ is the true sum and $S_N$ is the Nth partial sum:
$$|S - S_N| \le a_{N+1}$$
The error is bounded by the absolute value of the first omitted term, and the partial sums oscillate around $S$, alternately overshooting and undershooting.

## Why It Exists
Alternating signs introduce cancellation that can rescue a series whose terms don't decay fast enough to converge absolutely. The classic example: $\sum (-1)^{n+1}/n$ (the alternating harmonic series) converges to ln 2, even though the underlying harmonic series Σ1/n diverges. The test captures exactly when sign alternation is sufficient to force convergence, and the error bound makes it practical for computation.

## Real-World Applications
- **Alternating harmonic series and ln 2:** The series $1 - \frac{1}{2} + \frac{1}{3} - \frac{1}{4} + \cdots = \ln 2$ is the Maclaurin series for ln(1+x) at x=1. The error bound tells you how many terms to compute for N decimal places of ln 2.
- **Computing π:** The Leibniz formula $\frac{\pi}{4} = 1 - \frac{1}{3} + \frac{1}{5} - \cdots$ is an alternating series, convergence guaranteed by the AST. (It converges too slowly to be practical, but numerically better alternating series are used in computing π.)
- **Signal processing — alternating Fourier coefficients:** Certain signals have alternating Fourier series whose convergence is verified by AST. The error bound quantifies truncation error directly.
- **Numerical methods — alternating Taylor remainders:** When a Taylor series for a function has alternating signs (like cos x, the series for e^{-x}), the AST gives an immediate error bound for polynomial approximations — useful in embedded systems computing trig functions.
- **Conditional convergence in physics:** Some perturbation series and path integrals are conditionally convergent alternating series. Understanding that they converge but not absolutely flags their fragility under rearrangement.

## Intuition
Picture the partial sums hopping along a number line, each step alternating direction and shrinking:

$S_1 = a_1$ (jump right by $a_1$)
$S_2 = a_1 - a_2$ (jump left by $a_2 < a_1$ — don't cross zero)
$S_3 = a_1 - a_2 + a_3$ (jump right by $a_3 < a_2$)
...

The odd partial sums form a decreasing sequence bounded below. The even partial sums form an increasing sequence bounded above. Both are monotone and bounded → both converge. The gap between them is $a_{N+1}$ (the next term), which shrinks to zero. So they converge to the same limit $S$, and $S$ is sandwiched between any consecutive pair of partial sums.

This is why the error is at most $a_{N+1}$: the true sum lies *between* $S_N$ and $S_{N+1}$, so the distance from $S_N$ to $S$ is at most $|S_{N+1} - S_N| = a_{N+1}$.

## Derivation
**Theorem:** Under the three conditions above, $\sum_{n=1}^\infty (-1)^{n+1} a_n$ converges.

**Step 1 — Analyze even partial sums.**
$$S_{2m} = (a_1 - a_2) + (a_3 - a_4) + \cdots + (a_{2m-1} - a_{2m})$$

Each group $(a_{2k-1} - a_{2k}) \ge 0$ since the sequence is decreasing. So $\{S_{2m}\}$ is increasing.

**Step 2 — Bound the even partial sums.**
$$S_{2m} = a_1 - (a_2 - a_3) - (a_4 - a_5) - \cdots - (a_{2m-2} - a_{2m-1}) - a_{2m} \le a_1$$

Each grouped difference is non-negative, so $S_{2m} \le a_1$ for all m.

**Step 3 — Even partial sums converge.** $\{S_{2m}\}$ is increasing and bounded above by $a_1$. By the Monotone Convergence Theorem, $S_{2m} \to S$ for some $S \le a_1$.

**Step 4 — Odd partial sums converge to the same limit.**
$$S_{2m+1} = S_{2m} + a_{2m+1}$$

Since $a_{2m+1} \to 0$: $S_{2m+1} \to S + 0 = S$.

**Step 5 — All partial sums converge to S.** Every partial sum is either even-indexed or odd-indexed, and both subsequences converge to $S$. Therefore $S_N \to S$. □

**Error bound:** For any N, note that $S$ lies between $S_N$ and $S_{N+1}$ (since the sequence of partial sums oscillates around $S$ with decreasing amplitude). Therefore:
$$|S - S_N| \le |S_{N+1} - S_N| = a_{N+1} \qquad \square$$

## Worked Example
**Problem:** Show $\displaystyle\sum_{n=1}^{\infty} \frac{(-1)^{n+1}}{n} = 1 - \frac{1}{2} + \frac{1}{3} - \cdots$ converges. Then find N such that the partial sum approximates the true value within 0.01.

**Step 1 — Verify conditions.** Let $a_n = 1/n$.
1. $a_n = 1/n > 0$ ✓
2. $a_n = 1/n > 1/(n+1) = a_{n+1}$ ✓ (decreasing)
3. $\lim_{n\to\infty} 1/n = 0$ ✓

By the Alternating Series Test, the series **converges**.

**Step 2 — Find N for error ≤ 0.01.** We need $a_{N+1} \le 0.01$:
$$\frac{1}{N+1} \le 0.01 \implies N+1 \ge 100 \implies N \ge 99$$

So $S_{99}$ is within 0.01 of the true sum. The true sum is $\ln 2 \approx 0.6931$ (from the [[Maclaurin Series]] of $\ln(1+x)$ at $x=1$).

**Step 3 — Note the convergence type.** This series converges **conditionally** — the underlying $\sum 1/n$ ([[Harmonic Series]]) diverges, but the alternating signs rescue it.

## See Also
- [[Convergence]] — specifically conditional vs. absolute convergence
- [[Harmonic Series]] — the alternating version conditionally converges
- [[Maclaurin Series]] — many Maclaurin series (ln(1+x), arctan x) are alternating; AST proves convergence at endpoints
- [[Ratio Test]] — for absolute convergence; use AST when Ratio Test gives L=1
- [[Comparison Test]] — applies to absolute values; combined with AST distinguishes absolute vs. conditional convergence
- [[Taylor Series]] — alternating remainder terms; AST error bound applies to Taylor approximations
- [[p-Series]] — $\sum (-1)^n/n^p$ converges by AST for any p > 0, even p ≤ 1 where the non-alternating version diverges
