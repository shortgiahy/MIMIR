# Displacement

**One-liner:** Displacement is the straight-line vector from where you started to where you ended up, regardless of the path you took.

## Core Idea
$$\Delta x = x_f - x_i$$
Displacement tells you *how far and in what direction* an object moved. It is a vector — sign (or direction in 2D/3D) matters. If you walk forward 5 m then back 5 m, your displacement is zero even though you traveled 10 m.

## Why It Exists
Physics needs to track *where* things end up, not just how much ground they covered. Without displacement, we could not write equations of motion or define [[Velocity]]. Distance alone loses information about direction, which is fatal when predicting collisions, trajectories, or any vector-dependent outcome.

## Real-World Applications
- **GPS navigation:** your route may wind, but displacement gives the straight-line vector from start to destination — bearing and magnitude for dead-reckoning.
- **Robotics (odometry):** Baymax's wheels track total path length, but the robot's internal map updates using displacement vectors to know *where it actually is* in the room.
- **Structural engineering:** beam deflection under load is measured as displacement from rest position, not total path of vibration.
- **Sports analytics:** a soccer player's sprint is tracked as displacement from kick-off position for tactical analysis.

## Intuition
Imagine drawing a straight arrow from your starting dot to your ending dot on a map. That arrow — length and direction — is your displacement. The winding road you actually walked is [[Distance]]. The arrow doesn't care about the road.

## Derivation
Displacement is defined, not derived. In one dimension:
$$\Delta x = x_f - x_i$$
In vector form (2D or 3D):
$$\vec{\Delta r} = \vec{r}_f - \vec{r}_i = (x_f - x_i)\hat{i} + (y_f - y_i)\hat{j} + (z_f - z_i)\hat{k}$$
This is simply the tip-minus-tail rule for vector subtraction. It is the foundation from which [[Velocity]] is built via the [[Derivative]]:
$$\vec{v} = \frac{d\vec{r}}{dt}$$

## Worked Example
A car starts at position $x_i = 20\text{ m}$ (east of a reference point) and drives to $x_f = -5\text{ m}$ (west of the reference point).

**Step 1 — Apply the definition:**
$$\Delta x = x_f - x_i = -5 - 20 = -25\text{ m}$$

**Step 2 — Interpret the sign:**
The negative sign means displacement is 25 m to the *west* (our positive direction was east).

**Step 3 — Note what we do NOT know:**
We don't know the total [[Distance]] traveled — the car might have gone east first then doubled back. Displacement only records net change.

## See Also
- [[Distance]] — the scalar path length; contrast with displacement
- [[Velocity]] — displacement per unit time; built directly on this concept
- [[Kinematic Equations]] — all four use Δx as the displacement variable
- [[Derivative]] — instantaneous velocity is the derivative of displacement with respect to time
- [[Vector]] — displacement is the canonical example of a vector quantity in intro physics
