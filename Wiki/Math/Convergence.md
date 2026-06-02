# Convergence

**One-liner:** The property of a sequence or series that approaches a definite, finite value — the mathematical formalization of "settling down."

## Core Idea
$$\sum_{n=1}^{\infty} a_n \text{ converges} \iff \lim_{N\to\infty} S_N = S \in \mathbb{R}$$
A series converges if the sequence of its [[Partial Sum|partial sums]] has a finite limit. More precisely: $\sum a_n$ converges to $S$ if for every $\varepsilon > 0$ there exists $N$ such that for all $M > N$, $|S_M - S| < \varepsilon$. For sequences: $\{a_n\}$ converges to $L$ if the terms eventually stay within any prescribed tolerance of $L$.

## Why It Exists
Mathematics needs precision about what "infinite sums" mean. Without a rigorous convergence theory, manipulations like rearranging series terms can give contradictory results (as Riemann proved — you can rearrange a conditionally convergent series to sum to any value). Convergence theory prevents these pathologies and gives us reliable tools: the convergence tests.

## Real-World Applications
- **Algorithm stability:** Iterative algorithms (Newton's method, gradient descent) converge when the error sequence $|x_n - x^*|$ satisfies the definition — each step brings you reliably closer. A diverging optimization is useless. See [[Gradient Descent]].
- **Electrical circuits:** In AC circuit analysis, certain Fourier series representations of waveforms converge to the waveform's actual shape. Convergence rate determines how many harmonics you need for a faithful reconstruction.
- **Numerical methods:** Convergent numerical integration methods (trapezoidal rule, Simpson's rule) produce sequences of estimates that converge to the true integral as the step size shrinks.
- **Quantum field theory:** Perturbation series in QFT technically diverge in the formal sense but are asymptotically convergent — a subtler concept where partial sums get very accurate before eventually blowing up.
- **Control theory:** A stable feedback control system has a state trajectory that converges to the desired setpoint. Stability = convergence.

## Intuition
Convergence is like a rumor getting more accurate over time. Each partial sum is a new version of the rumor. If the rumor eventually stabilizes — if every subsequent version is indistinguishable from the truth within any tolerance you name — then it has converged.

**Rate of convergence** matters practically: a series that converges quickly (like $\sum 1/n!$) is useful for computation; one that converges slowly (like $\sum 1/n^{1.001}$) may require billions of terms for decent accuracy.

## Derivation
**Cauchy criterion for series convergence:**

A series $\sum a_n$ converges if and only if for every $\varepsilon > 0$ there exists $N$ such that for all $m > n > N$:
$$\left|\sum_{k=n+1}^{m} a_k\right| < \varepsilon$$

This is useful because it doesn't require knowing the limit $S$ in advance.

**Necessary condition (NOT sufficient):**

If $\sum a_n$ converges, then $\lim_{n\to\infty} a_n = 0$.

**Proof:** If $\sum a_n = S$, then $S_N \to S$ and $S_{N-1} \to S$. Since $a_N = S_N - S_{N-1}$, we get $a_N \to S - S = 0$. □

This gives the [[Divergence Test]]: if $a_n \not\to 0$, the series diverges. But the converse is false — see [[Harmonic Series]].

**Absolute vs. conditional convergence:**
- $\sum a_n$ **absolutely converges** if $\sum |a_n|$ converges.
- $\sum a_n$ **conditionally converges** if $\sum a_n$ converges but $\sum |a_n|$ diverges.
- Absolute convergence implies convergence. Conditional convergence is more fragile.

## Worked Example
**Problem:** Does $\sum_{n=1}^\infty \dfrac{(-1)^n}{n^2}$ converge? Absolutely or conditionally?

**Step 1 — Check absolute convergence.** Consider $\sum \dfrac{1}{n^2}$. This is a p-series with $p = 2 > 1$ → converges. See [[p-Series]].

**Step 2 — Conclude.** Since $\sum |a_n| = \sum \dfrac{1}{n^2}$ converges, the original series converges **absolutely**.

**Bonus:** $\sum_{n=1}^\infty \dfrac{(-1)^n}{n^2} = -\dfrac{\pi^2}{12}$ (this follows from the Basel problem result).

## See Also
- [[Divergence]] — the opposite; what happens when series don't converge
- [[Partial Sum]] — the mechanism: convergence is a limit of partial sums
- [[Divergence Test]] — necessary condition; first test to apply
- [[Ratio Test]] — powerful test for absolute convergence
- [[Integral Test]] — ties series convergence to improper integrals
- [[Comparison Test]] — prove convergence by comparison to known series
- [[Alternating Series Test]] — convergence criterion for alternating series
- [[p-Series]] — canonical family of convergent/divergent series
- [[Geometric Series]] — canonical convergent series
- [[Value Function]] — value iteration in RL is a convergent sequence of Bellman updates; mathematical convergence guarantees it reaches the optimal value function
