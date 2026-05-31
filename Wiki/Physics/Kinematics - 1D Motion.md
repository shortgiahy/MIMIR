# Kinematics - 1D Motion

**One-liner:** The mathematical description of *how* objects move through space and time — position, velocity, and acceleration — without asking *why* they move.

## Why It Exists

Before Newton ever wrote F = ma, there was a simpler question: can we describe motion precisely, without knowing its cause? Galileo's inclined plane experiments in the 1590s showed that falling objects don't move randomly — they accelerate *uniformly*, meaning velocity increases by the same amount every second. This regularity demanded a mathematical framework.

Kinematics is that framework. It gives us a set of equations that relate four quantities — displacement, initial velocity, final velocity, acceleration, and time — in every possible combination. The payoff: if you know three of those quantities, you can find the other two algebraically, no physics intuition required.

This matters because motion descriptions are the *input* to dynamics (forces). You can't apply [[Newton's Second Law]] meaningfully unless you know what acceleration actually means.

## The Concept

### Position and Displacement

**Position**, $x(t)$, is a coordinate: where the object is at time $t$, measured from some chosen origin. The choice of origin is arbitrary — it cancels out in calculations.

**Displacement** is *not* distance. It is the *change* in position:

$$\Delta x = x_f - x_i$$

Displacement is a signed quantity (positive or negative depending on direction). Distance is always non-negative and counts the total path length. If you walk 5 m forward and 3 m back, your displacement is $+2$ m but your distance traveled is $8$ m. Physics problems about motion almost always want displacement, not distance.

### Velocity

**Average velocity** is displacement divided by time elapsed:

$$\bar{v} = \frac{\Delta x}{\Delta t} = \frac{x_f - x_i}{t_f - t_i}$$

This is a slope — specifically, the slope of the secant line on an $x$ vs. $t$ graph. It tells you the *overall* rate of change, but says nothing about what happened in between.

**Instantaneous velocity** is the limit of average velocity as the time interval shrinks to zero:

$$v(t) = \lim_{\Delta t \to 0} \frac{\Delta x}{\Delta t} = \frac{dx}{dt}$$

This is the derivative of position with respect to time. Graphically, it is the slope of the *tangent line* on the $x$-$t$ graph at any moment.

Key point: **speed** is $|v|$ — the magnitude of velocity, always non-negative. Velocity has direction (sign in 1D); speed does not.

### Acceleration

**Acceleration** is the rate of change of velocity:

$$a(t) = \frac{dv}{dt} = \frac{d^2x}{dt^2}$$

Acceleration is the second derivative of position. On a $v$-$t$ graph, acceleration is the slope of the tangent line.

Critically: **acceleration does not require that speed is increasing**. A car braking has acceleration (directed opposite to velocity). An object thrown upward near its peak still has acceleration downward ($-9.8\ \text{m/s}^2$) even at the instant its speed is zero.

### Deriving the Kinematic Equations

These are not four arbitrary formulas to memorize. They are *all* derived from one assumption: **constant acceleration**. If $a = \text{const}$, integration gives everything.

**Start from the definition of acceleration:**

$$a = \frac{dv}{dt}$$

Separate variables and integrate both sides from time $0$ to time $t$:

$$\int_{v_0}^{v} dv' = \int_0^t a\, dt'$$

$$v - v_0 = at$$

$$\boxed{v = v_0 + at} \tag{1}$$

This is the velocity equation. It says velocity changes linearly with time when acceleration is constant.

**Now integrate velocity to get position:**

$$v = \frac{dx}{dt} = v_0 + at$$

Integrate:

$$\int_{x_0}^{x} dx' = \int_0^t (v_0 + at')\, dt'$$

$$x - x_0 = v_0 t + \frac{1}{2}at^2$$

$$\boxed{x = x_0 + v_0 t + \frac{1}{2}at^2} \tag{2}$$

The $\frac{1}{2}$ is not a coincidence or a magic number — it comes directly from integrating $at$. This is the area under the $v$-$t$ graph, which is a triangle plus a rectangle.

**Eliminate time between equations (1) and (2):**

From (1): $t = \frac{v - v_0}{a}$. Substitute into (2):

$$x - x_0 = v_0\left(\frac{v-v_0}{a}\right) + \frac{1}{2}a\left(\frac{v-v_0}{a}\right)^2$$

$$\Delta x = \frac{v_0(v-v_0)}{a} + \frac{(v-v_0)^2}{2a} = \frac{2v_0(v-v_0) + (v-v_0)^2}{2a}$$

$$2a\Delta x = (v-v_0)(2v_0 + v - v_0) = (v-v_0)(v+v_0) = v^2 - v_0^2$$

$$\boxed{v^2 = v_0^2 + 2a\Delta x} \tag{3}$$

**One more — average velocity shortcut:**

When acceleration is constant, the $v$-$t$ graph is linear, so average velocity is just the midpoint:

