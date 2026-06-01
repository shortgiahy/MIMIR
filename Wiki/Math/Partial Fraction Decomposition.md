# Partial Fraction Decomposition

**One-liner:** A technique that breaks a rational function into a sum of simpler fractions, each of which is easy to integrate.

## Core Idea
$$\frac{P(x)}{Q(x)} = \frac{A}{x - r_1} + \frac{B}{x - r_2} + \cdots$$
If you have a rational function (polynomial over polynomial) where deg(P) < deg(Q), factor the denominator and decompose into partial fractions. Each resulting piece integrates to a natural log or an arctangent — both elementary. This is the reversal of "adding fractions with common denominators."

## Why It Exists
Integrating $\frac{1}{x^2 - 1}$ directly is hard. But $\frac{1}{x^2-1} = \frac{1/2}{x-1} - \frac{1/2}{x+1}$, and each piece integrates trivially to $\pm\frac{1}{2}\ln|x \pm 1|$. Partial fractions exist to reduce complex rational integrands into a sum of the two integrable archetypes: $1/(x-a)$ and $1/(x^2+a^2)$. Without it, integrating rational functions would require guessing antiderivatives with no systematic path.

## Real-World Applications
- **Control Systems / EE:** Inverse Laplace transforms are essentially partial fraction decomposition. Every time you find the time-domain response of a circuit or a control system from its transfer function $H(s)$, you PFD it first. Directly used in filter design (IIR filters), PID controllers.
- **Signal Processing:** Z-transform inversion for discrete-time systems follows the same structure — PFD converts a complex transfer function into recognizable first/second-order pieces.
- **Probability:** Computing certain CDFs and moment generating functions of rational form.
- **Physics:** Integrating equations of motion with velocity-dependent drag forces (e.g., $\int dv/(v_T^2 - v^2)$) uses PFD.

## Intuition
Think of PFD as "un-adding fractions." When you add $\frac{1}{2} + \frac{1}{3}$, you find a common denominator to get $\frac{5}{6}$. PFD goes backward: given $\frac{5}{6}$, recover $\frac{1}{2} + \frac{1}{3}$. The point is that the decomposed pieces are each trivially integrable, while the combined fraction is not.

The method works because the field of rational functions over ℝ has a unique decomposition theorem: every proper rational function decomposes uniquely into partial fractions over the real roots and irreducible quadratic factors of the denominator.

## Derivation
**Setup:** Given $\frac{P(x)}{Q(x)}$ with deg(P) < deg(Q):

**Step 1 — Factor Q(x)** into linear factors $(x - r_i)^{m_i}$ and irreducible quadratic factors $(x^2 + bx + c)^{k_j}$.

**Step 2 — Write the decomposition:**
- For each $(x - r)^m$: contribute $\frac{A_1}{x-r} + \frac{A_2}{(x-r)^2} + \cdots + \frac{A_m}{(x-r)^m}$
- For each $(x^2 + bx + c)^k$: contribute $\frac{B_1x + C_1}{x^2+bx+c} + \cdots + \frac{B_kx + C_k}{(x^2+bx+c)^k}$

**Step 3 — Clear denominators** (multiply both sides by Q(x)).

**Step 4 — Solve for constants** either by:
- Substituting the roots $x = r_i$ (the "cover-up" method for simple roots)
- Expanding and matching coefficients of like powers of x
- A mix of both

**Step 5 — Integrate each piece:**
$$\int \frac{A}{x - r}\, dx = A\ln|x-r| + C$$
$$\int \frac{Bx + C}{x^2 + bx + c}\, dx = \frac{B}{2}\ln|x^2+bx+c| + \frac{2C - Bb}{\sqrt{4c-b^2}}\arctan\!\left(\frac{2x+b}{\sqrt{4c-b^2}}\right) + C$$

If deg(P) ≥ deg(Q), perform polynomial long division first to extract the polynomial part, then apply PFD to the remainder.

## Worked Example
**Problem:** Evaluate $\displaystyle\int \frac{2x + 1}{x^2 - x - 6}\, dx$

**Step 1 — Factor denominator.**
$x^2 - x - 6 = (x - 3)(x + 2)$

**Step 2 — Write decomposition.**
$$\frac{2x + 1}{(x-3)(x+2)} = \frac{A}{x - 3} + \frac{B}{x + 2}$$

**Step 3 — Clear denominators.**
$$2x + 1 = A(x + 2) + B(x - 3)$$

**Step 4 — Solve for constants (cover-up method).**
Set $x = 3$: $7 = 5A \Rightarrow A = 7/5$
Set $x = -2$: $-3 = -5B \Rightarrow B = 3/5$

**Step 5 — Integrate.**
$$\int \frac{2x+1}{x^2-x-6}\, dx = \frac{7}{5}\int\frac{dx}{x-3} + \frac{3}{5}\int\frac{dx}{x+2}$$
$$= \frac{7}{5}\ln|x - 3| + \frac{3}{5}\ln|x + 2| + C$$

**Check:** Differentiate using quotient/chain rules to recover the original integrand. ✓

## See Also
- [[u-Substitution]] — often used after PFD to handle each piece
- [[Integration by Parts]] — for products; sometimes combined with PFD
- [[Trigonometric Substitution]] — for integrands with radicals
- [[Convergence]] — PFD appears when analyzing convergence of certain series via integral representations
