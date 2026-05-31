# Trigonometric Substitution

**One-liner:** A technique that replaces algebraic radical expressions with trig functions, using Pythagorean identities to eliminate the radical and make the integral tractable.

## Why It Exists

Consider $\int \sqrt{1 - x^2} \, dx$. Substitution fails — there's no $x$ floating around to pair with $dx$. Integration by parts doesn't simplify it either. The problem is the square root: it tangles the algebra. 

The insight is geometric: $\sqrt{1 - x^2}$ is the height of a right triangle on the unit circle. If you *define* $x = \sin\theta$, then $\sqrt{1 - x^2} = \sqrt{1 - \sin^2\theta} = \cos\theta$. The radical vanishes, replaced by a clean trig function. You've turned an algebraic mess into a trigonometric integral — which you know how to handle.

This technique appears directly in physics and EE: computing arc lengths of ellipses, finding potential fields around charged distributions, and computing inductance in coaxial cables all require integrals with radicals of this form. Understanding the geometry underneath trig substitution means you'll recognize those integrals on sight.

## The Concept

All three cases of trig substitution exploit the same three Pythagorean identities:

$$1 - \sin^2\theta = \cos^2\theta$$
$$1 + \tan^2\theta = \sec^2\theta$$
$$\sec^2\theta - 1 = \tan^2\theta$$

Each substitution is chosen to transform the expression *inside* the radical into a perfect square (using one of those identities), so the radical drops away.

---

### Case 1: $\sqrt{a^2 - x^2}$

**Substitution:** $x = a\sin\theta$, so $dx = a\cos\theta \, d\theta$

**Why:** Replace $x$ with $a\sin\theta$:
$$\sqrt{a^2 - x^2} = \sqrt{a^2 - a^2\sin^2\theta} = \sqrt{a^2(1 - \sin^2\theta)} = \sqrt{a^2\cos^2\theta} = a|\cos\theta|$$

For $\theta \in [-\pi/2, \pi/2]$ (which is where $\arcsin$ lives), $\cos\theta \geq 0$, so $|cos\theta| = \cos\theta$ and the absolute value drops.

**Geometric picture:** Draw a right triangle with hypotenuse $a$ and one leg $x$. Then $\sin\theta = x/a$. The adjacent leg is $\sqrt{a^2 - x^2}$. This triangle is your "Rosetta stone" for converting back to $x$ at the end.

**Range:** $\theta \in \left[-\frac{\pi}{2}, \frac{\pi}{2}\right]$

---

### Case 2: $\sqrt{a^2 + x^2}$

**Substitution:** $x = a\tan\theta$, so $dx = a\sec^2\theta \, d\theta$

**Why:** Replace $x$ with $a\tan\theta$:
$$\sqrt{a^2 + x^2} = \sqrt{a^2 + a^2\tan^2\theta} = \sqrt{a^2(1 + \tan^2\theta)} = \sqrt{a^2\sec^2\theta} = a|\sec\theta|$$

For $\theta \in (-\pi/2, \pi/2)$, $\sec\theta > 0$.

**Geometric picture:** Right triangle with legs $a$ (adjacent) and $x$ (opposite). Then $\tan\theta = x/a$ and the hypotenuse is $\sqrt{a^2 + x^2}$.

**Range:** $\theta \in \left(-\frac{\pi}{2}, \frac{\pi}{2}\right)$

---

### Case 3: $\sqrt{x^2 - a^2}$

**Substitution:** $x = a\sec\theta$, so $dx = a\sec\theta\tan\theta \, d\theta$

**Why:** Replace $x$ with $a\sec\theta$:
$$\sqrt{x^2 - a^2} = \sqrt{a^2\sec^2\theta - a^2} = \sqrt{a^2(\sec^2\theta - 1)} = \sqrt{a^2\tan^2\theta} = a|\tan\theta|$$

For $\theta \in [0, \pi/2)$ (where $\sec\theta \geq 1$, meaning $x \geq a$), $\tan\theta \geq 0$.

**Geometric picture:** Right triangle with hypotenuse $x$ and adjacent leg $a$. Then $\sec\theta = x/a$ and the opposite leg is $\sqrt{x^2 - a^2}$.

**Range:** $\theta \in \left[0, \frac{\pi}{2}\right)$ for $x \geq a$

---

### The Back-Substitution Step

After integrating in $\theta$, you must convert back to $x$. The triangle diagrams are how you do this cleanly — don't try to use inverse trig formulas in your head. Draw the triangle, label its sides using your substitution, and read off whatever trig expressions remain.

For example, in Case 1 with $x = a\sin\theta$: after integrating, suppose you have $\cos\theta$ in the answer. From the triangle, $\cos\theta = \frac{\sqrt{a^2 - x^2}}{a}$. Substitute directly.

### Completing the Square First

Sometimes the radical contains a quadratic like $\sqrt{x^2 + 4x + 8}$. Complete the square first:
$$x^2 + 4x + 8 = (x+2)^2 + 4$$

Now let $u = x + 2$, so the expression becomes $\sqrt{u^2 + 4}$, which is Case 2 with $a = 2$. Completing the square transforms a "shifted" problem into a standard one.

## Intuition

The three cases correspond to the three sides of a right triangle. Think of the radical as a side length — it must satisfy a Pythagorean relationship. By *constructing* a right triangle where the radical is one side, you can always read off the other sides in trig terms and let the Pythagorean identity do the simplification.

Alternatively: the unit circle lives in your head. $\sin^2 + \cos^2 = 1$ is the unit circle equation. When your radical looks like $\sqrt{1 - x^2}$, you're literally describing a point on the circle. Trig substitution is just making that geometry explicit.