$$\bar{v} = \frac{v_0 + v}{2} = \frac{\Delta x}{\Delta t}$$

$$\boxed{x = x_0 + \frac{v_0 + v}{2}\cdot t} \tag{4}$$

### Choosing Which Equation to Use

| You know | You want | Use |
|----------|----------|-----|
| $v_0, a, t$ | $v$ | Eq. (1) |
| $v_0, a, t$ | $x$ | Eq. (2) |
| $v_0, v, a$ | $\Delta x$ | Eq. (3) |
| $v_0, v, t$ | $\Delta x$ | Eq. (4) |

Always write down your known and unknown variables *before* picking an equation. That one habit eliminates most kinematics errors.

### Free Fall

Free fall is kinematics with $a = -g = -9.80\ \text{m/s}^2$ (taking upward as positive). The equations are identical — just substitute $a \to -g$. The negative sign is doing real work: it ensures that an upward-thrown object decelerates on the way up and accelerates on the way down.

## Intuition

Think of a $v$-$t$ graph. The **slope** of that graph is acceleration. The **area under** that graph is displacement.

If the graph is a flat horizontal line ($a = 0$), the area is a rectangle and $\Delta x = v \cdot t$. If the graph is a rising line (constant positive $a$), the area is a trapezoid — which is exactly what equation (2) computes ($v_0 t + \frac{1}{2}at^2$ is the rectangle plus the triangle).

This geometric view is more reliable than formula-hunting. When you're lost on a problem, sketch the $v$-$t$ graph. The picture will usually tell you which quantity you need and where to find it.

## Key Formula / Rule

The four kinematic equations (constant acceleration only):

$$v = v_0 + at$$

$$x = x_0 + v_0 t + \tfrac{1}{2}at^2$$

$$v^2 = v_0^2 + 2a\,\Delta x$$

$$x = x_0 + \tfrac{1}{2}(v_0 + v)\,t$$

## Worked Example

**Problem:** A car starts from rest at a red light. It accelerates uniformly at $3.0\ \text{m/s}^2$. How fast is it going after $4.0\ \text{s}$, and how far has it traveled?

**Step 1 — List knowns and unknowns.**

- $v_0 = 0\ \text{m/s}$ (starts from rest)
- $a = 3.0\ \text{m/s}^2$
- $t = 4.0\ \text{s}$
- Want: $v$ and $\Delta x$

**Step 2 — Find final velocity using Eq. (1).**

$$v = v_0 + at = 0 + (3.0)(4.0) = 12\ \text{m/s}$$

Why this equation? Because we know $v_0$, $a$, $t$ and want $v$. Perfect match.

**Step 3 — Find displacement using Eq. (2).**

$$\Delta x = v_0 t + \tfrac{1}{2}at^2 = 0 + \tfrac{1}{2}(3.0)(4.0)^2 = \tfrac{1}{2}(3.0)(16) = 24\ \text{m}$$

**Step 4 — Sanity check.**

In 4 seconds, going from 0 to 12 m/s, average speed is 6 m/s. At 6 m/s for 4 s: $6 \times 4 = 24$ m. ✓ Consistent.

**Answer:** The car is traveling at $12\ \text{m/s}$ and has covered $24\ \text{m}$.

## Gotchas

**1. Displacement ≠ Distance.** Equation (3) uses $\Delta x$, which is signed. If the object reverses direction, you cannot plug in total distance traveled — you get the wrong answer.

**2. Free fall: the object at the peak still has acceleration.** At the highest point of a projectile, $v = 0$ for one instant, but $a = -9.8\ \text{m/s}^2$ the entire time — including at the top. Many students write $a = 0$ at the peak. That's wrong and breaks the equations.

**3. These equations require constant acceleration.** If acceleration is changing with time, these four equations are invalid. You need calculus integration for variable $a$, or the problem will supply a specific form of $a(t)$.

**4. Sign conventions must be consistent.** Choose a positive direction at the start and stick to it. If up is positive, then $g = -9.8\ \text{m/s}^2$. If down is positive, then $g = +9.8\ \text{m/s}^2$. Mixing conventions mid-problem is the single most common source of sign errors.

**5. "Deceleration" is not a physics word.** It informally means the magnitude of velocity is decreasing, but what matters in equations is the *direction* of $a$ relative to $v$. When $a$ and $v$ point opposite, speed decreases. When they point the same way, speed increases.

## See Also

- [[Newton's Second Law]] — explains *why* there is acceleration in the first place; kinematics just describes it
- [[Work and Kinetic Energy]] — derives directly from kinematics; the work-energy theorem comes from combining $F = ma$ with $v^2 = v_0^2 + 2a\Delta x$
- [[Calculus - Derivatives]] — velocity and acceleration are the first and second derivatives of position; kinematics is applied calculus
- [[Calculus - Integrals]] — position is the integral of velocity; displacement is the area under the $v$-$t$ curve
