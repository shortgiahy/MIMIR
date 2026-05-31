# Newton's Second Law

**One-liner:** The net force on an object equals its mass times its acceleration — this is the bridge between the cause of motion (force) and the description of motion (acceleration).

## Why It Exists

Kinematics describes *how* objects move. But it raises an obvious question: what actually *causes* acceleration? A ball rolling on flat ground slows down. A ball rolling off a table curves downward. Why?

Newton's Second Law is the answer. It says: wherever you see acceleration, there is a net force causing it — and the relationship is linear and proportional. Double the force, double the acceleration. Double the mass, halve the acceleration. Published in *Principia Mathematica* (1687), this law unified terrestrial mechanics and planetary motion under one framework for the first time in history.

The deeper reason it matters for you: **forces are how physics connects to real-world situations** — tension in ropes, normal forces from surfaces, gravity, friction. To find *what happens* to an object, you identify all forces, find the net force, and divide by mass to get acceleration. Then [[Kinematics - 1D Motion]] takes it from there.

## The Concept

### Newton's First Law First (The Setup)

Newton's First Law says: an object at rest stays at rest, and an object in motion stays in motion at constant velocity, *unless acted upon by a net external force*. This defines the meaning of force: a force is whatever changes an object's state of motion.

This law also defines **inertial reference frames** — frames where the first law actually holds (i.e., not accelerating frames like a spinning room). All of Newton's laws are valid only in inertial frames.

### Defining Mass

**Mass** is the measure of an object's resistance to acceleration — its *inertia*. It is not the same as weight. Mass is an intrinsic property of matter; weight is a force that depends on the local gravitational field.

Two identical objects pushed by the same force will accelerate identically. An object with twice the mass accelerates at half the rate under the same force. This is operationally *how mass is defined* in Newtonian mechanics: compare accelerations under identical forces.

### The Second Law

If the net (total) force on an object of mass $m$ is $\vec{F}_\text{net}$, then:

$$\vec{F}_\text{net} = m\vec{a}$$

This is a vector equation. In 1D it's a scalar with sign. In 2D/3D you apply it *component by component*:

$$F_{\text{net},x} = ma_x, \quad F_{\text{net},y} = ma_y$$

**What "net" means:** You must add up *all* forces acting on the object as vectors before setting equal to $ma$. A $10\ \text{N}$ push right and a $10\ \text{N}$ friction force left give $F_\text{net} = 0$, so $a = 0$. The object doesn't accelerate even though two real forces are present.

### Where Does This Come From? (A Deeper Justification)

Newton's original formulation was actually in terms of momentum: $\vec{F}_\text{net} = \frac{d\vec{p}}{dt}$, where $\vec{p} = m\vec{v}$ is momentum. For constant mass, $\frac{d(m\vec{v})}{dt} = m\frac{d\vec{v}}{dt} = m\vec{a}$, recovering $F = ma$.

The momentum form is more general — it handles cases where mass changes (like rockets burning fuel). The $F = ma$ form is the constant-mass special case, which covers virtually all Physics 1 problems.

### The Free Body Diagram (FBD)

The free body diagram is the single most important problem-solving tool in Newton's law problems. It is a picture of:
- Just the object (nothing else)
- Every force acting *on* that object, drawn as arrows from the object's center
- Each force labeled with its type and direction

