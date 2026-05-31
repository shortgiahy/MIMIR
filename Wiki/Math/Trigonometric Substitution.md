# Trigonometric Substitution

**One-liner:** A technique that eliminates square roots of quadratic expressions by substituting a trig function, exploiting Pythagorean identities.

## Core Idea
$$\text{Three cases: } \sqrt{a^2 - x^2},\quad \sqrt{a^2 + x^2},\quad \sqrt{x^2 - a^2}$$
When an integrand contains one of these forms, substitute x = a sin θ, x = a tan θ, or x = a sec θ respectively. The Pythagorean identities ($\sin^2\theta + \cos^2\theta = 1$ and variants) then eliminate the square root, converting an algebraic integral into a trigonometric one that can be handled by standard trig integrals.

## Why It Exists
Direct substitution can't remove a square root like $\sqrt{a^2 - x^2}$ — no obvious "inner function" and its derivative are present. Trig substitution exists because trig functions satisfy algebraic identities that are precisely shaped to cancel these roots. Without it, integrals computing arc lengths, areas of ellipses, and gravitational/electric field integrals over continuous distributions would be intractable.

## Real-World Applications
- **EE / Antenna theory:** Radiation integrals for computing electric fields from charge distributions involve $\int dx/\sqrt{a^2 + x^2}$ — exactly the second trig sub case. Appears in computing potential fields from line charges.
- **Arc length:** The arc length formula $\int \sqrt{1 + (f')^2}\, dx$ frequently yields trig sub integrals, relevant in robotics path planning and [[Kinematics]].
- **Optics / Lens design:** Computing optical path lengths over curved surfaces (Fermat's principle) involves these radical forms.
- **Mechanics:** Gravitational potential energy for extended bodies, e.g., $\int_{-L}^{L} dm/\sqrt{r^2 + x^2}$, uses the $\sqrt{a^2 + x^2}$ case.
- **Statistics:** The normalization of the Cauchy distribution uses $\int dx/(1+x^2)$, derived via $x = \tan\theta$.

## Intuition
Draw a right triangle. Each substitution corresponds to labeling two sides and reading off the third:

- **x = a sin θ**: label opposite = x, hypotenuse = a → adjacent = $\sqrt{a^2 - x^2}$. This is the "circle" case — geometrically, x ranges over a semicircle of radius a.
- **x = a tan θ**: label opposite = x, adjacent = a → hypotenuse = $\sqrt{a^2 + x^2}$. This is the "hyperbola" case.
- **x = a sec θ**: label hypotenuse = x, adjacent = a → opposite = $\sqrt{x^2 - a^2}$. Requires |x| ≥ a.

The triangle isn't just a mnemonic — it's how you convert back from θ to x at the end without memorizing inverse formulas.

## Derivation
**Case 1: $\sqrt{a^2 - x^2}$**

Let $x = a\sin\theta$, so $dx = a\cos\theta\, d\theta$ and $\theta \in [-\pi/2, \pi/2]$ (so $\cos\theta \geq 0$):
$$\sqrt{a^2 - x^2} = \sqrt{a^2 - a^2\sin^2\theta} = \sqrt{a^2(1 - \sin^2\theta)} = \sqrt{a^2\cos^2\theta} = a\cos\theta$$

**Case 2: $\sqrt{a^2 + x^2}$**

Let $x = a\tan\theta$, so $dx = a\sec^2\theta\, d\theta$:
$$\sqrt{a^2 + x^2} = \sqrt{a^2 + a^2\tan^2\theta} = \sqrt{a^2\sec^2\theta} = a\sec\theta$$

**Case 3: $\sqrt{x^2 - a^2}$**

Let $x = a\sec\theta$, so $dx = a\sec\theta\tan\theta\, d\theta$:
$$\sqrt{x^2 - a^2} = \sqrt{a^2\sec^2\theta - a^2} = \sqrt{a^2\tan^2\theta} = a\tan\theta$$

In each case the square root is eliminated. After integrating in θ, convert back using the triangle.

## Worked Example
**Problem:** Evaluate $\displaystyle\int \frac{\sqrt{9 - x^2}}{x^2}\, dx$

**Step 1 — Identify the form.** $\sqrt{9 - x^2} = \sqrt{3^2 - x^2}$ → Case 1, $a = 3$.

**Step 2 — Substitute.** Let $x = 3\sin\theta$, $dx = 3\cos\theta\, d\theta$.
$$\sqrt{9 - x^2} = 3\cos\theta$$

**Step 3 — Rewrite the integral.**
$$\int \frac{3\cos\theta}{9\sin^2\theta}\cdot 3\cos\theta\, d\theta = \int \frac{9\cos^2\theta}{9\sin^2\theta}\, d\theta = \int \cot^2\theta\, d\theta$$

**Step 4 — Use identity** $\cot^2\theta = \csc^2\theta - 1$:
$$\int (\csc^2\theta - 1)\, d\theta = -\cot\theta - \theta + C$$

**Step 5 — Convert back.** Draw triangle: $\sin\theta = x/3$, so $\cos\theta = \sqrt{9-x^2}/3$, $\cot\theta = \sqrt{9-x^2}/x$, $\theta = \arcsin(x/3)$.
$$= -\frac{\sqrt{9-x^2}}{x} - \arcsin\!\left(\frac{x}{3}\right) + C$$

## See Also
- [[u-Substitution]] — simpler substitution; try first
- [[Integration by Parts]] — for products; sometimes combined with trig sub
- [[Partial Fraction Decomposition]] — for rational functions without radicals
- [[Taylor Series]] — trig functions have known series expansions useful after substitution
