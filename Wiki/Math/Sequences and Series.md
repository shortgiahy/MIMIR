# Sequences and Series

**One-liner:** A sequence is an ordered list of numbers following a rule; a series is what you get when you add the terms of that list together — and infinite series can converge to a finite number, which is one of the most surprising facts in mathematics.

## Why It Exists

Before calculus, mathematicians struggled with questions like: what does it mean to add infinitely many numbers? Is $1 + \frac{1}{2} + \frac{1}{4} + \frac{1}{8} + \cdots$ equal to $2$, or is it just a number that "approaches" $2$ without ever arriving? The answer to that depends on having a rigorous definition of convergence, and that definition is built on sequences.

Series exist because approximation is central to science. Functions like $\sin x$ or $e^x$ don't have finite polynomial forms, but they can be represented as infinite sums of polynomial terms (see [[Taylor and Maclaurin Series]]). Digital signal processors, numerical integration, and virtually every simulation algorithm depend on computing partial sums of series — truncating after enough terms to get the precision you need. In Electrical Engineering, Fourier series decompose periodic signals into infinite sums of sinusoids; understanding convergence is what tells you when that decomposition is actually valid.

## The Concept

### Sequences

A **sequence** is a function $f: \mathbb{N} \to \mathbb{R}$ — it assigns a real number to each positive integer. We write the output as $a_n$ rather than $f(n)$, and list it as:

$$a_1, a_2, a_3, a_4, \ldots$$

Examples:
- $a_n = \frac{1}{n}$: the sequence $1, \frac{1}{2}, \frac{1}{3}, \frac{1}{4}, \ldots$
- $a_n = (-1)^n$: the sequence $-1, 1, -1, 1, \ldots$
- $a_n = \frac{n}{n+1}$: the sequence $\frac{1}{2}, \frac{2}{3}, \frac{3}{4}, \ldots$

### Convergence of a Sequence

A sequence $\{a_n\}$ **converges** to a limit $L$ if, as $n$ grows without bound, $a_n$ gets arbitrarily close to $L$. Formally:

$$\lim_{n \to \infty} a_n = L$$

This means: for any small $\varepsilon > 0$, there exists some index $N$ such that for all $n > N$, $|a_n - L| < \varepsilon$. The $\varepsilon$-$N$ definition is the mathematical way of saying "the terms eventually get permanently close to $L$."

A sequence that does not converge to any finite limit is called **divergent**. This happens either because the terms grow without bound ($a_n \to \pm\infty$), or because they oscillate and never settle (like $(-1)^n$).

Key examples:
- $\lim_{n\to\infty} \frac{1}{n} = 0$ — converges
- $\lim_{n\to\infty} n^2 = \infty$ — diverges
- $\lim_{n\to\infty} (-1)^n$ — diverges (oscillates)
- $\lim_{n\to\infty} \frac{n}{n+1} = 1$ — converges (divide numerator and denominator by $n$)

**Useful technique:** For rational sequences (ratios of polynomials), divide every term by the highest power of $n$ in the denominator. All terms with $1/n^k$ vanish to zero.

### Series: Adding Up the Sequence

A **series** is the sum of all terms of a sequence:

$$\sum_{n=1}^{\infty} a_n = a_1 + a_2 + a_3 + \cdots$$

But what does this *mean*? You can't literally add infinitely many numbers. Instead, we define the sum through **partial sums**:

$$S_N = \sum_{n=1}^{N} a_n = a_1 + a_2 + \cdots + a_N$$

The partial sum $S_N$ uses only finitely many terms — you can compute it exactly. Now ask: what happens to $S_N$ as $N \to \infty$? If the sequence $\{S_N\}$ converges to some limit $S$, we say the series **converges** and its sum is $S$:

$$\sum_{n=1}^{\infty} a_n = S \quad \Longleftrightarrow \quad \lim_{N \to \infty} S_N = S$$

If $\{S_N\}$ diverges, the series diverges — it has no finite sum.

### Why Infinite Sums Can Be Finite