Forces on an FBD include:
- **Weight** ($W = mg$, downward — not "gravity" in general, but the specific gravitational force near Earth's surface)
- **Normal force** ($N$, perpendicular to the contact surface, pushing away from it)
- **Tension** ($T$, along a rope or cable, pulling toward the attachment)
- **Friction** ($f$, opposing the direction of sliding or attempted sliding)
- **Applied force** (explicitly given in the problem)
- **Spring force** ($F = -kx$, see [[Hooke's Law]])

Forces that are *not* on an FBD: forces the object exerts on other things (Newton's Third Law pairs belong on the other object's FBD).

### Newton's Third Law (The Companion)

For every force object A exerts on object B, object B exerts an equal and opposite force on object A. The two forces in a Third Law pair are:
- Equal in magnitude
- Opposite in direction
- Acting on *different* objects

This means Third Law pairs never appear together on the same FBD. The floor pushes up on you (normal force, on *you*). You push down on the floor (reaction, on the *floor*). These two never cancel each other because they act on different objects.

### Applying the Second Law: A System

**Step 1:** Identify the object (or system) you're analyzing.

**Step 2:** Draw the FBD — every force on that object, with direction.

**Step 3:** Choose a coordinate system. Align an axis with the acceleration direction when possible.

**Step 4:** Write $\sum F_x = ma_x$ and $\sum F_y = ma_y$ (the $\sum$ means "sum of all").

**Step 5:** Solve the algebra.

A key case: when $a = 0$, the object is in **static or dynamic equilibrium** and $\sum F = 0$. The forces balance. This doesn't mean no forces — it means they cancel.

### Units

The SI unit of force is the **Newton**: $1\ \text{N} = 1\ \text{kg} \cdot \text{m/s}^2$.

This unit is self-consistent with $F = ma$: if $m = 1\ \text{kg}$ and $a = 1\ \text{m/s}^2$, then $F = 1\ \text{N}$.

Weight: $W = mg$, so a $70\ \text{kg}$ person weighs $70 \times 9.8 = 686\ \text{N}$ on Earth.

## Intuition

Imagine pushing a shopping cart (light) vs. a car (heavy). Same push, different accelerations. The car's resistance — the thing that makes it hard to accelerate — is its mass.

Now imagine pushing the cart with two friends pushing the opposite way. The cart barely moves (small net force, small acceleration). Each individual force matters less than the *vector sum*.

The Second Law is really just: **the net push determines the change in motion, and mass is how much the object resists that change**. That's it.

For free body diagrams: the object doesn't care about your intentions. It only "feels" the actual contact forces and field forces on it. Drawing the FBD is the process of listing everything the object actually feels.

## Key Formula / Rule

$$\vec{F}_\text{net} = m\vec{a}$$

Applied component-wise:

$$\sum F_x = ma_x \qquad \sum F_y = ma_y$$

Weight near Earth's surface:

$$W = mg \quad (g = 9.80\ \text{m/s}^2, \text{ downward})$$

## Worked Example

**Problem:** A $5.0\ \text{kg}$ box sits on a frictionless surface. A horizontal rope pulls it with $20\ \text{N}$ to the right. What is its acceleration?

**Step 1 — Free body diagram.**

Forces on the box:
- Weight: $W = mg = (5.0)(9.8) = 49\ \text{N}$, downward
- Normal force: $N$, upward (from surface)
- Tension: $T = 20\ \text{N}$, rightward

**Step 2 — Apply Newton's Second Law by component.**

Vertical ($y$-direction): The box doesn't accelerate vertically ($a_y = 0$).

$$\sum F_y = N - W = 0 \implies N = W = 49\ \text{N}$$

The surface pushes up exactly as hard as gravity pulls down. This always happens on a flat surface when there's no vertical acceleration.

Horizontal ($x$-direction): Only the tension acts horizontally.

$$\sum F_x = T = ma_x$$

$$20\ \text{N} = (5.0\ \text{kg})\, a_x$$

$$a_x = \frac{20}{5.0} = 4.0\ \text{m/s}^2$$

**Answer:** The box accelerates at $4.0\ \text{m/s}^2$ to the right.

**Extension:** If the surface had kinetic friction with $\mu_k = 0.30$:

Friction force $= \mu_k N = 0.30 \times 49 = 14.7\ \text{N}$ (opposing motion, so leftward).

$$\sum F_x = 20 - 14.7 = 5.3\ \text{N} = ma_x$$

$$a_x = \frac{5.3}{5.0} = 1.06 \approx 1.1\ \text{m/s}^2$$

## Gotchas

**1. Net force, not individual forces.** $F = ma$ uses *net* force. If you plug in one of several forces without adding the others, you get a wrong acceleration. Always draw the FBD first.

**2. Mass and weight are different.** Mass is $m$ (kg), weight is $W = mg$ (N). Saying "the object weighs 5 kg" is technically wrong in physics — it has a mass of 5 kg and a weight of 49 N.

**3. Normal force is not always $mg$.** The normal force equals $mg$ only on a flat horizontal surface with no vertical acceleration. On an incline, in an elevator, or with an applied force at an angle, $N \neq mg$.

**4. Newton's Third Law pairs do not cancel.** They act on *different* objects. The reason they don't cancel in $\sum F = ma$ is that $\sum F$ only includes forces on the *one* object you're analyzing.

**5. Acceleration can be in the opposite direction to velocity.** A decelerating car has velocity forward and acceleration backward. This is fine — the Second Law governs acceleration, not velocity.

**6. "No force" and "no net force" are different.** "No force" is impossible (gravity always acts). "No net force" means all forces cancel and $a = 0$. This is the equilibrium condition.

## See Also

- [[Kinematics - 1D Motion]] — once you have $a$ from Newton's Second Law, kinematics tells you what happens to position and velocity over time
- [[Work and Kinetic Energy]] — derives the work-energy theorem directly from $F = ma$ and kinematics; an alternative method for solving many force problems
- [[Impulse and Momentum]] — Newton's original formulation; $F = \Delta p / \Delta t$ is more general than $F = ma$
- [[Conservation of Energy]] — energy methods are often faster than force methods when the path is complicated; knowing when to switch is key
- [[Calculus - Derivatives]] — acceleration is $dv/dt$, force is $dp/dt$; Newton's law is fundamentally a differential equation
