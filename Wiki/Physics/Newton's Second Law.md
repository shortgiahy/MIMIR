# Newton's Second Law

**One-liner:** Net force equals mass times acceleration — the more force you apply, the more an object accelerates; the more massive it is, the less it accelerates.

## Core Idea
$$\vec{F}_{\text{net}} = m\vec{a} = \frac{d\vec{p}}{dt}$$
Newton actually wrote this as *rate of change of momentum*, not $F = ma$. The $ma$ form is the special case when mass is constant. This law is the central equation of classical mechanics: given forces, predict motion; given desired motion, determine required forces.

## Why It Exists
[[Newton's First Law]] tells you what happens with zero net force. The Second Law answers "what happens with *nonzero* net force?" It quantifies the relationship between cause (force) and effect (acceleration), making physics predictive. It replaced vague Aristotelian ideas about "natural motion" with a precise, calculable relationship.

## Real-World Applications
- **Vehicle dynamics:** engineers use $F = ma$ to size engines (how much force to accelerate a car from 0–100 km/h in a given time) and brakes (how much force to stop safely).
- **Rocket science:** $F_{\text{thrust}} - F_{\text{drag}} - mg = ma$; solve for $a$ to plan burn durations and fuel loads. (Note: rocket mass changes, so the full $dp/dt$ form is needed.)
- **Robotics (Baymax):** joint torque controllers use $\tau = I\alpha$ (the rotational form of Newton's Second Law) to compute how much motor torque is needed to achieve a desired joint acceleration. Every motion plan traces back to this law.
- **Biomechanics:** force plates measure ground reaction forces; combined with body mass, they give the acceleration of the center of mass during walking and jumping.
- **Machine learning analogy:** [[Gradient Descent]] is mathematically analogous — the "force" is the negative gradient $-\nabla L$, the "mass" is learning rate $1/\eta$, and the parameter update is the resulting "acceleration" toward lower loss. Both are laws of the form "net push determines change."

## Intuition
Mass is the resistance to acceleration — the harder it is to speed up, the more mass an object has. Force is the push. Acceleration is the result. Pushing a shopping cart vs. pushing a loaded truck: same force, drastically different acceleration because mass differs. Same truck, push twice as hard: twice the acceleration. This direct proportionality is the law.

## Derivation
Newton's original statement: the net force equals the rate of change of [[Momentum]] $\vec{p} = m\vec{v}$:
$$\vec{F}_{\text{net}} = \frac{d\vec{p}}{dt} = \frac{d(m\vec{v})}{dt}$$
If mass $m$ is constant (most introductory cases):
$$\vec{F}_{\text{net}} = m\frac{d\vec{v}}{dt} = m\vec{a}$$
This shows $F = ma$ is not the most general form — for variable-mass systems (rockets, chain falling off a table), you must use the full $dp/dt$ form.

In component form:
$$F_{\text{net},x} = ma_x, \quad F_{\text{net},y} = ma_y, \quad F_{\text{net},z} = ma_z$$
The three directions are independent — a horizontal force cannot directly cause vertical acceleration.

**Rotational analog:** $\vec{\tau}_{\text{net}} = I\vec{\alpha}$, where $\tau$ is torque, $I$ is moment of inertia, and $\alpha$ is angular acceleration.

## Worked Example
An elevator (total mass 800 kg) accelerates upward at $2\text{ m/s}^2$. Find the tension in the cable.

**Step 1 — Draw free-body diagram:** Two forces on elevator: tension $T$ upward, gravity $mg$ downward.

**Step 2 — Define positive direction:** Up is positive.

**Step 3 — Apply Newton's Second Law:**
$$T - mg = ma$$

**Step 4 — Solve for $T$:**
$$T = m(g + a) = 800(9.8 + 2) = 800 \times 11.8 = 9440\text{ N}$$

**Step 5 — Interpret:** $T > mg$ ($= 7840\text{ N}$) because the cable must both support the elevator *and* accelerate it upward. If $a = 0$, $T = mg$. If $a < 0$ (decelerating upward), $T < mg$.

## See Also
- [[Net Force]] — the $\vec{F}_{\text{net}}$ input to this law
- [[Acceleration]] — the $\vec{a}$ output of this law
- [[Momentum]] — Newton's original formulation; $F = ma$ is a special case
- [[Newton's First Law]] — special case when $F_{\text{net}} = 0$
- [[Newton's Third Law]] — paired reaction forces; crucial for identifying what's in $F_{\text{net}}$
- [[Derivative]] — $F = dp/dt$ is a derivative; this law is fundamentally calculus
- [[Gradient Descent]] — ML optimization with the same mathematical structure: the gradient plays the role of force, the parameter plays the role of mass, and the update step is the resulting "acceleration" toward lower loss
- [[Vector]] — $\vec{F} = m\vec{a}$ is a vector equation; all three quantities are vectors, the same mathematical object used for gradients and features in ML
