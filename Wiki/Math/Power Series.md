# Power Series

**One-liner:** An infinite polynomial of the form Σaₙ(x−c)ⁿ that represents a function on some interval — the bridge between algebra and analysis.

## Core Idea
$$\sum_{n=0}^{\infty} a_n (x - c)^n = a_0 + a_1(x-c) + a_2(x-c)^2 + a_3(x-c)^3 + \cdots$$
A power series is centered at $c$; the coefficients $\{a_n\}$ determine the function it represents. For each fixed value of $x$, this is just a series of real numbers — it either converges or diverges. The set of all $x$ for which it converges is an interval centered at $c$ called the **interval of convergence**. Within that interval, the power series defines a smooth function that can be differentiated and integrated term-by-term.

## Why It Exists
Polynomials are the functions we can actually compute: add, multiply, evaluate in a finite number of arithmetic steps. Power series extend this by allowing infinitely many terms, enabling us to represent transcendental functions (sin, cos, eˣ, ln) as "infinite polynomials." This makes them computable, differentiable, and integrable in a term-by-term way that would be impossible with the original function definition. All of calculus-based physics and engineering ultimately rests on power series representations.

## Real-World Applications
- **Calculators and CPUs — computing eˣ, sin(x), cos(x):** Hardware does not have a sine button in the physical sense. It uses a polynomial approximation derived from the power series ([[Maclaurin Series]]). The microcontroller evaluates a truncated power series — fast, exact to required precision.
- **Control systems — transfer functions:** In feedback control (a core EE/Robotics topic), transfer functions H(s) are rational functions often expanded as power series in s to analyze behavior near operating points. The power series encodes stability information directly.
- **Machine learning — activation function analysis:** Taylor/power series expansions of sigmoid, tanh, and softmax near zero reveal their linear and quadratic behavior — important for initialization, vanishing gradient analysis, and understanding why [[Gradient Descent]] behaves as it does near saddle points.
- **Quantum mechanics — perturbation theory:** The energy eigenvalues of a Hamiltonian perturbed by a small parameter λ are expanded as power series in λ: E = E₀ + λE₁ + λ²E₂ + ⋯. The radius of convergence tells you how large a perturbation is tractable.
- **GPS and signal processing — Doppler expansion:** The frequency shift of a signal from a moving satellite involves 1/√(1−v²/c²), which is a power series in (v/c). For v << c, truncating after two terms gives sub-millimeter accuracy.
- **Solving ODEs:** Bessel functions, Legendre polynomials, and other solutions to ODEs in EE/physics are defined as power series, since no closed form exists.

## Intuition
A power series is a polynomial with infinitely many terms, but the terms shrink fast enough (within the radius of convergence) that the sum stays finite.

Think of zooming in on a complicated function near the point x = c. Close to c, (x−c) is small. The term aₙ(x−c)ⁿ shrinks dramatically as n grows, so the early terms dominate — the series is approximating the function with ever-higher-degree polynomial corrections.

**Term-by-term operations:** Within the interval of convergence, a power series behaves like a finite polynomial for differentiation and integration:
$$\frac{d}{dx} \sum_{n=0}^{\infty} a_n (x-c)^n = \sum_{n=1}^{\infty} n\, a_n (x-c)^{n-1}$$
$$\int \sum_{n=0}^{\infty} a_n (x-c)^n\,dx = \sum_{n=0}^{\infty} \frac{a_n}{n+1} (x-c)^{n+1} + C$$
The resulting series has the same radius of convergence (though the interval endpoints may differ).

## Derivation
**Finding the Interval of Convergence via Ratio Test:**

For the general power series $\sum a_n(x-c)^n$, apply the Ratio Test to the absolute values:
$$L = \lim_{n\to\infty} \left|\frac{a_{n+1}(x-c)^{n+1}}{a_n(x-c)^n}\right| = |x-c| \cdot \lim_{n\to\infty} \left|\frac{a_{n+1}}{a_n}\right|$$

Let $\rho = \lim_{n\to\infty} |a_{n+1}/a_n|$ (assuming the limit exists). Then:

- If $\rho = 0$: $L = 0 < 1$ for all $x$ → converges everywhere, $R = \infty$.
- If $\rho = \infty$: $L = \infty > 1$ for all $x \ne c$ → converges only at $x = c$, $R = 0$.
- Otherwise: $L < 1$ iff $|x - c| < 1/\rho$. The radius of convergence is $R = 1/\rho$.

The interval of convergence is $(c-R, c+R)$ at minimum; the endpoints $x = c \pm R$ must be checked separately by substituting into the series and applying other tests.

**Differentiation theorem:** If $f(x) = \sum_{n=0}^\infty a_n(x-c)^n$ converges on $(c-R, c+R)$, then $f$ is differentiable on that interval and $f'(x) = \sum_{n=1}^\infty n a_n(x-c)^{n-1}$, also with radius of convergence $R$.

## Worked Example
**Problem:** Find the interval of convergence of $\displaystyle\sum_{n=1}^{\infty} \frac{(x-2)^n}{n \cdot 3^n}$.

**Step 1 — Ratio Test.**
$$\left|\frac{a_{n+1}}{a_n}\right| = \left|\frac{(x-2)^{n+1}}{(n+1)3^{n+1}} \cdot \frac{n \cdot 3^n}{(x-2)^n}\right| = |x-2| \cdot \frac{n}{3(n+1)}$$

$$L = |x-2| \cdot \lim_{n\to\infty} \frac{n}{3(n+1)} = \frac{|x-2|}{3}$$

**Step 2 — Radius of convergence.** $L < 1$ when $|x-2| < 3$, so $R = 3$. Series converges on $(-1, 5)$ in the interior.

**Step 3 — Check endpoints.**

At $x = 5$: $\sum \dfrac{3^n}{n \cdot 3^n} = \sum \dfrac{1}{n}$ → [[Harmonic Series]], diverges.

At $x = -1$: $\sum \dfrac{(-3)^n}{n \cdot 3^n} = \sum \dfrac{(-1)^n}{n}$ → alternating harmonic series, converges by [[Alternating Series Test]].

**Step 4 — Interval of convergence:** $[-1, 5)$.

## See Also
- [[Radius of Convergence]] — how to find R and what happens at the boundary
- [[Taylor Series]] — the canonical way to find the coefficients aₙ for a given function
- [[Maclaurin Series]] — power series centered at 0 for standard functions
- [[Ratio Test]] — the primary tool for finding the radius of convergence
- [[Alternating Series Test]] — often needed at the endpoints of convergence
- [[Geometric Series]] — the simplest power series: $\sum x^n = 1/(1-x)$ for |x| < 1
- [[Convergence]] — the general concept applied term-by-term here
- [[Transfer Functions]] — EE application of power series in the s-domain
- [[Gradient Descent]] — uses first-order Taylor (power series) approximation of the loss function
