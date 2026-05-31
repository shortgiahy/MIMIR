# Velocity

**One-liner:** Velocity is the rate at which an object's position changes, including the direction of that change.

## Core Idea
$$v = \frac{dx}{dt} \qquad \bar{v} = \frac{\Delta x}{\Delta t}$$
Average velocity is total [[Displacement]] divided by elapsed time. Instantaneous velocity is the [[Derivative]] of position with respect to time — the limit of average velocity as the time interval shrinks to zero. It is a vector quantity; sign (or direction in 2D) encodes which way you're moving.

## Why It Exists
Knowing where something *is* isn't enough to predict the future — you need to know how fast and in what direction it's moving. Velocity is the minimal additional information that lets you project forward in time. Without it, [[Newton's Second Law]] has nothing to integrate, and there's no way to connect force to motion.

## Real-World Applications
- **Air traffic control:** planes are tracked by position and velocity vectors; controllers need both to predict paths and prevent collisions.
- **Robotics (Baymax):** a robot arm's joint velocity tells the controller which way each joint is swinging, which is essential for smooth, collision-free motion planning.
- **Automotive crash testing:** airbag sensors measure velocity change (rate of deceleration), not just position, to decide when to deploy.
- **Ballistics:** artillery fire solutions require muzzle velocity magnitude and direction to predict trajectory.

## Intuition
If you looked at a position-vs-time graph, average velocity is the slope of the line connecting two points. Instantaneous velocity is the slope of the tangent line at a single point. When the tangent line is steep, you're moving fast; when it's flat, you're barely moving; when it slopes downward, you're moving in the negative direction.

## Derivation
Start with the definition of average velocity over interval $[t, t+\Delta t]$:
$$\bar{v} = \frac{x(t + \Delta t) - x(t)}{\Delta t}$$
Take the limit as $\Delta t \to 0$:
$$v(t) = \lim_{\Delta t \to 0} \frac{x(t + \Delta t) - x(t)}{\Delta t} = \frac{dx}{dt}$$
This is the definition of the [[Derivative]]. Running in reverse (integrating) gives position from velocity:
$$x(t) = x_0 + \int_0^t v(t')\, dt'$$
For the special case of constant velocity, the integral is trivial: $x = x_0 + vt$.

## Worked Example
A runner's position is given by $x(t) = 3t^2 - 2t + 1$ meters, where $t$ is in seconds.

**Step 1 — Find instantaneous velocity by differentiating:**
$$v(t) = \frac{dx}{dt} = 6t - 2 \text{ m/s}$$

**Step 2 — Find velocity at $t = 2\text{ s}$:**
$$v(2) = 6(2) - 2 = 10\text{ m/s}$$

**Step 3 — Find average velocity between $t = 1\text{ s}$ and $t = 3\text{ s}$:**
$$x(1) = 3(1)^2 - 2(1) + 1 = 2\text{ m}$$
$$x(3) = 3(9) - 2(3) + 1 = 22\text{ m}$$
$$\bar{v} = \frac{22 - 2}{3 - 1} = \frac{20}{2} = 10\text{ m/s}$$

For this linear $v(t)$, the instantaneous velocity at the midpoint equals the average — a useful sanity check.

## See Also
- [[Displacement]] — velocity is the time-derivative of displacement
- [[Speed]] — the magnitude of velocity; loses direction information
- [[Acceleration]] — the time-derivative of velocity
- [[Derivative]] — the mathematical tool that defines instantaneous velocity
- [[Integral]] — integrate velocity to recover position
- [[Kinematic Equations]] — special-case results for constant acceleration
