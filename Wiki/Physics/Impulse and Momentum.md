# Impulse and Momentum

**One-liner:** Momentum is an object's "quantity of motion" ($p = mv$); impulse is the force acting over a time interval that changes it; together they form the impulse-momentum theorem, and momentum is conserved in any isolated system.

## Why It Exists

[[Newton's Second Law]] in the form $F = ma$ is powerful, but it struggles with one category of problem: **short-duration collisions and impacts**. When a bat hits a baseball, the contact time is about $1\ \text{ms}$. The force varies wildly throughout that millisecond — it spikes at contact and drops back to zero. Nobody can measure that force profile precisely.

Yet we can measure exactly what happens to the ball: it comes in at 40 m/s and leaves at 50 m/s in the opposite direction. We know the mass. That means we know the *change in motion* precisely, even though we don't know the details of the force.

Momentum and impulse are the tools that let you work with *changes* in motion over *time intervals*, rather than forces at every instant. Newton himself formulated his Second Law this way: $\vec{F} = d\vec{p}/dt$, not $F = ma$. The $F = ma$ version is the special case when mass is constant.

Beyond collisions, momentum leads to **conservation of momentum**, which is arguably the most useful conservation law in all of physics. In any collision or explosion between objects with no outside forces, the total momentum before equals the total momentum after — regardless of how complicated the interaction is inside.

## The Concept

### Linear Momentum

The **linear momentum** of an object is:

$$\vec{p} = m\vec{v}$$

Momentum is a vector — it has both magnitude and direction, just like velocity. Its SI unit is $\text{kg} \cdot \text{m/s}$.

Mass and velocity both matter. A $10\ \text{kg}$ object at $1\ \text{m/s}$ has the same momentum as a $1\ \text{kg}$ object at $10\ \text{m/s}$: both have $p = 10\ \text{kg} \cdot \text{m/s}$. But they have very different kinetic energies ($5\ \text{J}$ vs. $50\ \text{J}$).

This illustrates that momentum and energy are different physical quantities. You need both concepts because they answer different questions.

### Newton's Second Law as a Momentum Statement

Newton's original formulation:

$$\vec{F}_\text{net} = \frac{d\vec{p}}{dt}$$

For constant mass:

$$\frac{d\vec{p}}{dt} = \frac{d(m\vec{v})}{dt} = m\frac{d\vec{v}}{dt} = m\vec{a}$$

So $\vec{F} = m\vec{a}$ is a *special case*. The momentum form is more general: it handles rocket propulsion (where mass changes as fuel burns), for instance, without any modification.

The key message: **net force is the rate of change of momentum**. If there is no net force, momentum does not change. This is the seed of conservation of momentum.

### Impulse

The **impulse** $\vec{J}$ delivered by a force over a time interval $[t_1, t_2]$ is:

$$\vec{J} = \int_{t_1}^{t_2} \vec{F}(t)\, dt$$

For a constant force, this simplifies to:

$$\vec{J} = \vec{F}\, \Delta t$$

Units: $\text{N} \cdot \text{s} = \text{kg} \cdot \text{m/s}$ — same as momentum. This is not a coincidence.

If the force varies, impulse is the *area under the $F$-vs-$t$ graph*. This is the same kind of integral thinking from [[Calculus - Integrals]] and [[Work and Kinetic Energy]] — but now integrating over time instead of distance.

### The Impulse-Momentum Theorem

Integrate Newton's Second Law $\vec{F} = d\vec{p}/dt$ over a time interval:

$$\int_{t_1}^{t_2} \vec{F}\, dt = \int_{t_1}^{t_2} \frac{d\vec{p}}{dt}\, dt = \vec{p}_f - \vec{p}_i = \Delta\vec{p}$$

The left side is impulse. The right side is the change in momentum. Therefore:

$$\boxed{\vec{J} = \Delta\vec{p}}$$

**Impulse equals the change in momentum.** This is the impulse-momentum theorem. It is not a new law — it is a direct consequence of Newton's Second Law, obtained by integrating over time rather than over distance (which gave us the work-energy theorem).

Compare the two:

| Theorem | Obtained by | Statement |
|---------|-------------|-----------|
| Work-energy | Integrate $F = ma$ over *distance* | $W = \Delta K$ |
| Impulse-momentum | Integrate $F = ma$ over *time* | $J = \Delta p$ |

They're parallel. Both come from the same source. They give different information about the same event.

### The Average Force Concept

For a collision with a known impulse and known duration, you can find the average force:

$$\vec{F}_\text{avg} = \frac{\Delta\vec{p}}{\Delta t}$$

You don't need to know the force profile — just the total momentum change and the duration. This is how engineers design car crumple zones: increasing the collision time $\Delta t$ while keeping $\Delta p$ fixed reduces the average force on the passenger.

### Conservation of Momentum

Consider two objects that interact (collide, explode) with only internal forces between them — no outside forces. By Newton's Third Law, every force that object 1 exerts on object 2 has an equal-and-opposite reaction from object 2 on object 1. These internal forces cancel in the total:

$$\vec{F}_{1\to2} + \vec{F}_{2\to1} = 0$$

By the impulse-momentum theorem, the total impulse on the system is zero, so:

$$\Delta\vec{p}_\text{total} = 0$$

$$\vec{p}_{1,i} + \vec{p}_{2,i} = \vec{p}_{1,f} + \vec{p}_{2,f}$$

$$m_1\vec{v}_{1,i} + m_2\vec{v}_{2,i} = m_1\vec{v}_{1,f} + m_2\vec{v}_{2,f}$$

This is **conservation of linear momentum**. The condition is: no net external force on the system during the interaction. During brief collisions, external forces like friction are often small enough compared to the collision force that they can be ignored — the "impulse approximation."

### Types of Collisions

Conservation of momentum applies to *all* collisions. Whether kinetic energy is also conserved depends on the type:

**Elastic collision:** Both momentum *and* kinetic energy are conserved. Perfectly elastic collisions occur in atomic/molecular interactions. Billiard balls are approximately elastic.

$$m_1\vec{v}_{1,i} + m_2\vec{v}_{2,i} = m_1\vec{v}_{1,f} + m_2\vec{v}_{2,f}$$
$$\frac{1}{2}m_1v_{1,i}^2 + \frac{1}{2}m_2v_{2,i}^2 = \frac{1}{2}m_1v_{1,f}^2 + \frac{1}{2}m_2v_{2,f}^2$$

Two equations, two unknowns. Solvable.

**Perfectly inelastic collision:** Momentum is conserved. Kinetic energy is *not* conserved — some is lost to heat, deformation, sound. The objects stick together after collision:

$$m_1v_{1,i} + m_2v_{2,i} = (m_1 + m_2)v_f$$

This gives the maximum loss of kinetic energy (though not all KE is lost — that would violate momentum conservation in the center-of-mass frame, unless the total initial momentum is also zero).

**Inelastic (but not perfectly):** Momentum conserved, kinetic energy not. Objects don't stick. Most real collisions are in this category.

### Explosions

An explosion is a collision in reverse: one object at rest splits into pieces. Total initial momentum is zero (or some value), and the pieces fly apart such that their momenta sum to the initial value. A rifle and bullet: before firing, total momentum is zero. After: the bullet goes one way, the rifle recoils the other, with equal and opposite momenta.

### Momentum in 2D

Momentum is a vector, so in 2D problems you apply conservation separately for $x$ and $y$ components:

$$m_1 v_{1x,i} + m_2 v_{2x,i} = m_1 v_{1x,f} + m_2 v_{2x,f}$$
$$m_1 v_{1y,i} + m_2 v_{2y,i} = m_1 v_{1y,f} + m_2 v_{2y,f}$$

Glancing collisions (where objects don't hit head-on) require this 2D treatment.

## Intuition

Think of a bowling ball and a ping-pong ball. Throw the ping-pong ball at the stationary bowling ball — the bowling ball barely moves. Now throw the bowling ball at the stationary ping-pong ball — the ping-pong ball flies off. Same speed before impact, very different results, because momentum ($mv$) is very different.

Impulse is the "push over time" needed to change momentum. A small force over a long time can change momentum just as much as a large force over a short time. This is why following through on a golf swing matters — more contact time, more impulse, more change in the ball's momentum.

Crumple zones: the car's momentum goes from $mv$ to zero in a crash. $\Delta p$ is fixed by the car's speed and mass. A rigid car does that in $0.01\ \text{s}$: average force = $\Delta p / 0.01$ — enormous. A car with crumple zones does it in $0.10\ \text{s}$: average force = $\Delta p / 0.10$ — ten times smaller. Same impulse, ten times safer.

## Key Formula / Rule

Linear momentum:
$$\vec{p} = m\vec{v}$$

Impulse (constant force):
$$\vec{J} = \vec{F}\,\Delta t$$

Impulse (variable force):
$$\vec{J} = \int_{t_1}^{t_2} \vec{F}(t)\, dt$$

Impulse-momentum theorem:
$$\vec{J} = \Delta\vec{p} = \vec{p}_f - \vec{p}_i$$

Conservation of momentum (isolated system):
$$\vec{p}_{\text{total},i} = \vec{p}_{\text{total},f}$$

Average force from impulse:
$$\vec{F}_\text{avg} = \frac{\Delta\vec{p}}{\Delta t}$$

## Worked Example

**Problem:** A $0.145\ \text{kg}$ baseball traveling at $40.0\ \text{m/s}$ toward a batter is hit and leaves the bat at $55.0\ \text{m/s}$ in the opposite direction. The contact time is $1.20\ \text{ms}$ = $0.00120\ \text{s}$. Find: (a) the impulse delivered to the ball, and (b) the average force exerted by the bat on the ball.

**Step 1 — Define positive direction.**

Let positive = direction the ball is hit (away from batter). Then:
- $v_i = -40.0\ \text{m/s}$ (ball coming toward batter is negative)
- $v_f = +55.0\ \text{m/s}$ (ball leaves toward outfield)

**Step 2 — Find change in momentum (= impulse).**

$$\Delta p = m v_f - m v_i = m(v_f - v_i)$$

$$\Delta p = (0.145)(55.0 - (-40.0)) = (0.145)(95.0)$$

$$\Delta p = 13.775\ \text{kg} \cdot \text{m/s} \approx 13.8\ \text{kg} \cdot \text{m/s}$$

The sign is positive, meaning the impulse is in the direction the ball was hit.

**Step 3 — Find average force.**

$$F_\text{avg} = \frac{\Delta p}{\Delta t} = \frac{13.8}{0.00120} = 11{,}500\ \text{N}$$

That is about $11.5\ \text{kN}$ — roughly 80 times the ball's weight. This is why a correctly timed swing matters so much more than swing "strength" in the traditional sense.

**Part B worked in reverse:** If we're told $F_\text{avg} = 11{,}500\ \text{N}$ and $\Delta t = 1.20\ \text{ms}$:
$$J = F_\text{avg}\,\Delta t = 11{,}500 \times 0.00120 = 13.8\ \text{kg\cdot m/s} \checkmark$$

---

**Second problem (conservation):** A $1.0\ \text{kg}$ cart moving at $4.0\ \text{m/s}$ east collides with a stationary $3.0\ \text{kg}$ cart. They stick together. What is their final velocity?

This is a perfectly inelastic collision. Conservation of momentum:

$$m_1 v_i = (m_1 + m_2) v_f$$

$$(1.0)(4.0) = (1.0 + 3.0)\, v_f$$

$$4.0 = 4.0\, v_f$$

$$v_f = 1.0\ \text{m/s (east)}$$

**Energy check:** Is kinetic energy conserved?

$$K_i = \frac{1}{2}(1.0)(4.0)^2 = 8.0\ \text{J}$$

$$K_f = \frac{1}{2}(4.0)(1.0)^2 = 2.0\ \text{J}$$

Energy lost = $6.0\ \text{J}$ — went to heat, sound, deformation. Kinetic energy was *not* conserved, as expected for a perfectly inelastic collision. Momentum *was* conserved. These two facts are not contradictory.

## Gotchas

**1. Momentum is a vector; you must account for direction (sign).** The baseball problem above: the ball's initial velocity is *negative* because it's moving toward the batter. If you use $+40$ for initial velocity, you get the wrong impulse — you'd compute $\Delta p = m(55 - 40) = 2.2\ \text{N·s}$ instead of $13.8\ \text{N·s}$. Sign errors here produce huge numerical mistakes.

**2. Kinetic energy is not conserved in most collisions.** Only elastic collisions conserve KE. Perfectly inelastic collisions lose the most KE. Students often try to apply both momentum and energy conservation and get contradictions — that only works for elastic collisions.

**3. Conservation of momentum requires no net external force.** If friction acts on both objects, or if there's an external wall involved, momentum of the two-object system is not conserved. You can still use impulse-momentum for each object individually.

**4. Momentum and energy solve different problems.** Use momentum for collisions and explosions (where forces are internal and unknown). Use energy for questions about speed at different positions, springs, heights. They're complementary tools.

**5. The "impulse approximation."** In a collision, external forces like gravity or friction act during $\Delta t$, but if $\Delta t$ is very short, their impulse ($F_\text{ext}\,\Delta t$) is negligible compared to the collision impulse. This is why we can treat most real collisions as if momentum is conserved even though gravity exists.

**6. "Objects stick together" → perfectly inelastic, use combined mass.** "Objects bounce off each other" → use two separate final velocities and two equations (momentum + elastic KE, or momentum alone if inelastic with information about one final velocity).

## See Also

- [[Newton's Second Law]] — Newton's original formulation is $F = dp/dt$; momentum is more fundamental than $F = ma$
- [[Work and Kinetic Energy]] — impulse integrates force over time; work integrates force over distance; they're parallel theorems from Newton's Second Law
- [[Conservation of Energy]] — in elastic collisions, both momentum and energy are conserved; using both gives you all final velocities; in inelastic collisions, only momentum is conserved
- [[Kinematics - 1D Motion]] — after using momentum to find velocities, kinematics takes over if you need to know what happens next (how far each object travels, etc.)
- [[Calculus - Integrals]] — impulse is a definite integral of force over time; the variable-force impulse requires this
- [[Vectors]] — in 2D collision problems, momentum conservation becomes two separate component equations; vector decomposition is essential
