# Net Force

**One-liner:** Net force is the single vector you get by adding up all the individual forces acting on an object — it is the only force that determines how the object accelerates.

## Core Idea
$$\vec{F}_{\text{net}} = \sum_i \vec{F}_i = m\vec{a}$$
Multiple forces act on any real object simultaneously. What matters for motion is their vector sum. A 100 N push east and a 100 N push west yield zero net force — the object does not accelerate, just as if no forces existed.

## Why It Exists
Forces come from many sources at once (gravity, friction, the hand pushing, the air resisting). We need a way to collapse all of them into one number that drives [[Newton's Second Law]]. Net force is that collapse. Without it, we would have no systematic way to predict motion when multiple agents act simultaneously.

## Real-World Applications
- **Free-body diagrams (FBD):** every structural and motion analysis starts with isolating an object and summing forces to find net force — bridges, cranes, satellites.
- **Tug of war:** two teams exert forces in opposite directions; the team with greater force wins because net force determines which way the rope (and both teams) accelerates.
- **Robotics:** a robot arm's end-effector experiences gravity, payload weight, joint forces, and contact forces. The controller must ensure net force on each link matches the desired acceleration.
- **Aircraft:** four forces act — thrust (forward), drag (backward), lift (up), weight (down). Level cruise means $F_{\text{net}} = 0$ in all directions.
- **Terminal velocity:** a falling object reaches terminal velocity when air drag exactly cancels gravity, making $F_{\text{net}} = 0$ and $a = 0$.

## Intuition
Imagine each force as a tug on a rope attached to the object. Net force is what you'd feel if all those people pulling simultaneously were replaced by a single person pulling with that combined effect. Only one person's pull matters for predicting the motion — the equivalent single person.

## Derivation
Forces obey superposition (experimentally established): if $\vec{F}_1, \vec{F}_2, \dots, \vec{F}_n$ all act on one object, the resulting [[Acceleration]] is the same as if a single force $\vec{F}_{\text{net}} = \vec{F}_1 + \vec{F}_2 + \dots + \vec{F}_n$ acted alone:
$$\vec{F}_{\text{net}} = \sum_{i=1}^{n} \vec{F}_i = m\vec{a}$$
In component form (2D), this splits into two independent equations:
$$F_{\text{net},x} = \sum F_{ix} = ma_x$$
$$F_{\text{net},y} = \sum F_{iy} = ma_y$$
This is why free-body diagrams resolve forces into $x$ and $y$ components before summing.

## Worked Example
A box (mass 10 kg) on a surface is pushed right with 50 N and experiences 20 N of friction (leftward), with gravity (98 N down) and normal force (98 N up).

**Step 1 — Sum horizontal forces:**
$$F_{\text{net},x} = +50 - 20 = +30\text{ N (rightward)}$$

**Step 2 — Sum vertical forces:**
$$F_{\text{net},y} = +98 - 98 = 0\text{ N}$$

**Step 3 — Apply Newton's Second Law:**
$$a_x = \frac{F_{\text{net},x}}{m} = \frac{30}{10} = 3\text{ m/s}^2\text{ (rightward)}$$
$$a_y = 0\text{ m/s}^2\text{ (doesn't accelerate vertically)}$$

**Step 4 — Interpret:**
The box accelerates at $3\text{ m/s}^2$ to the right. The vertical forces cancel — this is just how objects on flat surfaces behave.

## See Also
- [[Force]] — the individual forces that are summed
- [[Newton's Second Law]] — $\vec{F}_{\text{net}} = m\vec{a}$; net force is its direct input
- [[Newton's First Law]] — zero net force means zero acceleration (constant velocity)
- [[Acceleration]] — net force divided by mass gives acceleration
- [[Newton's Third Law]] — explains why reaction forces do NOT appear in the net force of the object