This is the key conceptual hurdle. Intuitively, you might think "adding more terms always makes the sum bigger, so adding infinitely many terms makes the sum infinite." But that's wrong — it misses the idea that the *terms themselves* must shrink.

Consider the **geometric series**: $1 + r + r^2 + r^3 + \cdots$ where $|r| < 1$. Each new term is smaller than the previous by a factor of $r$. The partial sum formula is:

$$S_N = \frac{1 - r^{N+1}}{1 - r}$$

Derivation: Let $S_N = 1 + r + r^2 + \cdots + r^N$. Multiply both sides by $r$:
$$r S_N = r + r^2 + r^3 + \cdots + r^{N+1}$$

Subtract: $S_N - r S_N = 1 - r^{N+1}$, so $S_N(1-r) = 1 - r^{N+1}$, giving the formula above.

As $N \to \infty$, if $|r| < 1$ then $r^{N+1} \to 0$, so:

$$\sum_{n=0}^{\infty} r^n = \frac{1}{1-r}, \quad |r| < 1$$

This is a finite number. The series converges because even though we add infinitely many terms, they shrink fast enough that the cumulative total is bounded.

The **Harmonic Series** shows the opposite: $\sum_{n=1}^{\infty} \frac{1}{n} = 1 + \frac{1}{2} + \frac{1}{3} + \frac{1}{4} + \cdots$ diverges, even though the terms go to zero. The terms shrink, but *too slowly*. This distinction — terms going to zero is necessary but not sufficient for convergence — is the core difficulty in series theory.

### Key Series to Know

**Geometric series** ($|r| < 1$):
$$\sum_{n=0}^{\infty} r^n = \frac{1}{1-r}$$

**$p$-series**:
$$\sum_{n=1}^{\infty} \frac{1}{n^p} = \begin{cases} \text{converges} & \text{if } p > 1 \\ \text{diverges} & \text{if } p \leq 1 \end{cases}$$

The harmonic series is the $p$-series with $p=1$; it diverges. The series $\sum \frac{1}{n^2}$ (p=2) converges to $\frac{\pi^2}{6}$ (a famous result called the Basel problem, solved by Euler in 1734).

**Telescoping series:** Terms cancel in pairs:
$$\sum_{n=1}^{\infty} \left(\frac{1}{n} - \frac{1}{n+1}\right)$$

Partial sum: $S_N = 1 - \frac{1}{N+1} \to 1$. The series converges to $1$.

### The Divergence Test (Zero Test)

The simplest convergence check: if $\lim_{n \to \infty} a_n \neq 0$, the series **diverges**. Period. If the terms don't go to zero, you can't have a finite total.

Warning: the converse is **false**. If $a_n \to 0$, the series *might* converge or it might not (the harmonic series is the counterexample). The divergence test only rules out convergence; it cannot confirm it.

## Intuition

Think of a sequence as a trail of footsteps. Convergence means the footsteps are heading toward a definite destination.

A series is asking: what is the *total* ground covered if you could walk every step? If your steps keep shrinking fast enough — say, each step is half the previous — you cover a finite total distance. If your steps shrink slowly (like $1, \frac{1}{2}, \frac{1}{3}, \frac{1}{4}, \ldots$), you never stop accumulating ground, and you'll eventually walk to arbitrarily far distances.

Another way to see it: zoom into the $p$-series comparison. Why does $\sum \frac{1}{n^2}$ converge but $\sum \frac{1}{n}$ doesn't? Because $\frac{1}{n^2}$ shrinks quadratically — it gets small much faster. The "area" represented by those terms, if you stacked them as rectangles under the curve $1/x^2$, is bounded. The rectangles under $1/x$ are not. The [[Convergence Tests#Integral Test|integral test]] makes this geometric picture rigorous.

## Key Formula / Rule

**Geometric Series** ($|r| < 1$):
$$\sum_{n=0}^{\infty} r^n = \frac{1}{1-r}$$

**$p$-Series**:
$$\sum_{n=1}^{\infty} \frac{1}{n^p} \text{ converges iff } p > 1$$

