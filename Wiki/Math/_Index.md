# Math — Master Index

Quick navigation for all Math atomic notes. Each cluster is ordered for study: complete earlier entries before tackling later ones within a cluster.

---

## Clusters

### Integration Techniques
*Foundation: before tackling series, ensure you can integrate fluently. These techniques also appear in the proofs of several convergence tests.*

| Note | One-liner |
|------|-----------|
| [[u-Substitution]] | Reversing the chain rule — the first tool to reach for when an integral contains a composite function |
| [[Integration by Parts]] | Reversing the product rule — turns ∫u dv into uv − ∫v du; essential for integrals with polynomials times exponentials or trig |
| [[Trigonometric Substitution]] | Replaces expressions like √(a²−x²) with a trig identity to make the integral algebraic |
| [[Partial Fraction Decomposition]] | Splits rational functions into simpler fractions that can be integrated term-by-term |

---

### Sequences & Series
*Start here before convergence tests. You must understand what a series IS before testing whether it converges.*

| Note | One-liner |
|------|-----------|
| [[Sequence]] | An ordered list of numbers indexed by the positive integers; the building block of series |
| [[Series]] | The sum of a sequence — defined rigorously as the limit of partial sums |
| [[Partial Sum]] | The finite sum of the first N terms; convergence of the series means convergence of this sequence |
| [[Convergence]] | A sequence or series that approaches a definite finite value; the central concept of the cluster |
| [[Divergence]] | A sequence or series that fails to converge — either grows without bound or oscillates |
| [[Geometric Series]] | Σarⁿ — the prototypical convergent series when \|r\| < 1; sum = a/(1−r) |
| [[Harmonic Series]] | Σ1/n — the prototypical divergent series despite terms going to zero |
| [[p-Series]] | Σ1/nᵖ — converges iff p > 1; the benchmark for comparing all other series |

---

### Convergence Tests
*Study in this order: Divergence Test first (cheapest check), then Ratio Test, Integral Test, Comparison Test, Alternating Series Test.*

| Note | One-liner |
|------|-----------|
| [[Divergence Test]] | If terms don't go to zero, the series diverges — the fastest possible disqualification |
| [[Ratio Test]] | Takes the limit of \|aₙ₊₁/aₙ\| — best for factorials and exponentials; inconclusive at L=1 |
| [[Integral Test]] | If f(n)=aₙ is positive and decreasing, the series and ∫f(x)dx converge or diverge together |
| [[Comparison Test]] | Bound an unknown series above (convergence) or below (divergence) by a known series |
| [[Alternating Series Test]] | Leibniz criterion: alternating series with decreasing terms → 0 converges; error ≤ first omitted term |

---

### Power Series
*The payoff cluster — all prior work feeds here. Study Power Series and Radius of Convergence before Taylor and Maclaurin.*

| Note | One-liner |
|------|-----------|
| [[Power Series]] | Σaₙ(x−c)ⁿ — an infinite polynomial representing a function on its interval of convergence |
| [[Radius of Convergence]] | R defines the half-width of the interval where a power series converges; found via Ratio Test |
| [[Taylor Series]] | Σf⁽ⁿ⁾(a)/n!·(x−a)ⁿ — the unique power series matching every derivative of f at point a |
| [[Maclaurin Series]] | Taylor series at a=0; the five essential series (eˣ, sin x, cos x, ln(1+x), 1/(1−x)) and Euler's formula |

---

## Suggested Study Order

```
Sequence → Series → Partial Sum → Convergence → Divergence
  ↓
Geometric Series → Harmonic Series → p-Series
  ↓
Divergence Test → Ratio Test → Integral Test → Comparison Test → Alternating Series Test
  ↓
u-Substitution → Integration by Parts → Trig Sub → Partial Fractions  (can run in parallel)
  ↓
Power Series → Radius of Convergence → Taylor Series → Maclaurin Series
```

**Minimum viable path for Calc 2 exam:** Geometric Series → p-Series → all five Convergence Tests → Taylor/Maclaurin Series

---

## Cross-Subject Connections

