# Impulse-Momentum Theorem

**One-liner:** The net impulse acting on an object equals its change in momentum — it is Newton's Second Law integrated over time, and the time-domain counterpart to the Work-Energy Theorem.

## Core Idea
$$\vec{J}_{\text{net}} = \Delta\vec{p} = \vec{p}_f - \vec{p}_i = m\vec{v}_f - m\vec{v}_i$$
This is not a new law — it *is* Newton's Second Law, rewritten. Where the [[Work-Energy Theorem]] integrates $F$ over displacement ($W = \int F\,dx = \Delta K$), this theorem integrates $F$ over time ($J = \int F\,dt = \Delta p$). The two theorems are parallel tools: one for the energy domain, one for the momentum domain.

## Why It Exists
Many problems give you information about forces and time (not forces and displacement). Impact problems, collision analysis, and propulsion calculations naturally live in the time domain. The Impulse-Momentum Theorem is the tool that translates "a force acted for this long" into "the momentum changed by this much." It also lets you work backward: measure the momentum change, and deduce the average force — useful when direct force measurement is impractical (a bat-ball collision lasts 1 ms; you can't put a sensor there, but you can photograph the ball before and after).

## Real-World Applications
- **Biomechanics / sports science:** force plates under an athlete measure $F(t)$ during a jump. Integrating gives impulse, which equals the change in momentum of the athlete-Earth system — used to compute jump height and takeoff velocity.
- **Crash analysis:** police reconstruct accidents by measuring skid marks (velocity before crash) and post-crash positions (velocity after). $\Delta p = J = \bar{F}\,\Delta t$ lets engineers estimate peak crash forces.
- **Rocket science (specific impulse):** the efficiency rating of a rocket engine, $I_{sp}$, is defined as total impulse delivered per unit weight of propellant — directly from this theorem.
- **Ballistics:** the "stopping power" of ammunition is its impulse delivered to a target. Forensic engineers use $\Delta p$ measurements to reconstruct bullet velocities.
- **Robotics:** joint controllers use impulse-momentum analysis to plan safe deceleration profiles — how much time must the arm decelerate over to avoid joint force limits.

## Intuition
This theorem is the "momentum accounting" version of $F = ma$. Instead of asking "what acceleration does this force produce right now?", it asks "what total momentum change does this force produce over this duration?"

**Parallel to Work-Energy Theorem:**

| Domain | Theorem | Equation |
|--------|---------|----------|
| Space (displacement) | Work-Energy | $W_{\text{net}} = \Delta K$ |
| Time (duration) | Impulse-Momentum | $J_{\text{net}} = \Delta p$ |

Both are derived from $F = ma$ by integrating. Both convert a force problem into a simpler "before/after" comparison. The choice of which to use depends on what you know: if you know displacement, use Work-Energy; if you know time, use Impulse-Momentum.

**Why it's more powerful than $F = ma$ for collisions:** During an impact, the force varies wildly from 0 to thousands of N in milliseconds. Solving $F(t) = ma(t)$ requires knowing the full force profile. The theorem bypasses this: just measure $\Delta p$, and the average force follows automatically.

## Derivation
Begin with [[Newton's Second Law]] in its general (momentum) form:
$$\vec{F}_{\text{net}}(t) = \frac{d\vec{p}}{dt}$$

Multiply both sides by $dt$:
$$\vec{F}_{\text{net}}(t)\,dt = d\vec{p}$$

Integrate both sides from time $t_i$ to $t_f$:
$$\int_{t_i}^{t_f} \vec{F}_{\text{net}}(t)\,dt = \int_{t_i}^{t_f} d\vec{p}$$

Left side is the definition of net [[Impulse]] $\vec{J}_{\text{net}}$. Right side evaluates the definite integral of an exact differential:
$$\vec{J}_{\text{net}} = \vec{p}(t_f) - \vec{p}(t_i) = \vec{p}_f - \vec{p}_i = \Delta\vec{p}$$

For constant mass $m$:
$$\vec{J}_{\text{net}} = m\vec{v}_f - m\vec{v}_i = m\Delta\vec{v}$$

**Note on multiple forces:** if multiple forces act, the net impulse is the vector sum of individual impulses:
$$\vec{J}_{\text{net}} = \sum_i \vec{J}_i = \Delta\vec{p}$$
This is used in [[Conservation of Momentum]] to show that internal force impulses cancel by [[Newton's Third Law]], leaving only external impulses to change total system momentum.

## Worked Example
A 0.5 kg hockey puck slides at $v_i = 8\text{ m/s}$ east across frictionless ice. A player strikes it with a stick for $\Delta t = 0.08\text{ s}$, applying a force with variable profile. After the strike, the puck moves at $v_f = 15\text{ m/s}$ at $60°$ north of east. Find: (a) the impulse vector, (b) the average force vector.

**Step 1 — Initial momentum:**
$$\vec{p}_i = (0.5)(8)\hat{x} = 4\hat{x}\text{ kg·m/s}$$

**Step 2 — Final momentum components:**
$$p_{fx} = (0.5)(15)\cos(60°) = (7.5)(0.5) = 3.75\text{ kg·m/s}$$
$$p_{fy} = (0.5)(15)\sin(60°) = (7.5)\left(\frac{\sqrt{3}}{2}\right) \approx 6.50\text{ kg·m/s}$$
$$\vec{p}_f = 3.75\hat{x} + 6.50\hat{y}\text{ kg·m/s}$$

**Step 3 — Impulse = $\Delta\vec{p}$:**
$$\vec{J} = \vec{p}_f - \vec{p}_i = (3.75 - 4)\hat{x} + (6.50 - 0)\hat{y} = -0.25\hat{x} + 6.50\hat{y}\text{ N·s}$$

**Step 4 — Magnitude and direction of impulse:**
$$J = \sqrt{(-0.25)^2 + (6.50)^2} = \sqrt{0.0625 + 42.25} \approx 6.50\text{ N·s}$$
$$\theta = \arctan\!\left(\frac{6.50}{-0.25}\right) \approx 88°\text{ north of west (in Q2)}$$

**Step 5 — Average force:**
$$\bar{\vec{F}} = \frac{\vec{J}}{\Delta t} = \frac{-0.25\hat{x} + 6.50\hat{y}}{0.08} = -3.13\hat{x} + 81.25\hat{y}\text{ N}$$

**Interpretation:** The stick slowed the puck slightly in $x$ and accelerated it strongly northward — the impulse vector tells the whole story even though the exact force profile during the $0.08\text{ s}$ contact is unknown.

## See Also
- [[Impulse]] — the left-hand side; $\vec{J} = \int \vec{F}\,dt$
- [[Momentum]] — the right-hand side; $\Delta\vec{p}$ is what changes
- [[Newton's Second Law]] — the law this theorem derives from; $\vec{F} = d\vec{p}/dt$
- [[Work-Energy Theorem]] — the space-domain parallel: $W = \Delta K$ vs. $J = \Delta p$
- [[Conservation of Momentum]] — applies when $\vec{J}_{\text{net, external}} = 0$
- [[Newton's Third Law]] — the reason internal impulses cancel in a system
- [[u-Substitution]] — the integral $\int F(t)\,dt$ sometimes requires substitution techniques (Math connection)
- [[Integration by Parts]] — used when $F(t)$ is a product of functions (e.g., $F = t e^{-t}$), making the impulse integral $\int F(t)\,dt$ require integration by parts
- [[Stochastic Gradient Descent]] — momentum in SGD is the time-integrated gradient — structurally identical to impulse ($J = \int F\,dt$) giving accumulated directional push to the optimizer
