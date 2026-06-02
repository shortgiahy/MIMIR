# Divergence Test

**One-liner:** If the terms of a series don't approach zero, the series definitely diverges — but the test is silent when terms do approach zero.

## Core Idea
$$\text{If } \lim_{n\to\infty} a_n \neq 0 \text{ (or the limit DNE), then } \sum_{n=1}^{\infty} a_n \text{ diverges.}$$
**The critical caveat:** If $\lim_{n\to\infty} a_n = 0$, the test is inconclusive. It says nothing — the series might converge or diverge. The [[Harmonic Series]] ($a_n = 1/n \to 0$) diverges; the p-series ($a_n = 1/n^2 \to 0$) converges. You need other tests to decide these cases.

## Why It Exists
The Divergence Test is the first filter — a quick sanity check before applying any other convergence test. It's the cheapest test to apply (just take a limit of a single sequence) and it immediately eliminates many series from consideration. It exists because a necessary condition for convergence is so useful as a preliminary screening tool.

## Real-World Applications
- **Algorithm monitoring:** In iterative ML algorithms, if the loss values $\{L_n\}$ don't approach zero (or some target), the algorithm is diverging — stop it and adjust hyperparameters. The Divergence Test is literally the stopping criterion check. See [[Gradient Descent]].
- **Engineering verification:** Quick check for divergent simulations or numerical methods — if residuals aren't decreasing toward zero, the method is diverging.
- **Signal processing:** If a filter's output sequence $y[n]$ doesn't decay to zero for a zero-input system, the filter is unstable (diverging). Immediate flag.

## Intuition
Imagine trying to fill a bucket by pouring in splashes of water. If each successive splash is at least a fixed size (say, always at least $0.1$ liters), the bucket overflows — infinite total water. The Divergence Test captures exactly this: if $a_n \not\to 0$, then no matter how many terms you add, each new one contributes a non-negligible amount.

**But** if the splashes do go to zero, that alone doesn't tell you if the bucket fills or overflows. The splashes could decrease slowly (like $1/n$) and still overflow, or decrease quickly (like $1/n^2$) and converge. You need a more careful test.

## Derivation
This is the contrapositive of: "convergence implies terms go to zero."

**Theorem:** If $\sum_{n=1}^\infty a_n$ converges to $S$, then $\lim_{n\to\infty} a_n = 0$.

**Proof:** Let $S_N = \sum_{n=1}^N a_n$. By assumption, $S_N \to S$.

Since $a_n = S_n - S_{n-1}$:
$$\lim_{n\to\infty} a_n = \lim_{n\to\infty} (S_n - S_{n-1}) = S - S = 0 \quad \square$$

**Contrapositive (Divergence Test):** $\lim_{n\to\infty} a_n \neq 0 \Rightarrow \sum a_n$ diverges.

**Why the converse fails:** $a_n = 1/n \to 0$, yet $\sum 1/n$ diverges. The convergence proof requires a limit that approaches zero fast enough; slow convergence to zero is insufficient. The Integral Test gives the precise threshold for power functions.

## Worked Example
**Problem:** Determine if $\sum_{n=1}^\infty \cos(1/n)$ converges or diverges.

**Step 1 — Apply the Divergence Test.**
$$\lim_{n\to\infty} \cos(1/n) = \cos\!\left(\lim_{n\to\infty} \frac{1}{n}\right) = \cos(0) = 1 \neq 0$$

**Step 2 — Conclude.** Since $a_n \to 1 \neq 0$, the series **diverges** by the Divergence Test.

**Interpretation:** We're forever adding roughly 1 to the sum. Of course it diverges.

**Counter-example (inconclusive case):** For $\sum 1/n$: $a_n = 1/n \to 0$. Test is inconclusive. Must use [[Integral Test]] or [[Comparison Test]] to determine divergence.

## See Also
- [[Divergence]] — general theory of divergent series
- [[Convergence]] — necessary condition proved here
- [[Harmonic Series]] — canonical example where Divergence Test is inconclusive
- [[Ratio Test]] — the next test to try; handles factorials and exponentials
- [[Integral Test]] — use when Divergence Test is inconclusive and a_n = f(n) for integrable f
- [[Comparison Test]] — use when you can bound a_n by a known series
