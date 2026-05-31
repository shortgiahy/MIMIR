# Acceleration

**One-liner:** Acceleration is the rate at which velocity changes — it tells you how quickly your speed or direction (or both) is shifting.

## Core Idea
$$a = \frac{dv}{dt} = \frac{d^2x}{dt^2} \qquad \bar{a} = \frac{\Delta v}{\Delta t}$$
Acceleration is a vector. It can be nonzero even when [[Speed]] is constant, if direction is changing (e.g., circular motion). The sign of acceleration indicates direction, not whether an object is speeding up or slowing down — that depends on the *relative* signs of velocity and acceleration.

## Why It Exists
[[Newton's Second Law]] says forces cause acceleration. To connect the force world (causes) to the motion world (effects), we need a quantity that bridges them. Acceleration is exactly that bridge: $a = F_{\text{net}}/m$. Without acceleration, Newton's laws would be inexpressible.

## Real-World Applications
- **Accelerometers in phones:** measure the acceleration vector (including gravity) — used for screen rotation, step counting, and crash detection (sudden deceleration triggers emergency calls).
- **Inertial navigation systems (INS):** aircraft and rockets integrate acceleration twice to track position when GPS isn't available.
- **Robotics:** Baymax's joint controllers use [[Derivative]] of velocity (acceleration feedback) to achieve smooth, jerk-limited motion — high acceleration means jerky, potentially unsafe movement.
- **Roller coaster design:** peak acceleration (in g's) determines physiological load on riders; designs must stay within ~5g sustained.
- **Automotive safety:** crash tests measure peak deceleration — above ~40g instantaneous, injuries become severe.

## Intuition
Hold a full coffee cup while accelerating a car: the coffee pushes backward. That push is the physical signature of acceleration. You feel acceleration, not velocity — in a smooth airplane at 900 km/h (constant velocity), you feel nothing. Stomp the gas at 30 km/h, and you're pressed into your seat. Acceleration is what your body senses.

## Derivation
Start with [[Velocity]] $v = dx/dt$. Acceleration is its [[Derivative]]:
$$a(t) = \frac{dv}{dt} = \frac{d}{dt}\left(\frac{dx}{dt}\right) = \frac{d^2x}{dt^2}$$
This is the second derivative of position with respect to time. Integrating once gives velocity from acceleration:
$$v(t) = v_0 + \int_0^t a(t')\, dt'$$
Integrating a second time gives position:
$$x(t) = x_0 + \int_0^t v(t')\, dt' = x_0 + v_0 t + \int_0^t\int_0^{t'} a(t'')\, dt''\, dt'$$
For the special constant-acceleration case ($a = \text{const}$), both integrals evaluate simply, producing the [[Kinematic Equations]].

## Worked Example
A car traveling at $v_0 = 20\text{ m/s}$ (east) brakes to a stop in $t = 4\text{ s}$.

**Step 1 — Find average acceleration:**
$$\bar{a} = \frac{\Delta v}{\Delta t} = \frac{0 - 20}{4 - 0} = -5\text{ m/s}^2$$

**Step 2 — Interpret the sign:**
The negative sign means acceleration is directed *west* (opposing eastward motion). The car is *decelerating* because velocity (+east) and acceleration (−east) have opposite signs.

**Step 3 — Find displacement during braking (using kinematics with constant a):**
$$\Delta x = v_0 t + \tfrac{1}{2}at^2 = 20(4) + \tfrac{1}{2}(-5)(16) = 80 - 40 = 40\text{ m}$$

**Step 4 — Sanity check with $v^2 = v_0^2 + 2a\Delta x$:**
$$0 = (20)^2 + 2(-5)(40) = 400 - 400 = 0 \checkmark$$

## See Also
- [[Velocity]] — acceleration is its time-derivative
- [[Net Force]] — Newton's Second Law connects net force to acceleration: $\vec{F}_{\text{net}} = m\vec{a}$
- [[Kinematic Equations]] — results of integrating constant acceleration twice
- [[Newton's Second Law]] — the fundamental reason acceleration matters
- [[Derivative]] — the mathematical operation that defines acceleration from velocity
- [[Integral]] — integrate acceleration to recover velocity and position
