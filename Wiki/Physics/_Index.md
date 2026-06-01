# Physics — Index

Navigation hub for all Physics atomic notes. Each concept lives in its own file; this page organizes them by cluster and suggests a study path optimized for calculus-based Physics 1 (MIT/Stanford transfer track).

---

## Concept Clusters

### Kinematics
*How objects move through space and time — before asking why.*

| Note | One-sentence description |
|------|-------------------------|
| [[Displacement]] | The straight-line vector from start to finish; direction matters. |
| [[Distance]] | Total path length traveled; always positive, always a scalar. |
| [[Velocity]] | Rate of change of displacement; a vector with direction. |
| [[Speed]] | Magnitude of velocity; a scalar with no direction. |
| [[Acceleration]] | Rate of change of velocity; points in the direction the velocity is changing. |
| [[Kinematic Equations]] | The four SUVAT equations linking displacement, velocity, acceleration, and time under constant acceleration. |

---

### Forces
*Why objects accelerate — Newton's three laws and their direct consequences.*

| Note | One-sentence description |
|------|-------------------------|
| [[Force]] | A push or pull that causes or tends to cause acceleration; a vector. |
| [[Net Force]] | The vector sum of all forces on one object; determines its acceleration. |
| [[Newton's First Law]] | An object stays at rest or constant velocity unless a net force acts on it (law of inertia). |
| [[Newton's Second Law]] | Net force equals rate of change of momentum; $F = ma$ is the constant-mass special case. |
| [[Newton's Third Law]] | Every action force has an equal and opposite reaction force on the *other* object. |

---

### Energy
*What forces accomplish over distance — the scalar bookkeeping of physics.*

| Note | One-sentence description |
|------|-------------------------|
| [[Work]] | Force times displacement (dot product); energy transferred by a force over a distance. |
| [[Kinetic Energy]] | Energy of motion: $K = \frac{1}{2}mv^2$; always non-negative. |
| [[Work-Energy Theorem]] | Net work done on an object equals its change in kinetic energy. |
| [[Gravitational Potential Energy]] | Stored energy due to height in a gravitational field: $U_g = mgh$. |
| [[Elastic Potential Energy]] | Stored energy in a compressed or stretched spring: $U_s = \frac{1}{2}kx^2$. |
| [[Conservation of Energy]] | Total mechanical energy is constant when only conservative forces act. |
| [[Conservative Force]] | A force whose work is path-independent, making potential energy definable. |

---

### Momentum
*What forces accomplish over time — the vector bookkeeping of physics.*

| Note | One-sentence description |
|------|-------------------------|
| [[Momentum]] | Product of mass and velocity: $\vec{p} = m\vec{v}$; measures quantity of motion. |
| [[Impulse]] | Force integrated over time: $\vec{J} = \int \vec{F}\,dt$; the agent that changes momentum. |
| [[Impulse-Momentum Theorem]] | Net impulse equals change in momentum: $\vec{J} = \Delta\vec{p}$; the time-domain counterpart to Work-Energy. |
| [[Conservation of Momentum]] | Total momentum of an isolated system is constant; follows from Newton's Third Law. |

---

## Suggested Study Order

This sequence builds each concept on the previous — no gaps, no circular dependencies.

### Phase 1 — Motion Without Forces (Kinematics)
1. [[Distance]] and [[Displacement]] — scalar vs. vector distinction; sets up everything.
2. [[Speed]] and [[Velocity]] — first derivatives; introduce vector direction.
3. [[Acceleration]] — second derivative; the bridge to forces.
4. [[Kinematic Equations]] — toolkit for constant-acceleration problems; drill until automatic.

### Phase 2 — Why Things Move (Forces)
5. [[Force]] — what it is; free-body diagrams.
6. [[Net Force]] — vector addition; the actual input to Newton's Second Law.
7. [[Newton's First Law]] — inertia; the zero-force baseline.
8. [[Newton's Second Law]] — $\vec{F} = d\vec{p}/dt$; the master equation.
9. [[Newton's Third Law]] — action-reaction; sets up momentum conservation.

### Phase 3 — Energy Domain
10. [[Work]] — force over displacement; dot product definition.
11. [[Kinetic Energy]] — what work produces.
12. [[Work-Energy Theorem]] — derived bridge between force domain and energy domain.
13. [[Conservative Force]] — path independence; why potential energy is definable.
14. [[Gravitational Potential Energy]] — the most common potential energy; $U = mgh$.
15. [[Elastic Potential Energy]] — springs; $U = \frac{1}{2}kx^2$.
16. [[Conservation of Energy]] — the full energy accounting law.

### Phase 4 — Momentum Domain
17. [[Momentum]] — $\vec{p} = m\vec{v}$; why $F = dp/dt$ is more fundamental than $F = ma$.
18. [[Impulse]] — force over time; the momentum-change agent.
19. [[Impulse-Momentum Theorem]] — derived bridge between force domain and momentum domain.
20. [[Conservation of Momentum]] — the system-level consequence of Newton's Third Law.

---

## Cross-Subject Connection Table

Physics connects deeply to other subjects in the wiki. Use these links to reinforce understanding across domains.

| Physics Concept | Math Connection | EE Connection | ML & Robotics Connection |
|----------------|----------------|---------------|--------------------------|
| [[Kinematic Equations]] | [[u-Substitution]] (integrating $a$ to get $v$) | — | — |
| [[Work]] | [[Integration by Parts]], [[Trigonometric Substitution]] ($\int F\cos\theta\,dx$) | [[Voltage]] × [[Electric Charge]] = work done by E-field | — |
| [[Kinetic Energy]] | [[Sequence]], [[Series]] (Taylor expansion of relativistic KE) | Power = $\frac{dK}{dt}$ in motor theory | — |
| [[Conservative Force]] | [[Gradient]] ($\vec{F} = -\nabla U$), [[Divergence]], [[Convergence]] | [[Voltage]] is electric potential energy per charge; Coulomb force is conservative | [[Gradient Descent]] minimizes a loss landscape — the gradient of the loss plays the role of a conservative force |
| [[Momentum]] | [[Vector]] (direction matters), [[Dot Product]] | [[Electric Current]] = charge momentum per unit time (loosely) | [[Vector]] operations; momentum is a vector quantity in robotics state spaces |
| [[Impulse]] | [[Integration by Parts]] ($\int F(t)\,dt$ for complex profiles) | [[Kirchhoff's Current Law]] analogy: charge impulse = $\int I\,dt = \Delta Q$ | Control theory: impulsive inputs in robot trajectory planning |
| [[Impulse-Momentum Theorem]] | Fundamental Theorem of Calculus ($\int \frac{dp}{dt}\,dt = \Delta p$) | — | [[Gradient Descent]]: each gradient step is an "impulse" to the parameter momentum |
| [[Conservation of Momentum]] | [[Vector]] addition (2D/3D component decomposition), [[Dot Product]] | Conservation of charge (KCL) is the electrical analog | Rigid-body dynamics in robotics; [[Matrix Multiplication]] for multi-body momentum state |
| [[Conservation of Energy]] | [[Geometric Series]] (energy decay in damped systems) | [[Kirchhoff's Voltage Law]] is energy conservation per loop | [[Loss Function]] minimization mirrors energy minimization in physical systems |
| [[Newton's Second Law]] | [[Ratio Test]] (stability of ODE solutions), [[Convergence]] | [[Ohm's Law]] ($V = IR$) is the electrical analog | [[Gradient]] of loss = force on parameters in the optimization landscape |

---

## Thematic Cross-Cuts

**The Two Great Conservation Laws of Classical Mechanics:**
- [[Conservation of Energy]] — energy is conserved; sourced from *space symmetry* (same physics everywhere, via Noether's theorem)
- [[Conservation of Momentum]] — momentum is conserved; sourced from *time symmetry* (same physics at all times)
Both are deeper than Newton's laws.

**The Two "Bridge" Theorems (Force ↔ Other Quantities):**
- [[Work-Energy Theorem]]: integrate force over *space* → get energy change. $W = \Delta K$
- [[Impulse-Momentum Theorem]]: integrate force over *time* → get momentum change. $J = \Delta p$

**Conservative vs. Non-Conservative — The Organizing Distinction for Energy:**
- Conservative forces ([[Conservative Force]]): gravity, springs, Coulomb. Potential energy exists. Energy is mechanically recoverable.
- Non-conservative forces: friction, drag, applied forces. Energy leaks to heat. Use $W_{\text{nc}} = \Delta E_{\text{mech}}$.

**Newton's Third Law as the Root of Two Major Results:**
- [[Newton's Third Law]] → [[Conservation of Momentum]] (impulses cancel internally)
- [[Newton's Third Law]] → explains why action-reaction pairs never cancel in a single free-body diagram
