# Work-Energy Theorem

**One-liner:** The net work done on an object equals its change in kinetic energy — it is the direct connection between forces and energy.

## Core Idea
$$W_{\text{net}} = \Delta K = K_f - K_i = \frac{1}{2}mv_f^2 - \frac{1}{2}mv_i^2$$
This theorem is not a definition — it is a *derived result* from [[Newton's Second Law]]. It says: add up all the work done by all forces on an object, and you get exactly the change in its kinetic energy. No work done? Kinetic energy stays constant.

## Why It Exists
[[Newton's Second Law]] works in the "force and acceleration" domain; the Work-Energy Theorem translates everything into the "energy" domain. Energy methods are often dramatically simpler for problems where you know initial and final states but not the intermediate forces. It is the gateway theorem to all of energy conservation.

## Real-World Applications
- **Roller coasters:** engineers use this theorem to calculate speed at the bottom of a hill from the height at the top (converting potential to kinetic energy), without needing to analyze forces at every point of the curve.
- **Crash testing:** the work done by crumple zones (negative work, opposing motion) equals the kinetic energy that must be absorbed — this determines how much crumple zone material is needed.
- **Catapults and trebuchets:** the work done by the throwing arm equals the kinetic energy of the projectile at release.
- **Robotics:** motor work calculations — how much electrical energy (work input) is needed to bring a robot arm to a target speed.

## Intuition
The theorem says energy is conserved in a specific sense: work is energy being *transferred in* via forces. If forces do positive net work, the object speeds up (gains KE). If forces do negative net work (like friction, braking), the object slows down (loses KE). The theorem is an accounting identity: all energy has to go somewhere.

Think of it as a bank account. Net work is the deposit or withdrawal; kinetic energy is the balance. The theorem says "balance change = net deposit."

## Derivation
Start with [[Newton's Second Law]] in one dimension: $F_{\text{net}} = ma$.

Multiply both sides by an infinitesimal displacement $dx$:
$$F_{\text{net}}\, dx = ma\, dx$$

Rewrite $a\, dx$ using the chain rule (a key calculus move):
$$a = \frac{dv}{dt} = \frac{dv}{dx}\cdot\frac{dx}{dt} = v\frac{dv}{dx} \implies a\,dx = v\, dv$$

So:
$$F_{\text{net}}\, dx = mv\, dv$$

Integrate both sides from initial to final state:
$$\int_{x_i}^{x_f} F_{\text{net}}\, dx = \int_{v_i}^{v_f} mv\, dv$$
$$W_{\text{net}} = \frac{1}{2}mv_f^2 - \frac{1}{2}mv_i^2 = \Delta K$$

This derivation shows the theorem is *exactly* Newton's Second Law rewritten in energy language — same physics, different vocabulary.

## Worked Example
A 2 kg block starts at rest and is pushed 3 m by a 20 N force, with 8 N of friction opposing motion.

**Step 1 — Calculate work by applied force:**
$$W_{\text{applied}} = (20\text{ N})(3\text{ m})\cos(0°) = 60\text{ J}$$

**Step 2 — Calculate work by friction:**
$$W_{\text{friction}} = (8\text{ N})(3\text{ m})\cos(180°) = -24\text{ J}$$
(Negative because friction opposes displacement.)

**Step 3 — Net work:**
$$W_{\text{net}} = 60 + (-24) = 36\text{ J}$$

**Step 4 — Apply Work-Energy Theorem:**
$$\Delta K = W_{\text{net}} = 36\text{ J}$$
$$K_f = K_i + 36 = 0 + 36 = 36\text{ J}$$

**Step 5 — Find final speed:**
$$\frac{1}{2}(2)v_f^2 = 36 \implies v_f^2 = 36 \implies v_f = 6\text{ m/s}$$

## See Also
- [[Work]] — the input; $W_{\text{net}}$ is the sum of all individual works
- [[Kinetic Energy]] — the output; net work changes kinetic energy
- [[Newton's Second Law]] — the law this theorem is derived from
- [[Conservation of Energy]] — generalizes this theorem when potential energy is included
- [[Impulse-Momentum Theorem]] — the time-domain analog: $J = \Delta p$ vs $W = \Delta K$
- [[Integral]] — the derivation requires integrating $F\, dx$
- [[Electric Power]] — power $P = dW/dt$ is the rate of work; the Work-Energy Theorem has a direct EE counterpart where electrical energy delivered equals the change in the circuit's stored energy
