# Kinetic Energy

**One-liner:** Kinetic energy is the energy an object has by virtue of its motion — the work required to bring it from rest to its current speed.

## Core Idea
$$K = \frac{1}{2}mv^2$$
Kinetic energy is a scalar, always non-negative, measured in joules. It depends on mass linearly but on speed *quadratically* — doubling speed quadruples kinetic energy. This quadratic relationship has major consequences for safety (crash energy), efficiency (air drag), and control (braking distance).

## Why It Exists
We need a way to quantify "how much motion energy" an object has so we can track how [[Work]] transfers it. Without kinetic energy, the [[Work-Energy Theorem]] has nothing to compute, and energy methods (which are often far simpler than force methods) become unavailable.

## Real-World Applications
- **Vehicle safety:** kinetic energy at impact scales as $v^2$ — a car going 60 mph has *four times* the kinetic energy of a car going 30 mph, which is why crash severity increases so rapidly with speed.
- **Bullets and ballistics:** a bullet's stopping power is $\frac{1}{2}mv^2$; high-velocity rounds with modest mass can carry more energy than slow, heavy rounds.
- **Wind turbines:** power generated scales as $v^3$ (kinetic energy per unit time from a cross-section of air), explaining why turbines are placed in high-wind areas.
- **Robotics:** a fast-moving robot arm stores significant kinetic energy; emergency stops must plan for dissipating this energy safely — hitting a person at high speed transfers that energy dangerously.
- **Hydroelectric dams:** water's kinetic energy as it falls is converted to electrical energy by turbines.

## Intuition
Think of kinetic energy as the "damage potential" of a moving object. A slow-rolling ball does little damage when stopped. The same ball moving ten times faster has one hundred times the stopping energy to absorb (because $v^2$). This is why the speed limit difference between 30 mph and 60 mph is not "twice as dangerous" but *four times* the kinetic energy at impact.

## Derivation
Start with [[Newton's Second Law]]: $F = ma$. Apply a constant net force over displacement $\Delta x$, starting from rest ($v_0 = 0$):

From [[Kinematic Equations]]: $v^2 = v_0^2 + 2a\Delta x = 2a\Delta x$, so $a\Delta x = v^2/2$.

Now multiply both sides of $F = ma$ by displacement $\Delta x$:
$$W = F\Delta x = ma\Delta x = m\cdot\frac{v^2}{2} = \frac{1}{2}mv^2$$
The work done on the object from rest equals $\frac{1}{2}mv^2$. That's kinetic energy.

**The full general derivation (variable force):**
$$W_{\text{net}} = \int_{x_i}^{x_f} F\, dx = \int m\frac{dv}{dt}\, dx = \int m\frac{dv}{dx}\frac{dx}{dt}\, dx = \int mv\, dv = \frac{1}{2}mv_f^2 - \frac{1}{2}mv_i^2$$
This is the [[Work-Energy Theorem]] in general form, and it shows exactly where the $\frac{1}{2}$ and $v^2$ come from.

## Worked Example
A 1500 kg car accelerates from $v_i = 10\text{ m/s}$ to $v_f = 30\text{ m/s}$.

**Step 1 — Calculate initial kinetic energy:**
$$K_i = \frac{1}{2}(1500)(10)^2 = \frac{1}{2}(1500)(100) = 75{,}000\text{ J} = 75\text{ kJ}$$

**Step 2 — Calculate final kinetic energy:**
$$K_f = \frac{1}{2}(1500)(30)^2 = \frac{1}{2}(1500)(900) = 675{,}000\text{ J} = 675\text{ kJ}$$

**Step 3 — Find change in kinetic energy:**
$$\Delta K = K_f - K_i = 675 - 75 = 600\text{ kJ}$$

**Step 4 — By the Work-Energy Theorem, the engine (minus friction losses) did 600 kJ of net work.**

**Step 5 — Intuition check:** Speed tripled (10 → 30 m/s), kinetic energy increased by a factor of 9 (75 → 675 kJ). $3^2 = 9$. The quadratic relationship confirmed.

## See Also
- [[Work-Energy Theorem]] — net work equals $\Delta K$; KE is one side of this equation
- [[Work]] — work done is what changes kinetic energy
- [[Conservation of Energy]] — KE and potential energy trade off in conservative systems
- [[Momentum]] — $p = mv$; related but distinct: $K = p^2/(2m)$
- [[Newton's Second Law]] — used to derive the $\frac{1}{2}mv^2$ form
- [[Kinematic Equations]] — the constant-acceleration intermediate step in the derivation