**Divergence Test** (necessary condition only):
$$\sum a_n \text{ converges} \implies \lim_{n\to\infty} a_n = 0$$

Equivalently: $\lim_{n\to\infty} a_n \neq 0 \implies \sum a_n$ diverges.

**Partial sum definition of series convergence:**
$$\sum_{n=1}^{\infty} a_n = S \iff \lim_{N\to\infty} S_N = S, \quad S_N = \sum_{n=1}^{N} a_n$$

## Worked Example

**Problem:** Does $\displaystyle\sum_{n=1}^{\infty} \frac{3}{5^n}$ converge? If so, find its sum.

**Step 1 — Recognize the form.**

Rewrite: $\frac{3}{5^n} = 3 \cdot \left(\frac{1}{5}\right)^n$. This is a geometric series with ratio $r = \frac{1}{5}$.

Since $|r| = \frac{1}{5} < 1$, the series converges.

**Step 2 — Adjust for the starting index.**

The standard formula $\sum_{n=0}^{\infty} r^n = \frac{1}{1-r}$ starts at $n = 0$. Our sum starts at $n = 1$. Factor out one power of $r$:

$$\sum_{n=1}^{\infty} r^n = \sum_{n=0}^{\infty} r^n - r^0 = \frac{1}{1-r} - 1 = \frac{r}{1-r}$$

**Step 3 — Compute.**

$$\sum_{n=1}^{\infty} \frac{3}{5^n} = 3 \cdot \sum_{n=1}^{\infty} \left(\frac{1}{5}\right)^n = 3 \cdot \frac{1/5}{1 - 1/5} = 3 \cdot \frac{1/5}{4/5} = 3 \cdot \frac{1}{4} = \frac{3}{4}$$

**Verification:** Compute partial sums:
- $S_1 = 3/5 = 0.6$
- $S_2 = 3/5 + 3/25 = 0.6 + 0.12 = 0.72$
- $S_3 = 0.72 + 0.024 = 0.744$
- As $N \to \infty$, $S_N \to 0.75 = \frac{3}{4}$ ✓

## Gotchas

**Gotcha 1 — Confusing sequence convergence with series convergence.** Just because the sequence $a_n \to 0$ does NOT mean the series $\sum a_n$ converges. The harmonic series is the classic counterexample: $\frac{1}{n} \to 0$ but $\sum \frac{1}{n}$ diverges. These are two completely different questions.

**Gotcha 2 — Getting the geometric series index wrong.** The formula $\frac{1}{1-r}$ applies when the sum starts at $n = 0$. If it starts at $n = 1$, the sum is $\frac{r}{1-r}$. If it starts at $n = k$, the sum is $\frac{r^k}{1-r}$. Always check your starting index.

**Gotcha 3 — Applying the divergence test backwards.** The divergence test says: terms not going to zero → series diverges. It does NOT say: terms going to zero → series converges. The test only works in one direction.

**Gotcha 4 — The sign of the ratio in a geometric series.** The geometric series converges for $|r| < 1$, not just $r < 1$. A ratio of $r = -\frac{1}{2}$ still converges; a ratio of $r = -2$ diverges (even though it's negative).

**Gotcha 5 — Partial sums are the definition, not a method.** Many students treat partial sums as a "numerical approximation technique" and miss that partial sums are *what a series literally means*. There is no infinite sum until you take the limit of partial sums.

## See Also

- [[Convergence Tests]] — the toolkit for determining whether a given series converges; this entry gives the definitions, that one gives the tools
- [[Taylor and Maclaurin Series]] — every Taylor series is an infinite series; convergence questions determine where the representation is valid
- [[Integration by Parts]] — IBP appears in derivations of convergence for integrals, and in the integral test for series
- [[Kinematics - 1D Motion]] — summing infinitely fine time steps to find displacement is exactly the integral-as-limit-of-sums picture; series is the discrete version
- [[Voltage, Current, and Resistance]] — Fourier series (infinite sums of sinusoids) model periodic signals in circuits; convergence determines the validity of the model
