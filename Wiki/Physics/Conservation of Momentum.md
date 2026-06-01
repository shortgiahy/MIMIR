# Conservation of Momentum

**One-liner:** The total momentum of an isolated system is constant — a direct consequence of Newton's Third Law, and one of the most powerful bookkeeping tools in all of physics.

## Core Idea
$$\vec{p}_{\text{total}} = \sum_i m_i\vec{v}_i = \text{constant} \quad \text{(no net external force)}$$
$$\vec{p}_{\text{before}} = \vec{p}_{\text{after}}$$
If the net external force on a system is zero — meaning internal forces only, or all external forces cancel — the system's total momentum cannot change. This holds in every direction independently: if $F_{\text{ext},x} = 0$, then $p_{\text{total},x}$ is conserved, even if $p_{\text{total},y}$ is not.

## Why It Exists
Conservation of momentum follows directly from [[Newton's Third Law]]: internal forces within a system come in equal-and-opposite pairs. Their impulses cancel exactly. By the [[Impulse-Momentum Theorem]], only external impulses can change total momentum. Remove external forces, and total momentum is locked in.

More deeply, conservation of momentum is a consequence of *spatial translation symmetry* (Noether's theorem): if the laws of physics are the same everywhere in space, momentum must be conserved. This is why it holds even in quantum mechanics and relativity — it is more fundamental than Newton's laws.

## Real-World Applications
- **Collisions (automotive):** crash investigators use momentum conservation to determine pre-crash speeds from post-crash skid patterns and final rest positions.
- **Explosions:** a bomb at rest has zero total momentum. After exploding, all fragments' momenta must still sum to zero. Forensic analysis of debris patterns uses this.
- **Billiards:** two-ball collisions in pool are solved exactly by combining momentum conservation (vector) and energy conservation (scalar). The 90° rule for identical-mass elastic collisions is a direct consequence.
- **Space navigation:** when a spacecraft fires thrusters, there are no external forces in deep space — momentum gained by the craft equals momentum carried away by exhaust.
- **Particle physics:** at the LHC, proton-proton collisions conserve total 4-momentum. Physicists detect "missing momentum" as evidence of neutrinos (or new particles) that escaped the detector.
- **Robotics:** when a robot arm pushes an object, the reaction force on the arm base changes base momentum — robot designers account for this to prevent tipping.

## Intuition
Momentum conservation is a "closed ledger": the total momentum in the system is a fixed sum, and internal interactions just shuffle it between objects. Two skaters at rest push off each other — they started with zero total momentum, so they must end with equal and opposite momenta. No external agent added or removed anything from the ledger.

**Elastic vs. Inelastic:** Both conserve momentum. The difference is kinetic energy:
- **Elastic collision:** $K$ conserved (billiard balls, atomic collisions). Objects bounce, no permanent deformation.
- **Perfectly inelastic collision:** maximum $K$ lost (objects stick together). $K$ is not conserved, but $p$ always is.
- **Partially inelastic:** reality — some $K$ lost to heat/deformation, but $p$ still conserved.

**Common trap:** "Momentum isn't conserved because the ball stopped." If a ball hits the floor and stops, the floor (and Earth) gained the ball's momentum. The *ball + Earth* system has conserved momentum — Earth just has enormous mass, so its velocity change is unmeasurable.

## Derivation
Consider a two-body system (A and B) with no external forces.

By [[Newton's Third Law]]:
$$\vec{F}_{B \text{ on } A} = -\vec{F}_{A \text{ on } B}$$

By [[Newton's Second Law]] for each object:
$$\frac{d\vec{p}_A}{dt} = \vec{F}_{B \text{ on } A}, \qquad \frac{d\vec{p}_B}{dt} = \vec{F}_{A \text{ on } B}$$

Add the two equations:
$$\frac{d\vec{p}_A}{dt} + \frac{d\vec{p}_B}{dt} = \vec{F}_{B \text{ on } A} + \vec{F}_{A \text{ on } B} = 0$$

$$\frac{d}{dt}(\vec{p}_A + \vec{p}_B) = 0 \implies \vec{p}_A + \vec{p}_B = \text{constant}$$

For $N$ objects, the argument extends: all internal forces form Third Law pairs that cancel pairwise. The sum of all internal forces is zero. So $\frac{d\vec{p}_{\text{total}}}{dt} = \vec{F}_{\text{ext, net}}$. If $\vec{F}_{\text{ext, net}} = 0$: $\vec{p}_{\text{total}} = \text{const}$.

**Elastic collision equations (1D, two bodies):**

Combine momentum conservation and kinetic energy conservation:
$$m_1 v_{1i} + m_2 v_{2i} = m_1 v_{1f} + m_2 v_{2f} \quad \text{(momentum)}$$
$$\frac{1}{2}m_1 v_{1i}^2 + \frac{1}{2}m_2 v_{2i}^2 = \frac{1}{2}m_1 v_{1f}^2 + \frac{1}{2}m_2 v_{2f}^2 \quad \text{(energy)}$$

Solve simultaneously (algebra omitted — factor the energy equation as difference of squares):
$$v_{1f} = \frac{m_1 - m_2}{m_1 + m_2}v_{1i} + \frac{2m_2}{m_1 + m_2}v_{2i}$$
$$v_{2f} = \frac{2m_1}{m_1 + m_2}v_{1i} + \frac{m_2 - m_1}{m_1 + m_2}v_{2i}$$

**Special case** $m_1 = m_2$ (equal masses, $v_{2i} = 0$): $v_{1f} = 0$, $v_{2f} = v_{1i}$ — objects exchange velocities completely (classic billiard result).

## Worked Example
**2D Collision:** A 1.2 kg ball A moves at $v_A = 6\text{ m/s}$ east. It strikes a stationary 0.8 kg ball B. After collision, A moves at $3\text{ m/s}$ at $30°$ north of east. Find B's velocity.

**Step 1 — Initial momenta:**
$$\vec{p}_{Ai} = (1.2)(6)\hat{x} = 7.2\hat{x}\text{ kg·m/s}$$
$$\vec{p}_{Bi} = 0$$
$$\vec{p}_{\text{total}} = 7.2\hat{x}\text{ kg·m/s}$$

**Step 2 — Final momentum of A:**
$$p_{Afx} = (1.2)(3)\cos(30°) = 3.6 \times \frac{\sqrt{3}}{2} \approx 3.118\text{ kg·m/s}$$
$$p_{Afy} = (1.2)(3)\sin(30°) = 3.6 \times 0.5 = 1.8\text{ kg·m/s}$$

**Step 3 — Conservation of momentum (each component):**
$$x:\quad 7.2 = 3.118 + p_{Bfx} \implies p_{Bfx} = 4.082\text{ kg·m/s}$$
$$y:\quad 0 = 1.8 + p_{Bfy} \implies p_{Bfy} = -1.8\text{ kg·m/s}$$

**Step 4 — B's velocity:**
$$v_{Bfx} = \frac{4.082}{0.8} = 5.10\text{ m/s (east)}$$
$$v_{Bfy} = \frac{-1.8}{0.8} = -2.25\text{ m/s (south)}$$
$$v_{Bf} = \sqrt{5.10^2 + 2.25^2} = \sqrt{26.01 + 5.06} \approx 5.57\text{ m/s}$$
$$\theta = \arctan\!\left(\frac{2.25}{5.10}\right) \approx 23.8°\text{ south of east}$$

**Step 5 — Check energy (is this elastic?):**
$$K_i = \frac{1}{2}(1.2)(6)^2 = 21.6\text{ J}$$
$$K_f = \frac{1}{2}(1.2)(3)^2 + \frac{1}{2}(0.8)(5.57)^2 \approx 5.4 + 12.4 = 17.8\text{ J}$$
$K_f < K_i$: this is a **partially inelastic** collision — 3.8 J was lost to deformation/heat.

## See Also
- [[Newton's Third Law]] — the direct cause: internal force pairs cancel, leaving total $p$ unchanged
- [[Impulse-Momentum Theorem]] — total external impulse = total $\Delta p$; zero external impulse → conservation
- [[Momentum]] — the quantity being conserved; $\vec{p} = m\vec{v}$
- [[Kinetic Energy]] — conserved additionally in elastic collisions; not conserved in inelastic ones
- [[Conservation of Energy]] — energy's counterpart; the two conservation laws together fully constrain elastic collisions
- [[Newton's Second Law]] — $F_{\text{ext}} = dp_{\text{total}}/dt$; zero net external force means zero rate of change
- [[Vector]] — 2D/3D momentum conservation requires component-wise bookkeeping (Math/ML connection)
- [[Dot Product]] — useful in elastic collision analysis to project velocity components (Math/ML connection)
