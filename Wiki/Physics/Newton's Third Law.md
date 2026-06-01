# Newton's Third Law

**One-liner:** For every force that object A exerts on object B, object B simultaneously exerts a force on A that is equal in magnitude and opposite in direction.

## Core Idea
$$\vec{F}_{A\text{ on }B} = -\vec{F}_{B\text{ on }A}$$
Action-reaction pairs always involve *two different objects* — they never act on the same object, which is why they never cancel each other out in a [[Net Force]] calculation. This is the most misunderstood law in introductory physics.

## Why It Exists
The Third Law expresses conservation of [[Momentum]]: if $A$ and $B$ exert equal and opposite forces on each other, the impulses they exchange are equal and opposite, so the total momentum of the system never changes. The Third Law and [[Conservation of Momentum]] are two faces of the same truth.

## Real-World Applications
- **Rocket propulsion:** the engine pushes exhaust gas backward; the exhaust gas pushes the rocket forward (reaction). No external surface needed — this is why rockets work in space.
- **Walking:** your foot pushes backward on the ground (action); the ground pushes forward on your foot (reaction) — that reaction is what propels you forward.
- **Swimming:** hands push water backward; water pushes swimmer forward.
- **Robotics (Baymax):** when a robot gripper squeezes an object, the gripper exerts a force on the object *and* the object pushes back on the gripper with equal force — this reaction force must be accounted for in joint torque calculations.
- **Recoil:** a gun exerts a forward force on the bullet; the bullet exerts a backward force on the gun — recoil.

## Intuition
The most important thing to burn into memory: **action-reaction pairs act on DIFFERENT objects**. When you push a wall, the wall pushes back on *you*, not on itself. The forces never appear in the same free-body diagram (unless you're analyzing both objects together as a system). The reason you don't fly backward when you push a wall is [[Newton's First Law]] + friction, not because the forces cancel — they're on different objects.

**Common misconception:** "If the reaction force is equal and opposite, why does anything ever accelerate?" Answer: because each object experiences *different* net forces. A bug hits a truck windshield: the bug exerts exactly as much force on the truck as the truck exerts on the bug. But the truck is millions of times more massive, so its acceleration is negligible. The bug accelerates enormously.

## Derivation
The Third Law follows from the requirement that isolated systems conserve [[Momentum]]. Consider objects A and B interacting with no external forces:
$$\frac{d\vec{p}_{\text{total}}}{dt} = \frac{d\vec{p}_A}{dt} + \frac{d\vec{p}_B}{dt} = \vec{F}_{B\text{ on }A} + \vec{F}_{A\text{ on }B}$$
For total momentum to be conserved (which experiment confirms), this sum must be zero:
$$\vec{F}_{B\text{ on }A} + \vec{F}_{A\text{ on }B} = 0 \implies \vec{F}_{A\text{ on }B} = -\vec{F}_{B\text{ on }A}$$
Thus the Third Law is equivalent to momentum conservation for two-body interactions.

## Worked Example
A 60 kg person (A) stands on a 200 kg boat (B), floating frictionlessly. They push off the boat's wall with 120 N for 0.5 s. Find velocity of each after the push.

**Step 1 — Identify the third-law pair:**
Person pushes boat: $\vec{F}_{A\text{ on }B} = +120\text{ N}$ (say, rightward).
Boat pushes person: $\vec{F}_{B\text{ on }A} = -120\text{ N}$ (leftward). Same magnitude, opposite direction, different objects.

**Step 2 — Impulse on person (using [[Impulse-Momentum Theorem]]):**
$$J_{\text{person}} = (-120)(0.5) = -60\text{ N·s}$$
$$v_{\text{person}} = \frac{J}{m} = \frac{-60}{60} = -1\text{ m/s (leftward)}$$

**Step 3 — Impulse on boat:**
$$J_{\text{boat}} = (+120)(0.5) = +60\text{ N·s}$$
$$v_{\text{boat}} = \frac{60}{200} = +0.3\text{ m/s (rightward)}$$

**Step 4 — Check momentum conservation:**
Initial total momentum = 0. Final: $60(-1) + 200(0.3) = -60 + 60 = 0\text{ kg·m/s} \checkmark$

## See Also
- [[Newton's Second Law]] — the law each object obeys individually
- [[Net Force]] — reaction forces do NOT appear in an object's own net force (they're on the other object)
- [[Momentum]] — Third Law is equivalent to conservation of momentum for two-body systems
- [[Impulse]] — equal-and-opposite forces over the same time interval give equal-and-opposite impulses
- [[Conservation of Momentum]] — the system-level consequence of the Third Law
