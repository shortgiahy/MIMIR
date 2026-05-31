# Math Wiki — Index

> Navigation hub for all Math entries. Built for Giahy's Calc 2 retake (SLCC, Summer 2026). Cross-linked to Physics and EE where the math directly appears — because understanding *where* a technique gets used is what makes it stick.

---

## Integration Techniques

These entries cover the core integration methods that appear throughout Calc 2. Most exam problems require choosing the right technique for a given integrand.

| Entry | What It Is |
|---|---|
| [[Integration by Parts]] | The product rule in reverse — for integrands that are products of unlike functions ($x e^x$, $x \ln x$, $e^x \sin x$) |
| [[Trigonometric Substitution]] | Replace radical expressions with trig functions using Pythagorean identities — three cases: $\sqrt{a^2 - x^2}$, $\sqrt{a^2 + x^2}$, $\sqrt{x^2 - a^2}$ |

**Integration techniques connect directly to:**
- **Physics** — Work integrals, impulse-momentum ($\int F \, dt$), and arc length all require these techniques. See [[Kinematics - 1D Motion]] (position from velocity = integration) and [[Newton's Second Law]] (impulse).
- **EE** — Energy in inductors and capacitors ($E = \int v \cdot i \, dt$), Laplace transforms, and convolution integrals in circuit analysis all use IBP and trig sub. See [[Voltage, Current, and Resistance]].

---

## Sequences and Series

Sequences and series answer the question: what happens when you sum infinitely many terms? This cluster builds from definition to convergence tests to the series representation of functions.

| Entry | What It Is |
|---|---|
| [[Sequences and Series]] | Foundational definitions — what a sequence is, what a series is, how convergence is defined through partial sums, and why infinite sums can be finite |
| [[Convergence Tests]] | The toolkit: Divergence Test, Geometric Series, Integral Test, Comparison Test, Limit Comparison, Ratio Test, Root Test, Alternating Series Test — with the reasoning behind each |

**Series connects directly to:**
- **EE** — Fourier series decompose periodic signals into infinite sums of sinusoids; convergence determines when the decomposition is valid. See [[Voltage, Current, and Resistance]].
- **Numerical Methods / Robotics** — Every numerical ODE solver (Runge-Kutta, Euler method) uses truncated series. Robot simulation in Isaac Sim uses these under the hood.

---

## Power Series and Function Approximation

The culmination of Calc 2 series work: representing functions as infinite polynomials, with full control over accuracy.

| Entry | What It Is |
|---|---|
| [[Taylor and Maclaurin Series]] | Deriving the infinite polynomial representation of any smooth function from its derivatives; the five essential series ($e^x$, $\sin x$, $\cos x$, $\frac{1}{1-x}$, $\ln(1+x)$); radius of convergence; Euler's formula |

**Taylor/Maclaurin connects directly to:**
- **Physics** — Linearizing equations of motion around equilibrium points (small-angle approximation for pendulums, etc.) is a first-order Taylor expansion.
- **EE** — Phasor analysis in AC circuits is grounded in Euler's formula $e^{i\theta} = \cos\theta + i\sin\theta$, derived directly from the Taylor series of $e^x$. See [[Voltage, Current, and Resistance]].
- **Control Theory / Robotics** — Jacobian linearization (used to design controllers for robot arms) is a multivariable Taylor expansion around an operating point.
- **ML** — Gradient descent in neural network training is a first-order Taylor approximation of the loss landscape. Newton's method uses the second-order (Hessian) term.

---

## Reading Order for Calc 2

If starting from scratch or reviewing for an exam, follow this sequence:

1. **[[Integration by Parts]]** — the technique used everywhere; builds on Calc 1 product rule
2. **[[Trigonometric Substitution]]** — extends integration to radical expressions
3. **[[Sequences and Series]]** — new conceptual framework; read this before any convergence test
4. **[[Convergence Tests]]** — tools for series; requires [[Sequences and Series]] first
5. **[[Taylor and Maclaurin Series]]** — payoff of the series cluster; also uses integration and convergence

---

## Cross-Subject Map

| Calc 2 Topic | Physics | EE | ML & Robotics |
|---|---|---|---|
| Integration techniques | Work, impulse, arc length | Energy in circuits, Laplace transforms | Expectation integrals in probability |
| Series convergence | Sum of forces in infinite media | Fourier series for signal analysis | Infinite-horizon reward sums in RL |
| Taylor series | Linearization, approximations | Phasor analysis, small-signal transistor models | Gradient descent, Taylor-based optimizers |

---

## Other Subjects

- [[Wiki/Physics/_Index]] — not yet created
- [[Wiki/EE/_Index]] — not yet created
- [[Wiki/CS/_Index]] — not yet created