## Key Formula / Rule

| Radical Form | Substitution | Identity Used | Triangle Setup |
|---|---|---|---|
| $\sqrt{a^2 - x^2}$ | $x = a\sin\theta$ | $1 - \sin^2\theta = \cos^2\theta$ | hyp $= a$, opp $= x$, adj $= \sqrt{a^2-x^2}$ |
| $\sqrt{a^2 + x^2}$ | $x = a\tan\theta$ | $1 + \tan^2\theta = \sec^2\theta$ | adj $= a$, opp $= x$, hyp $= \sqrt{a^2+x^2}$ |
| $\sqrt{x^2 - a^2}$ | $x = a\sec\theta$ | $\sec^2\theta - 1 = \tan^2\theta$ | hyp $= x$, adj $= a$, opp $= \sqrt{x^2-a^2}$ |

## Worked Example

**Problem:** Evaluate $\displaystyle\int \frac{x^2}{\sqrt{9 - x^2}} \, dx$.

**Step 1 — Identify the case.**
The radical is $\sqrt{9 - x^2} = \sqrt{3^2 - x^2}$. This is Case 1 with $a = 3$.

**Step 2 — Apply the substitution.**
$$x = 3\sin\theta, \quad dx = 3\cos\theta \, d\theta$$
$$\sqrt{9 - x^2} = 3\cos\theta \quad \text{(assuming } \theta \in [-\pi/2, \pi/2])$$

**Step 3 — Rewrite the entire integral in $\theta$.**
$$\int \frac{x^2}{\sqrt{9 - x^2}} \, dx = \int \frac{(3\sin\theta)^2}{3\cos\theta} \cdot 3\cos\theta \, d\theta$$

The $3\cos\theta$ in the denominator cancels with the $3\cos\theta$ in $dx$:
$$= \int \frac{9\sin^2\theta \cdot 3\cos\theta}{3\cos\theta} \, d\theta = \int 9\sin^2\theta \, d\theta$$

**Step 4 — Use the power-reduction identity.**
$$\sin^2\theta = \frac{1 - \cos 2\theta}{2}$$

$$\int 9\sin^2\theta \, d\theta = 9\int \frac{1 - \cos 2\theta}{2} \, d\theta = \frac{9}{2}\int (1 - \cos 2\theta) \, d\theta$$

$$= \frac{9}{2}\left[\theta - \frac{\sin 2\theta}{2}\right] + C = \frac{9}{2}\theta - \frac{9}{4}\sin 2\theta + C$$

**Step 5 — Use the double-angle identity.**
$$\sin 2\theta = 2\sin\theta\cos\theta$$

$$= \frac{9}{2}\theta - \frac{9}{4}(2\sin\theta\cos\theta) + C = \frac{9}{2}\theta - \frac{9}{2}\sin\theta\cos\theta + C$$

**Step 6 — Back-substitute using the triangle.**
From $x = 3\sin\theta$: draw the triangle with hypotenuse 3, opposite leg $x$.

- $\sin\theta = \frac{x}{3}$, so $\theta = \arcsin\left(\frac{x}{3}\right)$
- Adjacent leg $= \sqrt{9 - x^2}$, so $\cos\theta = \frac{\sqrt{9 - x^2}}{3}$

Substituting:
$$= \frac{9}{2}\arcsin\!\left(\frac{x}{3}\right) - \frac{9}{2} \cdot \frac{x}{3} \cdot \frac{\sqrt{9-x^2}}{3} + C$$

$$= \boxed{\frac{9}{2}\arcsin\!\left(\frac{x}{3}\right) - \frac{x\sqrt{9-x^2}}{2} + C}$$

## Gotchas

**Gotcha 1 — Using the wrong case.** Memorize the three forms by their plus/minus sign: $a^2 - x^2$ → sin, $a^2 + x^2$ → tan, $x^2 - a^2$ → sec. The position of the $x^2$ term relative to $a^2$ tells you which case you're in.

**Gotcha 2 — Forgetting to substitute $dx$.** Every $dx$ must become $(a\cos\theta \, d\theta)$ or whatever the differentiated form is. Missing this is a critical error that makes the whole integral wrong.

**Gotcha 3 — Dropping the absolute value.** $\sqrt{\cos^2\theta} = |\cos\theta|$, not just $\cos\theta$. For the standard ranges of $\theta$ in each case, the absolute value resolves to a positive expression — but you must justify why, not just drop it.

**Gotcha 4 — Forgetting to back-substitute.** The final answer must be in terms of $x$, not $\theta$. Draw the triangle and read off every trig expression before writing your final answer.

**Gotcha 5 — Not completing the square.** When the quadratic under the radical has a cross-term (e.g., $\sqrt{x^2 + 6x + 13}$), complete the square first. Jumping straight to trig substitution without this step produces an integral that doesn't match any standard form.

**Gotcha 6 — Definite integrals and limits.** If you're doing a definite integral $\int_a^b$, you must either change the limits to $\theta$-values when you make the substitution, or keep the $x$-limits and back-substitute before evaluating. Mixing them is a fatal error.

## See Also

- [[Integration by Parts]] — complementary technique; sometimes a trig sub simplifies an integral enough to finish it with IBP
- [[Taylor and Maclaurin Series]] — series for $\arcsin x$ and $\arctan x$ can be derived by applying trig substitution to their integral representations
- [[Sequences and Series]] — integrals involving radicals appear in arc-length series and convergence proofs
