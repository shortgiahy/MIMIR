# Distance

**One-liner:** Distance is the total length of the path an object actually traveled, with no regard for direction.

## Core Idea
$$d = \text{total path length} \geq |\Delta x|$$
Distance is a scalar — it is always non-negative and it accumulates regardless of direction changes. It equals the magnitude of [[Displacement]] only when motion is in one direction with no backtracking.

## Why It Exists
Many real problems care about *how much ground was covered*, not net position change. Fuel consumption, wear on tires, and odometer readings all depend on distance, not displacement. We need both concepts to fully describe motion.

## Real-World Applications
- **Odometers:** your car's odometer measures distance traveled, not displacement from your driveway.
- **Running/fitness trackers:** steps logged are distance; the "as the crow flies" GPS result is displacement.
- **Robotics:** a robot wheel encoder counts total rotations (distance), and the software integrates direction over time to compute displacement for localization.
- **Cable management:** when routing a wire around obstacles, you need to order the correct *length* of cable — that's distance, not displacement.

## Intuition
If displacement is the straight arrow drawn from start to finish on a map, distance is the length of the actual winding road you drove. You can have zero displacement (you drove in a circle and came home) but a distance of many kilometers.

## Derivation
Distance is defined as the arc length of the path. In calculus terms:
$$d = \int_{t_i}^{t_f} |v(t)|\, dt$$
The absolute value is crucial — it prevents forward and backward motion from canceling. Compare this to the displacement formula:
$$\Delta x = \int_{t_i}^{t_f} v(t)\, dt$$
without the absolute value. In the displacement integral, a negative velocity (moving backward) subtracts from the total, reflecting the actual position change. In the distance integral, every bit of motion adds.

## Worked Example
A runner goes 400 m north, then 150 m south.

**Step 1 — Calculate displacement:**
$$\Delta x = +400 + (-150) = +250\text{ m north}$$

**Step 2 — Calculate distance:**
$$d = 400 + 150 = 550\text{ m}$$
(No negatives — we just add up all the path segments.)

**Step 3 — Confirm inequality:**
$$d = 550\text{ m} \geq |\Delta x| = 250\text{ m} \checkmark$$

## See Also
- [[Displacement]] — the vector counterpart; always ≤ distance
- [[Speed]] — distance per unit time; the scalar version of [[Velocity]]
- [[Velocity]] — rate of change of displacement, not distance
- [[Integral]] — distance is computed from |v(t)| via integration
