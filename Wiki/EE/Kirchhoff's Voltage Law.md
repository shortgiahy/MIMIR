# Kirchhoff's Voltage Law

**One-liner:** The sum of all voltage rises and drops around any closed loop in a circuit equals zero — energy is conserved as charge travels around a loop.

## Core Idea
$$\sum_{k} V_k = 0 \quad \text{(around any closed loop)}$$
Traversing a closed loop in a circuit, every voltage rise (through a source) and every voltage drop (through a resistor or load) must cancel exactly. The choice of loop direction (clockwise or counterclockwise) is arbitrary — just be consistent within a loop. Use sign convention: voltage rises are positive when traversing in the direction of conventional current; voltage drops are negative (or reverse for the opposite convention — pick one).

## Why It Exists
KVL is not an arbitrary rule — it is conservation of energy. Moving a test charge around a complete loop brings it back to exactly its starting potential. If the sum of voltage changes weren't zero, the charge would gain or lose energy on a round trip — spontaneously creating or destroying energy. That's impossible (thermodynamics). KVL is how that physical constraint gets applied to circuit analysis: it provides the equations needed to solve for unknown voltages in any circuit loop.

## Real-World Applications
- **Battery and internal resistance:** A 12 V battery with 0.1 Ω internal resistance driving a 2 Ω load: KVL around the loop: $12 - I(0.1) - I(2) = 0 \implies I = 5.71\,\text{A}$. Terminal voltage: $12 - 5.71 \times 0.1 = 11.43\,\text{V}$. The battery "drags" under load.
- **Voltage drops in Baymax's power harness:** Every connector and wire drops voltage under load. KVL traces the full supply path from battery → fuse → switch → trace → motor, ensuring the motor sees its minimum required voltage.
- **Op-amp feedback loops:** In negative feedback amplifiers, KVL around the feedback loop determines gain. The virtual short approximation is a KVL consequence.
- **Multi-source circuits:** Two batteries in a loop with resistors requires KVL to find currents — simple Ohm's Law can't handle multiple sources.

## Intuition
**The elevation analogy:** Imagine hiking a circular trail. You start at base camp (1000 m), climb a hill (gain 500 m), cross a plateau, descend 300 m, continue, and eventually return to base camp. The total elevation change is zero — you ended where you started. You may have gone up and down many times, but the net is zero.

In a circuit: starting at a node, each voltage source is a hill (gain) or valley (drop), each resistor is a descent (drop in direction of current). When you return to the starting node, the total voltage change is zero.

**The physical reality:** Voltage is electric potential. Moving a charge around a closed path in a static electric field requires zero net work — otherwise you'd have a perpetual energy source. KVL is this statement for circuits. (Caveat: in the presence of changing magnetic flux — like a transformer or inductor — the path encloses EMF, and KVL must account for it via Faraday's Law. For DC resistive circuits, ignore this.)

## Derivation
**From conservation of energy:**

Moving charge $q$ from point A around a loop back to A:
$$W_{total} = q \oint \vec{E} \cdot d\vec{l}$$

For static electric fields (electrostatics), the field is conservative:
$$\oint \vec{E} \cdot d\vec{l} = 0$$

Therefore:
$$W_{total} = 0 \implies \sum_k V_k = 0$$

**Lumped circuit formulation:**

In a loop with voltage sources $V_{s,k}$ and resistors $R_j$ carrying current $I_j$:
$$\sum_k V_{s,k} - \sum_j I_j R_j = 0$$

For a single-loop circuit with source $V_s$, and resistors $R_1, R_2, \ldots, R_n$ in series:
$$V_s - IR_1 - IR_2 - \cdots - IR_n = 0$$
$$V_s = I(R_1 + R_2 + \cdots + R_n)$$

This is the series circuit formula derived directly from KVL.

**Mesh analysis:** For multi-loop circuits, assign a mesh current to each independent loop. Apply KVL to each loop. This generates a system of linear equations solved simultaneously — the mesh current method.

## Worked Example
**Problem:** Baymax's left servo motor circuit has:
- 7.4 V LiPo battery (internal resistance $r = 0.05\,\Omega$)
- Fuse resistance $0.02\,\Omega$
- Wiring resistance $0.03\,\Omega$
- Servo motor (modeled as $R_{servo} = 2.4\,\Omega$ at stall)

Find the stall current and the voltage actually at the servo terminals.

**Setup:** Single loop. KVL (going clockwise, sum of drops = source):
$$7.4 = I(0.05) + I(0.02) + I(0.03) + I(2.4)$$
$$7.4 = I(0.05 + 0.02 + 0.03 + 2.4) = I(2.5)$$
$$I = \frac{7.4}{2.5} = 2.96\,\text{A}$$

Voltage at servo terminals (KVL from battery positive, subtracting drops):
$$V_{servo} = 7.4 - 2.96(0.05) - 2.96(0.02) - 2.96(0.03)$$
$$V_{servo} = 7.4 - 0.148 - 0.059 - 0.089 = 7.4 - 0.296 = 7.104\,\text{V}$$

The parasitic resistances "steal" 0.296 V (about 4% of supply) — acceptable here, but in a 3.3 V logic system the same losses would be severe.

**Verification with KVL:** Sum all drops including servo:
$$0.148 + 0.059 + 0.089 + 2.96 \times 2.4 = 0.296 + 7.104 = 7.4\,\text{V} \checkmark$$

## See Also
- [[Voltage]] — KVL is fundamentally a statement about voltage differences summing to zero
- [[Kirchhoff's Current Law]] — the companion law for nodes; together KCL+KVL solve any circuit
- [[Series Circuit]] — series circuits are analyzed primarily via KVL (voltages add)
- [[Voltage Divider]] — voltage divider formula derived by applying KVL and Ohm's Law
- [[Ohm's Law]] — used within KVL loops to express $V = IR$ for each resistive element
- [[Electric Power]] — power dissipated in each element equals $V_k \cdot I$; KVL distributes source energy
- [[Work]] — KVL is the statement that net work done moving charge around a closed loop is zero
- [[Conservation of Energy]] — KVL IS energy conservation: the sum of voltage rises and drops around a loop equals zero because energy cannot be created or destroyed on a round trip