| Math Concept | Connected To | Why |
|---|---|---|
| [[Taylor Series]] | [[Gradient Descent]] (ML) | Gradient descent is a first-order Taylor approximation of the loss function |
| [[Taylor Series]] | [[Gradient]] (ML) | The gradient IS the coefficient vector of the first-order Taylor expansion |
| [[Maclaurin Series]] | [[Activation Function]] (ML) | Taylor expansions of sigmoid/tanh explain saturation and vanishing gradients |
| [[Maclaurin Series]] | [[Neural Network]] (ML) | Softmax uses eˣ series; log-sum-exp trick comes from the ln(1+x) series |
| [[Integral Test]] | [[Work]] (Physics) | Improper integrals model infinite-domain physical work; same convergence analysis applies |
| [[Integration by Parts]] | [[Impulse]] (Physics) | ∫F dt and integration by parts both appear in deriving the impulse-momentum theorem |
| [[Integration by Parts]] | [[Work-Energy Theorem]] (Physics) | The work integral ∫F·dx is evaluated via IBP when force varies with position |
| [[Convergence]] | [[Gradient Descent]] (ML) | A converging optimization means the error sequence satisfies the mathematical convergence definition |
| [[Convergence]] | [[Stochastic Gradient Descent]] (ML) | SGD convergence analysis is a probabilistic convergence question |
| [[Power Series]] | [[Voltage]] (EE) | The z-transform in digital signal processing is a power series in z⁻¹; region of convergence determines filter stability |
| [[Radius of Convergence]] | [[Kirchhoff's Voltage Law]] (EE) | Transfer functions' poles determine the radius of convergence of their Laurent series expansions |
| [[Taylor Series]] | [[Conservation of Energy]] (Physics) | Physics simulators use Taylor (Runge-Kutta) integration to conserve energy numerically |
| [[Geometric Series]] | [[Voltage Divider]] (EE) | Ladder networks and infinite transmission lines produce geometric series for voltage at each stage |
| [[Alternating Series Test]] | [[Dot Product]] (ML) | Alternating sign patterns in weight matrices relate to conditioning; AST gives error bounds for truncated eigenvalue series |

---

## Quick Formula Sheet

$$e^x = \sum_{n=0}^\infty \frac{x^n}{n!}, \quad \sin x = \sum_{n=0}^\infty \frac{(-1)^n x^{2n+1}}{(2n+1)!}, \quad \cos x = \sum_{n=0}^\infty \frac{(-1)^n x^{2n}}{(2n)!}$$

$$\ln(1+x) = \sum_{n=1}^\infty \frac{(-1)^{n+1}x^n}{n}\ (|x|\le 1,\ x\ne -1), \qquad \frac{1}{1-x} = \sum_{n=0}^\infty x^n\ (|x|<1)$$

$$e^{i\theta} = \cos\theta + i\sin\theta \qquad \text{(Euler's formula, derived from Maclaurin series)}$$

**Convergence tests at a glance:**

| Test | Use when | Converges if | Diverges if | Inconclusive |
|------|----------|-------------|------------|--------------|
| Divergence | Always try first | — | $a_n \not\to 0$ | $a_n \to 0$ |
| Ratio | Factorials, $c^n$ | $L < 1$ | $L > 1$ | $L = 1$ |
| Integral | Positive, decreasing, integrable | $\int f\,dx < \infty$ | $\int f\,dx = \infty$ | — |
| Comparison (Direct) | Known benchmark series | $a_n \le b_n$, $\sum b_n$ conv. | $a_n \ge b_n$, $\sum b_n$ div. | Wrong direction |
| Limit Comparison | Terms behave like known series | $0 < L < \infty$ (same fate) | $0 < L < \infty$ (same fate) | $L=0$ or $L=\infty$ |
| Alt. Series Test | Alternating signs | Terms decr. → 0 | — | Non-alternating |

**p-Series:** $\sum 1/n^p$ converges iff $p > 1$.

**Geometric Series:** $\sum_{n=0}^\infty ar^n = \dfrac{a}{1-r}$ iff $|r| < 1$.
