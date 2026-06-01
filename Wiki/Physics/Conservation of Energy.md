# Conservation of Energy

**One-liner:** The total energy of a closed system is constant — energy can change form but is never created or destroyed.

## Core Idea
$$E_{\text{total}} = K + U = \text{constant} \quad \text{(no non-conservative forces)}$$
$$K_i + U_i = K_f + U_f$$
When only [[Conservative Force|conservative forces]] do work, the total mechanical energy (kinetic + potential) is conserved. When non-conservative forces (friction, drag, applied forces) act, the total energy still conserves if you include all forms (heat, etc.), but mechanical energy alone changes.

## Why It Exists
Energy conservation is not something we derived from Newton's laws — it is a consequence of the *symmetry of physical laws under time translation* (Noether's theorem: if the laws of physics are the same today as tomorrow, energy must be conserved). It is one of the most powerful problem-solving tools in physics because it relates initial and final states without needing to know anything about the path in between.

## Real-World Applications
- **Roller coasters:** height converts to speed and back. Engineers use $K + U_g = \text{const}$ to design minimum heights for loops without solving the full differential equations of motion.
- **Pendulum clocks:** energy oscillates between $U_g$ (at peak) and $K$ (at bottom). The period is determined entirely by energy conservation + geometry.
- **Power grids:** energy input (chemical, nuclear, wind) must match energy output (electrical, heat). Grid operators track energy to balance supply and demand.
- **Robotics:** a falling robot arm stores gravitational PE; controllers must plan to absorb that energy (via braking torques) rather than letting it slam into the stops. Energy auditing prevents hardware damage.
- **Bouncing ball:** a perfectly elastic ball converts all KE to elastic PE at impact and back again. Real balls lose some to heat — the coefficient of restitution measures this.

## Intuition
Energy is the universe's currency. You can convert dollars to euros to yen, but the total wealth doesn't change (ignoring fees). Work is depositing; friction is the "bank fee" that converts mechanical energy to heat, permanently. In a frictionless world, energy sloshes between KE and PE forever. In the real world, friction and drag are the leaks.

The key insight: this law doesn't say motion is unchanging — a ball can speed up and slow down dramatically. It says the *total* of its speed-energy and height-energy stays the same.

## Derivation
For a conservative force, by definition, the work done equals the *decrease* in potential energy:
$$W_{\text{conservative}} = -\Delta U$$

The [[Work-Energy Theorem]] says:
$$W_{\text{net}} = \Delta K$$

If only conservative forces act ($W_{\text{net}} = W_{\text{conservative}}$):
$$\Delta K = -\Delta U$$
$$\Delta K + \Delta U = 0$$
$$(K_f - K_i) + (U_f - U_i) = 0$$
$$K_f + U_f = K_i + U_i = E_{\text{total}} = \text{const}$$

**With non-conservative forces (friction, applied forces):**
$$W_{\text{non-conservative}} = \Delta E_{\text{mechanical}} = \Delta K + \Delta U$$
Energy is still conserved overall — friction converts mechanical energy to thermal energy:
$$K_i + U_i + W_{\text{nc}} = K_f + U_f$$

## Worked Example
A 2 kg ball is dropped from 10 m. Find its speed just before hitting the ground, and compare to the speed if it had been thrown horizontally at $5\text{ m/s}$ from the same height.

**Case 1 — Dropped:**

Step 1: Set $h = 0$ at ground. Initial: $K_i = 0$, $U_i = mgh = 2(9.8)(10) = 196\text{ J}$.

Step 2: Final (at ground): $U_f = 0$. By conservation:
$$K_f = K_i + U_i = 0 + 196 = 196\text{ J}$$
$$v_f = \sqrt{\frac{2K_f}{m}} = \sqrt{\frac{2(196)}{2}} = \sqrt{196} = 14\text{ m/s}$$

**Case 2 — Thrown horizontally at $5\text{ m/s}$:**

Step 1: Initial $K_i = \frac{1}{2}(2)(5)^2 = 25\text{ J}$, $U_i = 196\text{ J}$. Total $E = 221\text{ J}$.

Step 2: Final: $U_f = 0$, $K_f = 221\text{ J}$.
$$v_f = \sqrt{\frac{2(221)}{2}} = \sqrt{221} \approx 14.87\text{ m/s}$$

**Key insight:** Both balls hit at nearly the same speed because gravitational PE dominates. The horizontal speed just adds a little. This also reveals why the vertical and horizontal motions are energy-additive via $K = \frac{1}{2}m(v_x^2 + v_y^2)$.

## See Also
- [[Conservative Force]] — required for the simple $K + U = \text{const}$ form
- [[Kinetic Energy]] — one form of mechanical energy
- [[Gravitational Potential Energy]] — the most common potential energy in introductory physics
- [[Elastic Potential Energy]] — potential energy in springs; also conserved
- [[Work-Energy Theorem]] — the foundation from which this law is derived
- [[Momentum]] — separately conserved; independent of energy conservation
- [[Noether's Theorem]] — the deep reason energy conservation exists (Math/Physics connection)
- [[Loss Function]] — in ML, the loss is the quantity minimized during training; energy conservation and loss minimization are both about tracking a global scalar that the system "wants" to reduce
- [[Electric Power]] — in EE circuits, energy conservation governs power flow: all power delivered by sources must equal power consumed by resistors ($\sum P = 0$)
