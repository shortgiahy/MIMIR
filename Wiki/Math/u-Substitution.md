# u-Substitution

**One-liner:** The integration technique that reverses the chain rule, transforming a composite function's integral into a simpler one by substituting an inner function.

## Core Idea
$$\int f(g(x))\,g'(x)\, dx = \int f(u)\, du, \quad u = g(x)$$
Identify an inner function $u = g(x)$; if its derivative $g'(x)$ (or a constant multiple) also appears in the integrand, substitute to simplify. This converts a hard integral in x into an easier one in u. After integrating in u, substitute back $u = g(x)$.

## Why It Exists
The chain rule tells us how to differentiate composite functions: $\frac{d}{dx}F(g(x)) = F'(g(x))\cdot g'(x)$. When integrating, we need to undo this. u-Substitution is the systematic procedure for doing so — it's the antiderivative analogue of the chain rule. It's the first and most broadly applicable integration technique, bridging Calc 1 (basic antiderivatives) into Calc 2 (all advanced techniques assume you've mastered this first).

## Real-World Applications
- **Physics:** Almost every substitution in solving differential equations uses u-sub. Integrating acceleration to find velocity when $a(t)$ is a composite function.
- **EE / Signal Processing:** Computing energy in a signal $\int |x(t)|^2 dt$ after a frequency shift uses substitution.
- **Probability / Statistics:** Change of variables in integration (the general multi-variable version) is u-substitution generalized to higher dimensions — critical for deriving the Gaussian integral and hence all of statistics.
- **ML / Robotics:** Normalizing probability distributions in Bayesian inference and computing partition functions in reinforcement learning. See [[Gradient Descent]] for applications in optimization.
- **Economics:** Consumer/producer surplus integrals with shifted demand curves.

## Intuition
Think of u-substitution as "zooming in" on the inner function. If you're looking at the integral of $2x \cdot \cos(x^2)$, notice that the $2x$ is exactly the derivative of $x^2$. That's a signal: the integrand was probably formed by differentiating $\sin(x^2)$ via the chain rule. u-sub lets you "see through" the composite structure.

A useful heuristic: look for a piece of the integrand that, when you take its derivative, gives you another piece of the integrand. That inner piece is your u.

## Derivation
If $F'(u) = f(u)$, and $u = g(x)$ is differentiable, then by the chain rule:
$$\frac{d}{dx}F(g(x)) = F'(g(x)) \cdot g'(x) = f(g(x)) \cdot g'(x)$$

Integrating both sides:
$$\int f(g(x))\cdot g'(x)\, dx = F(g(x)) + C$$

But also, substituting $u = g(x)$, $du = g'(x)\, dx$:
$$\int f(u)\, du = F(u) + C = F(g(x)) + C$$

So the substitution is valid: replacing $g(x)$ with $u$ and $g'(x)\, dx$ with $du$ gives the same answer.

**For definite integrals,** change the limits: if $u = g(x)$, then when $x = a$, $u = g(a)$; when $x = b$, $u = g(b)$:
$$\int_a^b f(g(x))\,g'(x)\, dx = \int_{g(a)}^{g(b)} f(u)\, du$$

## Worked Example
**Problem:** Evaluate $\displaystyle\int_0^1 x\, e^{x^2}\, dx$

**Step 1 — Spot the inner function.**
The exponent $x^2$ has derivative $2x$. We have $x\, dx$ in the integrand (off by factor 2) → set $u = x^2$.

**Step 2 — Compute du.**
$$u = x^2 \implies du = 2x\, dx \implies x\, dx = \frac{du}{2}$$

**Step 3 — Change the limits.**
$x = 0 \Rightarrow u = 0$; $x = 1 \Rightarrow u = 1$ (limits unchanged here, coincidentally).

**Step 4 — Substitute.**
$$\int_0^1 x\, e^{x^2}\, dx = \int_0^1 e^u \cdot \frac{du}{2} = \frac{1}{2}\int_0^1 e^u\, du$$

**Step 5 — Integrate.**
$$= \frac{1}{2}\left[e^u\right]_0^1 = \frac{1}{2}(e - 1)$$

**Answer:** $\dfrac{e-1}{2} \approx 0.859$

## See Also
- [[Integration by Parts]] — the next technique after u-sub; for products of unrelated functions
- [[Trigonometric Substitution]] — specialized substitution for radical quadratics
- [[Partial Fraction Decomposition]] — for rational functions
- [[Chain Rule]] — the differentiation rule that u-sub reverses
- [[Kinematic Equations]] — integrating constant acceleration to find velocity uses u-substitution (∫a dt with u = t)
- [[Velocity]] — position x = ∫v dt is evaluated via u-sub when v(t) is a composite function
