# Impulse

**One-liner:** Impulse is the total "push over time" that a force delivers — the integral of force with respect to time, which equals the change in momentum.

## Core Idea
$$\vec{J} = \int_{t_i}^{t_f} \vec{F}(t)\, dt$$
For a constant force, this simplifies to:
$$\vec{J} = \vec{F}\,\Delta t$$
Impulse is a vector with units of N·s (newton-seconds), which are identical to kg·m/s — the same units as momentum. This is not a coincidence: impulse *is* momentum change. The integral form handles the common real-world case where force varies in time (a bat hitting a ball, an airbag deploying, a foot striking the ground).

## Why It Exists
Newton's Second Law tells us that force changes velocity instantaneously, but in practice we care about the *total effect* of a force acting over a finite time. Impulse packages that effect into a single quantity: how much did the force change the object's momentum? It lets us analyze impacts, collisions, and propulsion events where we may not know the exact force profile at every millisecond — only the total effect.

## Real-World Applications
- **Crumple zones:** a car crash stops a passenger from $v = 60\text{ km/h}$ to $v = 0$. The required momentum change (impulse) is fixed. Crumple zones extend $\Delta t$ — spreading the impulse over a longer time reduces peak force on the body. $F = J/\Delta t$: same $J$, larger $\Delta t$, smaller $F$. This is why crumple zones save lives.
- **Catching a ball:** you instinctively move your hand back while catching. This extends contact time, reducing the peak force on your hand — same impulse (stopping the ball), lower peak force.
- **Airbags:** deploy in milliseconds to extend the time over which a person decelerates, reducing peak force on the skull and chest.
- **Golf/baseball/tennis:** bat or club contact times are ~1 ms. Coaches say "follow through" because longer contact time increases $\Delta t$, increasing impulse delivered (more momentum transferred to ball).
- **Rocket engines:** thrust is a continuous impulse. "Specific impulse" ($I_{sp}$) measures how efficiently a rocket converts propellant mass into impulse — the standard figure of merit for rocket engines.
- **Robotics:** collision-safe robots (collaborative robots / cobots) are designed with compliant, padded arms to increase collision contact time and reduce peak impact force on humans.

## Intuition
Think of impulse as a "momentum budget." To change an object's momentum by a fixed amount, you can either:
1. Apply a large force for a short time (hammer hitting a nail — violent, concentrated)
2. Apply a small force for a long time (gently pushing a car — slow, sustained)

The total momentum change — the impulse — is the same either way. The difference is the *experience* of that momentum change: a sharp spike vs. a gentle ramp.

**The area under a force-time graph** is the impulse. A tall narrow spike (impact) and a short wide plateau (push) can have identical areas — identical impulses, identical momentum changes.

**Why $\vec{J} = \vec{F}\,\Delta t$ for constant force:** if $\vec{F}$ is constant, $\int_{t_i}^{t_f} \vec{F}\,dt = \vec{F}(t_f - t_i) = \vec{F}\,\Delta t$.

## Derivation
Start with [[Newton's Second Law]] in its general form:
$$\vec{F}_{\text{net}} = \frac{d\vec{p}}{dt}$$

Multiply both sides by $dt$ and integrate from $t_i$ to $t_f$:
$$\int_{t_i}^{t_f} \vec{F}_{\text{net}}\,dt = \int_{t_i}^{t_f} \frac{d\vec{p}}{dt}\,dt$$

Left side is the definition of impulse $\vec{J}$. Right side integrates a perfect derivative:
$$\vec{J} = \int_{t_i}^{t_f} d\vec{p} = \vec{p}_f - \vec{p}_i = \Delta\vec{p}$$

This is the [[Impulse-Momentum Theorem]]: $\vec{J} = \Delta\vec{p}$.

**Average force interpretation:**

For a variable force, define the average force $\bar{F}$ such that:
$$\bar{F} = \frac{J}{\Delta t} = \frac{\Delta p}{\Delta t}$$

This is what engineers use when the force profile is unknown but the total impulse and contact time are measured (e.g., force plates in biomechanics labs).

## Worked Example
A 0.145 kg baseball traveling at $v_i = -40\text{ m/s}$ (toward the batter, negative direction) is struck by a bat and leaves at $v_f = +50\text{ m/s}$ (away from the batter). Contact time is $\Delta t = 1.5\text{ ms} = 0.0015\text{ s}$. Find the impulse and average force.

**Step 1 — Initial and final momentum:**
$$p_i = (0.145)(-40) = -5.8\text{ kg·m/s}$$
$$p_f = (0.145)(+50) = +7.25\text{ kg·m/s}$$

**Step 2 — Impulse (= momentum change):**
$$J = \Delta p = p_f - p_i = 7.25 - (-5.8) = 13.05\text{ N·s}$$

**Step 3 — Average force:**
$$\bar{F} = \frac{J}{\Delta t} = \frac{13.05}{0.0015} = 8{,}700\text{ N} \approx 8.7\text{ kN}$$

This is about 61× the weight of the baseball — an enormous force compressed into 1.5 ms. The bat doesn't need to sustain this force; it only needs to deliver the impulse.

**Step 4 — Intuition check:** If the batter "follows through" and extends contact to $\Delta t = 2\text{ ms}$, the average force drops to $\bar{F} = 13.05/0.002 = 6{,}525\text{ N}$ — but the ball still receives the same impulse and exits at the same speed (if the momentum change is the same). "Following through" actually matters for maintaining contact longer, which increases the impulse delivered.

## See Also
- [[Impulse-Momentum Theorem]] — the theorem connecting impulse to $\Delta p$; impulse is the cause, $\Delta p$ is the effect
- [[Momentum]] — what impulse changes; $\vec{J} = \Delta\vec{p}$
- [[Newton's Second Law]] — the law impulse is derived from; $\vec{F} = d\vec{p}/dt$
- [[Conservation of Momentum]] — when internal impulses cancel (Newton's Third Law), total momentum is conserved
- [[Work]] — the space-domain analog; work = $\int F\,dx$, impulse = $\int F\,dt$
- [[Integration by Parts]] — sometimes needed when evaluating complex $\int F(t)\,dt$ profiles (Math connection)
