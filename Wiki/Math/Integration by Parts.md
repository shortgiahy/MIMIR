# Integration by Parts

**One-liner:** A technique that transforms the integral of a product into a simpler integral by reversing the product rule of differentiation.

## Core Idea
$$\int u \, dv = uv - \int v \, du$$
Given an integral of the form ∫f(x)g(x) dx, we assign one factor as u and the other as dv, then use the formula above to replace it with (hopefully) an easier integral. The art is in choosing u and dv wisely — the LIATE rule (Logarithms, Inverse trig, Algebraic, Trig, Exponential) gives the priority order for u.

## Why It Exists
Differentiation has the product rule: d(uv)/dx = u'v + uv'. Integration is the reverse process, but there's no direct "product rule for integrals." Integration by parts fills this gap — it's what you reach for when substitution fails because the integrand is a product of two unrelated function families.

## Real-World Applications
- **Signal processing / EE:** Computing Fourier coefficients requires integrating products like x·sin(nx) or x·cos(nx) — integration by parts is unavoidable. Every frequency decomposition in your phone's audio codec relies on this.
- **Physics — Work:** The work-energy theorem involves ∫F·v dt; when force is a function of time and velocity changes, IBP appears. Direct link: [[Work]].
- **Physics — Impulse:** Impulse integrals ∫F(t) dt with time-varying forces in collision problems. Link: [[Impulse]].
- **Probability / ML:** Expected values E[X] = ∫x·f(x) dx for continuous distributions often require IBP, especially for normal distributions and moment calculations used in [[Gradient Descent]] variance analysis.
- **Laplace Transforms:** The Laplace transform L{f'(t)} = sF(s) − f(0) is derived entirely via integration by parts — foundational for control systems and circuit analysis.
- **Quantum Mechanics:** Expectation values ⟨p⟩ = ∫ψ*(-iℏ∂/∂x)ψ dx use IBP to show Hermiticity of the momentum operator.

## Intuition
Think of IBP as a trade. You have a hard integral. You "trade" part of the complexity: differentiate one factor (making it simpler) while integrating the other (possibly making it more complex). The deal is profitable when differentiating u eventually kills it (polynomials go to zero after enough IBP) and the resulting ∫v du is tractable. The formula is just: "evaluate the easy part (uv) at the boundary, then handle the remainder."

The geometric picture: ∫u dv + ∫v du = uv (the area of a rectangle). IBP is literally computing the area two different ways — horizontally vs. vertically.

## Derivation
Start from the product rule:
$$\frac{d}{dx}[u(x)v(x)] = u'(x)v(x) + u(x)v'(x)$$

Integrate both sides with respect to x:
$$\int \frac{d}{dx}[uv] \, dx = \int u'v \, dx + \int uv' \, dx$$

The left side integrates exactly:
$$uv = \int v \, du + \int u \, dv$$

Rearranging:
$$\int u \, dv = uv - \int v \, du$$

That's the formula. For definite integrals:
$$\int_a^b u \, dv = \left[uv\right]_a^b - \int_a^b v \, du$$

## Worked Example
**Problem:** Evaluate $\int x e^x \, dx$

**Step 1 — Choose u and dv using LIATE.**
Algebraic before Exponential → $u = x$, $dv = e^x dx$

**Step 2 — Compute du and v.**
$$du = dx, \quad v = \int e^x \, dx = e^x$$

**Step 3 — Apply the formula.**
$$\int x e^x \, dx = x \cdot e^x - \int e^x \, dx$$

**Step 4 — Evaluate the remaining integral.**
$$= x e^x - e^x + C = e^x(x - 1) + C$$

**Check:** Differentiate: $\frac{d}{dx}[e^x(x-1)] = e^x(x-1) + e^x \cdot 1 = xe^x$ ✓

## See Also
- [[u-Substitution]] — the other major technique; try this first before IBP
- [[Trigonometric Substitution]] — for integrands with radical expressions
- [[Taylor Series]] — IBP appears in the proof of Taylor's remainder theorem
- [[Work]] — physics context: ∫F·ds integrals
- [[Impulse]] — physics context: force-time integrals in collisions
- [[Partial Fraction Decomposition]] — for rational functions; complementary technique
