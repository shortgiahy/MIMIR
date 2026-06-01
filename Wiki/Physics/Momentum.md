# Momentum

**One-liner:** Momentum is the product of an object's mass and velocity — the quantity that measures how much "motion" an object carries and how hard it is to stop.

## Core Idea
$$\vec{p} = m\vec{v}$$
Momentum is a vector: it has both magnitude and direction. Its SI unit is kg·m/s. But more fundamentally, Newton's original Second Law was stated as:
$$\vec{F}_{\text{net}} = \frac{d\vec{p}}{dt}$$
This is more general than $F = ma$ — it applies even when mass changes (rockets burning fuel, a chain piling up, a raindrop collecting water). The familiar $F = ma$ is the special case where $m$ is constant.

## Why It Exists
Newton needed a quantity that captures "quantity of motion" — something that persists and transfers during interactions. A bowling ball and a ping-pong ball moving at the same speed are not equivalent; the bowling ball is far harder to stop. Momentum captures exactly that. It is also the quantity that is *conserved* in isolated systems, making it the key to analyzing collisions and explosions without knowing the internal forces.

## Real-World Applications
- **Collisions:** car crash analysis, billiard balls, asteroid impacts — momentum bookkeeping determines post-collision velocities without needing force details.
- **Rocket propulsion:** a rocket expels mass (exhaust) at high velocity to gain momentum in the opposite direction. The thrust equation $F = v_{\text{exhaust}}\,(dm/dt)$ is Newton's Second Law in $F = dp/dt$ form — $F = ma$ fails here because mass is changing.
- **Particle physics:** in a particle accelerator, physicists track momentum (not just velocity) of fragments after collisions to reconstruct what particles were created.
- **Sports:** a heavier pitcher's ball carries more momentum than a lighter ball at the same speed — it resists deflection from the bat more. A sprinter's momentum at the finish line determines how hard it is to decelerate to a stop.
- **Robotics:** momentum must be managed in robotic arms during fast moves — sudden stops create impulsive reaction forces at joints that can damage hardware.

## Intuition
Momentum is "how much it takes to stop something." A bullet has small mass but enormous velocity — its momentum is deadly. A loaded freight train has moderate velocity but enormous mass — its momentum is nearly impossible to stop quickly. Both cases require a large impulse (force × time) to change.

A useful reframe: while energy is a scalar "bank balance," momentum is a vector "debt" — it must be balanced across a system. When two skaters push off each other, their momenta must sum to zero (if they started at rest). No momentum is created or destroyed.

**Connection to energy:** $K = \frac{1}{2}mv^2 = \frac{p^2}{2m}$. This means at fixed momentum, lighter objects have more kinetic energy. A bullet and a gun have equal and opposite momenta after firing, but the bullet has vastly more kinetic energy because $K = p^2/(2m)$ and the bullet has tiny $m$.

## Derivation
**Why $F = dp/dt$ is more fundamental than $F = ma$:**

By definition: $\vec{p} = m\vec{v}$.

Differentiate with respect to time (using the product rule):
$$\frac{d\vec{p}}{dt} = \frac{d(m\vec{v})}{dt} = m\frac{d\vec{v}}{dt} + \vec{v}\frac{dm}{dt} = m\vec{a} + \vec{v}\dot{m}$$

When $m$ is constant ($\dot{m} = 0$): $\frac{d\vec{p}}{dt} = m\vec{a}$, recovering $F = ma$.

When $m$ changes (rocket): $F = m\dot{v} + v\dot{m}$ — the extra term $v\dot{m}$ is the thrust due to expelled mass.

**Momentum in 2D:**
Momentum is a vector, so it adds component-wise:
$$\vec{p} = p_x\hat{x} + p_y\hat{y} = mv_x\hat{x} + mv_y\hat{y}$$

Each component is independently governed by $F_x = dp_x/dt$ and $F_y = dp_y/dt$.

## Worked Example
A rocket in space (initially stationary) has mass $m_0 = 500\text{ kg}$ (including 100 kg of fuel). It burns all fuel, expelling it at $v_{\text{exhaust}} = 2000\text{ m/s}$ relative to the rocket. Find the rocket's final speed using momentum conservation.

**Setup:** The system (rocket + exhaust) starts at rest: $\vec{p}_{\text{total}} = 0$.

After all fuel is expelled, let the rocket's final velocity be $v_r$ (forward) and exhaust velocity be $-v_{\text{exhaust}}$ (backward) in the lab frame.

**By conservation of momentum** (no external forces in space):
$$0 = m_{\text{rocket}}\,v_r + m_{\text{exhaust}}\,(-v_{\text{exhaust}})$$
$$0 = (500 - 100)\,v_r - 100 \times 2000$$
$$400\,v_r = 200{,}000$$
$$v_r = 500\text{ m/s}$$

This is a simplified model (constant exhaust speed, all fuel expelled at once); the exact result uses the Tsiolkovsky rocket equation, which integrates $F = dp/dt$ over continuous mass loss.

## See Also
- [[Newton's Second Law]] — $F = dp/dt$ is the general form; $F = ma$ is the constant-mass special case
- [[Newton's Third Law]] — equal and opposite forces mean equal and opposite momentum changes; leads to conservation
- [[Impulse]] — the agent that changes momentum: $J = \Delta p$
- [[Impulse-Momentum Theorem]] — the direct relationship between impulse and momentum change
- [[Conservation of Momentum]] — momentum is constant in an isolated system
- [[Kinetic Energy]] — related by $K = p^2/(2m)$; conserved independently from momentum
- [[Vector]] — momentum is a vector; direction matters as much as magnitude (Math/ML connection)
