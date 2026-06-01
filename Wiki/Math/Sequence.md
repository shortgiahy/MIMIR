# Sequence

**One-liner:** An ordered, indexed list of numbers defined by a rule, which may approach a fixed value (converge) or grow without bound (diverge).

## Core Idea
$$a_n = f(n), \quad n = 1, 2, 3, \ldots$$
A sequence is a function from the positive integers (or natural numbers) to the reals. We write it as $\{a_n\}_{n=1}^\infty$ or just $\{a_n\}$. The sequence converges to limit $L$ if:
$$\lim_{n \to \infty} a_n = L$$
meaning: for every $\varepsilon > 0$, there exists $N$ such that for all $n > N$, $|a_n - L| < \varepsilon$.

## Why It Exists
Sequences are the discrete foundation of analysis. Before you can talk about infinite sums (series), limits of functions, or continuity, you need a rigorous way to describe "what a list of numbers approaches." They formalize the intuition of "getting closer and closer to a value" — the bedrock of calculus.

## Real-World Applications
- **Algorithms / CS:** Iteration sequences — Newton's method for root-finding, gradient descent updates in ML, convergent iterative solvers. The sequence of approximations $\{x_n\}$ must converge for the algorithm to work. See [[Gradient Descent]].
- **Digital Signal Processing:** Discrete-time signals are sequences. Sampling audio gives you a sequence $\{x[n]\}$; the entire theory of digital filters operates on sequences.
- **Numerical Analysis:** Approximating π: $3, 3.1, 3.14, 3.141, \ldots$ is a convergent sequence. Every numerical computation on a computer uses convergent sequences.
- **Finance:** Compound interest generates a sequence of account balances converging (in present-value terms) under certain conditions. See [[Geometric Series]].
- **Control Systems:** Discrete-time control outputs form sequences; stability requires convergence. See [[Convergence]].

## Intuition
A sequence is like a GPS trace — a list of positions indexed by time steps. "Convergent" means the GPS trace homes in on a fixed destination. "Divergent" means the vehicle drives away forever (or oscillates without settling).

The $\varepsilon$-$N$ definition is the rigorous version of: "no matter how tight a circle you draw around L, eventually all remaining terms fall inside it."

## Derivation
**Formal convergence definition:**

$\lim_{n\to\infty} a_n = L$ iff:
$$\forall \varepsilon > 0,\; \exists N \in \mathbb{N} \text{ such that } n > N \implies |a_n - L| < \varepsilon$$

**Key theorems:**

1. **Squeeze Theorem for Sequences:** If $b_n \leq a_n \leq c_n$ for all large $n$, and $\lim b_n = \lim c_n = L$, then $\lim a_n = L$.

2. **Continuity theorem:** If $\lim_{n\to\infty} a_n = L$ and $f$ is continuous at $L$, then $\lim_{n\to\infty} f(a_n) = f(L)$.

3. **Monotone Convergence Theorem:** A sequence that is monotone (always increasing or always decreasing) and bounded must converge. This is an existence result — we know a limit exists without computing it.

**Relationship between sequences and functions:** If $\lim_{x\to\infty} f(x) = L$ (continuous variable), then the sequence $\{f(n)\}$ also converges to $L$. This bridge lets us use L'Hôpital's rule on sequences.

## Worked Example
**Problem:** Does the sequence $a_n = \dfrac{n^2 + 3}{2n^2 - 1}$ converge? If so, find the limit.

**Step 1 — Divide numerator and denominator by $n^2$.**
$$a_n = \frac{1 + 3/n^2}{2 - 1/n^2}$$

**Step 2 — Take the limit.** As $n \to \infty$, $3/n^2 \to 0$ and $1/n^2 \to 0$:
$$\lim_{n\to\infty} a_n = \frac{1 + 0}{2 - 0} = \frac{1}{2}$$

**Conclusion:** The sequence converges to $1/2$.

**Intuition check:** The highest-degree terms dominate, so the ratio of leading coefficients ($1/2$) is the limit — same logic as rational function limits.

## See Also
- [[Series]] — a sequence of partial sums; what you build from a sequence
- [[Partial Sum]] — the mechanism connecting sequences to series
- [[Convergence]] — the formal theory of limits for sequences and series
- [[Divergence]] — what happens when a sequence doesn't converge
- [[Geometric Series]] — the canonical convergent sequence example
