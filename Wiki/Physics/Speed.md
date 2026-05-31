# Speed

**One-liner:** Speed is how fast an object is moving, without any information about which direction — it is the magnitude of velocity.

## Core Idea
$$s = |v| = \left|\frac{dx}{dt}\right|$$
Speed is a scalar — always non-negative. Average speed is total [[Distance]] divided by elapsed time (not displacement). Instantaneous speed is the magnitude of instantaneous [[Velocity]]. You can have zero velocity (standing still) and zero speed simultaneously; you cannot have negative speed.

## Why It Exists
Direction is not always the relevant quantity. Speed limits on roads, wind speed in weather reports, and processor clock speed all describe magnitudes of change without direction. Speed is the useful reduction of velocity when you only care about "how fast," not "how fast in what direction."

## Real-World Applications
- **Speed limits and traffic enforcement:** radar guns measure the scalar speed of approaching vehicles (they don't care about lateral motion).
- **Speedometers:** your car displays speed in mph or km/h — no direction, just magnitude.
- **Internet/data throughput:** "download speed" is bits per second, a magnitude with no vector component.
- **Robotics:** maximum joint speed constraints in a robot arm's motion planner are scalar limits — the controller ensures no joint exceeds its speed limit regardless of direction.

## Intuition
Speed is the number on your car's speedometer. You can be driving north or south at 60 mph — the speedometer reads 60 either way. Velocity would distinguish +60 (north) from -60 (south). Speed is just 60.

## Derivation
Speed is defined as the magnitude of the velocity vector. In one dimension:
$$s = |v_x|$$
In two or three dimensions:
$$s = |\vec{v}| = \sqrt{v_x^2 + v_y^2 + v_z^2}$$
Average speed uses [[Distance]] (not displacement):
$$\bar{s} = \frac{d_{\text{total}}}{\Delta t} = \frac{\int |v(t)|\, dt}{\Delta t}$$
This differs from $|\bar{v}|$ (magnitude of average velocity) whenever direction reverses during the interval.

## Worked Example
A ball is thrown upward at $20\text{ m/s}$, rises, then falls back.

**Step 1 — Upward leg (say it takes 2 s to reach the top):**
Speed at launch: $|v| = 20\text{ m/s}$. Velocity: $v = +20\text{ m/s}$ (upward = positive).

**Step 2 — At the peak:**
$v = 0\text{ m/s}$, so speed $= 0\text{ m/s}$.

**Step 3 — Falling back (say it returns to starting height in another 2 s with speed 20 m/s):**
Velocity: $v = -20\text{ m/s}$ (downward). Speed: $|{-20}| = 20\text{ m/s}$.

**Step 4 — Average speed for the whole trip (total distance 40 m in 4 s):**
$$\bar{s} = \frac{40}{4} = 10\text{ m/s}$$

**Step 5 — Average velocity for the whole trip (displacement = 0, since it returned):**
$$\bar{v} = \frac{0}{4} = 0\text{ m/s}$$

Average speed $\neq$ magnitude of average velocity when direction reverses.

## See Also
- [[Velocity]] — the vector form; speed is its magnitude
- [[Distance]] — average speed = distance / time
- [[Displacement]] — average velocity = displacement / time; different from average speed
- [[Acceleration]] — change in speed over time (but also includes directional change)
