# Radius of Convergence

**One-liner:** The number R that defines how far from the center a power series converges — found via the Ratio Test, with endpoint behavior requiring separate investigation.

## Core Idea
For a power series $\sum_{n=0}^\infty a_n(x-c)^n$:
$$R = \frac{1}{\displaystyle\limsup_{n\to\infty} |a_n|^{1/n}}$$

The series **converges absolutely** for $|x - c| < R$, **diverges** for $|x - c| > R$, and behavior at $|x - c| = R$ (the two endpoints) must be checked separately.

In practice, R is usually found via the Ratio Test:
$$R = \lim_{n\to\infty} \left|\frac{a_n}{a_{n+1}}\right| \quad \text{(when this limit exists)}$$

## Why It Exists
A power series $\sum a_n(x-c)^n$ is not a function of x on all of ℝ — it only makes sense where the series converges. The radius of convergence precisely characterizes this domain. Without knowing R, you cannot safely evaluate, differentiate, or integrate the series — operations done outside the interval of convergence can give nonsense. For [[Taylor Series]], R tells you how large a neighborhood around $c$ the Taylor expansion faithfully represents the original function.

## Real-World Applications
- **Taylor series in physics simulations:** A physics engine using a Taylor expansion of sin(θ) for pendulum motion is only valid within the radius of convergence (which for sin is ∞, but for other series it's finite). Naively applying a series outside its R causes catastrophic numerical errors.
- **Signal processing — z-transform:** In digital signal processing, the z-transform of a discrete signal is a power series in z⁻¹. The radius of convergence determines the **region of convergence (ROC)**, which must include the unit circle for DTFT to exist — a fundamental constraint in filter design.
- **Control systems — Laplace series:** Laurent/power series expansions of transfer functions H(s) have a radius of convergence related to the poles of the system. Poles outside the ROC correspond to unstable modes. See [[Transfer Functions]].
- **Machine learning — convergence of learning rate series:** Gradient descent with learning rate η produces a sequence that can be analyzed as a power series in η. The radius of convergence determines the maximum stable learning rate — exceed it, and training diverges.
- **Complex analysis — analytic functions:** In complex analysis (essential for advanced EE), a function is analytic at a point iff it has a power series representation with R > 0 there. Singularities (poles, branch points) of circuits' transfer functions are exactly the points that limit R.

## Intuition
Imagine you are at the center c, and you can walk along the real line. The power series is like a spotlight that illuminates the ground around you. The radius of convergence R is how far the spotlight reaches — within that circle, the series faithfully represents the function; outside it, the partial sums explode.

**Why do singularities kill convergence?** Consider $f(x) = 1/(1-x)$, expanded around $c = 0$. It has a singularity at $x = 1$. No matter how many polynomial terms you use, the series cannot capture the blowup at $x = 1$. So $R = 1$ — the series is valid only for $|x| < 1$.

In the complex plane, $R$ is the distance from the center to the *nearest singularity*. A Taylor series about $c$ cannot converge past the nearest pole or branch point of the function in the complex plane, even if you only care about real values.

**Endpoint behavior is unpredictable:** At $|x - c| = R$, anything can happen:
- $\sum x^n$ at $x = 1$: diverges (geometric series with ratio 1)
- $\sum x^n/n$ at $x = -1$: converges (alternating harmonic series)
- $\sum x^n/n^2$ at $x = \pm 1$: both converge absolutely (p-series, p=2)

Each endpoint must be tested individually.

## Derivation
**Hadamard's Formula** (the limsup version):

Define $R = 1/\limsup_{n\to\infty} |a_n|^{1/n}$ (with $R = \infty$ if the limsup is 0, and $R = 0$ if the limsup is ∞).

The key step is the **root test**: for the series $\sum a_n(x-c)^n$:
$$\limsup_{n\to\infty} |a_n(x-c)^n|^{1/n} = |x-c| \cdot \limsup_{n\to\infty} |a_n|^{1/n} = \frac{|x-c|}{R}$$

By the root test:
- If $|x-c|/R < 1$ (i.e., $|x-c| < R$): converges absolutely.
- If $|x-c|/R > 1$ (i.e., $|x-c| > R$): diverges.

**Ratio Test formula** (easier to compute when $\lim |a_{n+1}/a_n|$ exists):

Apply the [[Ratio Test]] to $|a_n(x-c)^n|$:
$$L = \lim_{n\to\infty} \frac{|a_{n+1}||x-c|^{n+1}}{|a_n||x-c|^n} = |x-c| \cdot \lim_{n\to\infty} \frac{|a_{n+1}|}{|a_n|}$$

Set $L < 1$ and solve for $|x-c|$: converges when $|x-c| < \lim |a_n/a_{n+1}| = R$.

## Worked Example
**Problem:** Find the radius and interval of convergence of the [[Maclaurin Series]] for $e^x = \displaystyle\sum_{n=0}^\infty \dfrac{x^n}{n!}$.

**Step 1 — Identify coefficients.** $a_n = 1/n!$, centered at $c = 0$.

**Step 2 — Apply the Ratio Test formula.**
$$R = \lim_{n\to\infty} \frac{|a_n|}{|a_{n+1}|} = \lim_{n\to\infty} \frac{1/n!}{1/(n+1)!} = \lim_{n\to\infty} (n+1) = \infty$$

**Step 3 — Conclude.** $R = \infty$. The series for $e^x$ converges **for all real x** (and in fact for all complex x). No endpoint check needed.

**Contrast — geometric series:** $\sum x^n$ has $a_n = 1$, so $R = \lim |a_n/a_{n+1}| = \lim 1 = 1$. Converges on $(-1, 1)$.

At $x = 1$: $\sum 1^n = 1 + 1 + 1 + \cdots$ diverges.
At $x = -1$: $\sum (-1)^n$ diverges (terms don't → 0 in an alternating way that satisfies AST — they oscillate between -1 and 1).

Interval of convergence: $(-1, 1)$.

## See Also
- [[Power Series]] — the object whose convergence R describes
- [[Taylor Series]] — R determines where the Taylor expansion is valid
- [[Maclaurin Series]] — specific R values for standard functions
- [[Ratio Test]] — the main computational tool for finding R
- [[Alternating Series Test]] — often needed at the endpoints $x = c \pm R$
- [[Comparison Test]] — useful for endpoint convergence analysis
- [[Geometric Series]] — the canonical power series with R = 1
- [[Convergence]] — the general framework R operates within
- [[Transfer Functions]] — EE context where R determines system stability regions
