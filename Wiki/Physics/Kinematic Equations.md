# Kinematic Equations

**One-liner:** The four kinematic equations are the closed-form results of integrating constant acceleration twice, giving position and velocity as explicit functions of time.

## Core Idea
$$v = v_0 + at$$
$$\Delta x = v_0 t + \tfrac{1}{2}at^2$$
$$v^2 = v_0^2 + 2a\Delta x$$
$$\Delta x = \frac{v_0 + v}{2}\cdot t$$

These only hold when **acceleration is constant**. They are not new laws — they are convenient algebra derived from integrating $a = \text{const}$.

## Why It Exists
[[Newton's Second Law]] tells you acceleration; the kinematic equations let you skip the [[Integral]] calculus in the common case of constant $a$ (free fall, constant engine thrust, uniform braking) and solve for position or velocity algebraically. Without them, every introductory projectile problem would require setting up and evaluating integrals.

## Real-World Applications
- **Projectile motion:** calculating range and peak height of a thrown ball (constant $a = -g$ vertically, $a = 0$ horizontally).
- **Braking distance:** automobile safety standards use $v^2 = v_0^2 + 2a\Delta x$ to relate stopping distance to initial speed and deceleration.
- **Rocket staging (simplified):** approximate burn phase with constant thrust-to-weight, then use kinematics to estimate velocity gained.
- **Robotics (trapezoidal motion profiles):** Baymax's arm controller uses kinematic equations during the constant-acceleration phase of a move — accelerate, cruise, decelerate.

## Intuition
These equations are what you get when you assume the acceleration arrow never changes length or direction. Constant acceleration means $v$-vs-$t$ is a straight line (slope = $a$). Position is the area under that line, which is a trapezoid — that's where $\Delta x = \frac{v_0+v}{2}\cdot t$ comes from directly. The rest follow from substitution.

## Derivation
Start with constant acceleration $a$.

**Equation 1:** Integrate $a$ once:
$$v(t) = v_0 + \int_0^t a\, dt' = v_0 + at$$

**Equation 2:** Integrate $v(t)$ to get position:
$$x(t) = x_0 + \int_0^t (v_0 + at')\, dt' = x_0 + v_0 t + \tfrac{1}{2}at^2$$
So $\Delta x = v_0 t + \tfrac{1}{2}at^2$.

**Equation 3:** Eliminate $t$ between Eq.1 ($t = (v-v_0)/a$) and Eq.2:
$$\Delta x = v_0 \cdot\frac{v-v_0}{a} + \tfrac{1}{2}a\left(\frac{v-v_0}{a}\right)^2 = \frac{v^2 - v_0^2}{2a}$$
Rearranging: $v^2 = v_0^2 + 2a\Delta x$.

**Equation 4:** Average of initial and final velocity (linear $v$-vs-$t$):
$$\Delta x = \bar{v}\cdot t = \frac{v_0 + v}{2}\cdot t$$

Each equation is useful when a different variable is unknown:

| Unknown | Use equation |
|---------|-------------|
| $v$ (no $\Delta x$) | $v = v_0 + at$ |
| $\Delta x$ (no $v$) | $\Delta x = v_0 t + \frac{1}{2}at^2$ |
| $t$ unknown (no $t$) | $v^2 = v_0^2 + 2a\Delta x$ |
| $a$ unknown | $\Delta x = \frac{v_0+v}{2}\cdot t$ |

## Worked Example
A ball is dropped from a 45 m cliff ($v_0 = 0$, $a = -9.8\text{ m/s}^2$, taking down as negative).

**Step 1 — Find time to hit the ground:**
$$\Delta x = -45\text{ m}, \quad v_0 = 0$$
$$-45 = 0 + \tfrac{1}{2}(-9.8)t^2 \implies t^2 = \frac{45}{4.9} = 9.18 \implies t = 3.03\text{ s}$$

**Step 2 — Find speed at impact (using Eq.3, no $t$ needed):**
$$v^2 = 0 + 2(-9.8)(-45) = 882 \implies v = -29.7\text{ m/s}$$
Speed at impact: $|v| = 29.7\text{ m/s}$.

**Step 3 — Sanity check with Eq.1:**
$$v = 0 + (-9.8)(3.03) = -29.7\text{ m/s} \checkmark$$

## See Also
- [[Acceleration]] — these equations assume $a$ is constant; derived by integrating $a$
- [[Velocity]] — Eq.1 gives $v(t)$ directly
- [[Displacement]] — Eq.2 and Eq.4 give $\Delta x$
- [[Integral]] — the calculus operation that produces these results in the general case
- [[Derivative]] — differentiating any kinematic equation gives the one above it in the hierarchy
- [[u-Substitution]] — the integration technique used when evaluating $\int a\, dt$ and $\int v\, dt$ to produce the kinematic equations from first principles
- [[Taylor Series]] — position $x(t)$ around $t = 0$ is exactly a Taylor expansion: $x = x_0 + v_0 t + \frac{1}{2}at^2 + \dots$; the kinematic equations are the first three terms
