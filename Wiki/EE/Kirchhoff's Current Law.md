# Kirchhoff's Current Law

**One-liner:** At any node in a circuit, the sum of all currents entering equals the sum of all currents leaving — no charge accumulates at a node.

## Core Idea
$$\sum_{k} I_k = 0 \quad \text{(at any node, with sign convention)}$$
Equivalently: $I_{in} = I_{out}$. Using the sign convention where currents into a node are positive and currents out are negative (or vice versa — pick one and stay consistent), KCL states the algebraic sum of all currents at a node is zero. This applies at every instant in time, for DC and AC circuits.

## Why It Exists
KCL is not a circuit rule Kirchhoff invented — it is a direct consequence of conservation of charge. Charge cannot accumulate indefinitely at a junction (that would require a capacitor, and KCL holds for the node excluding capacitor interiors). Every electron that arrives at a junction must leave through one of the other branches. Without KCL, there's no systematic way to write the equations needed to solve circuits with multiple branches, multiple voltage sources, or complex topology.

## Real-World Applications
- **Node voltage method:** The standard technique for solving multi-node circuits applies KCL at each unknown node to generate a system of equations. Every circuit simulator (SPICE, LTspice) builds its matrix equations from KCL at each node.
- **Current sensing in motor drivers:** In Baymax's H-bridge, KCL tells you that the current into the motor must equal the current measured on the high-side (or low-side) sense resistor. If they don't match, there's a fault (short, open, or bypass path).
- **Fuse and circuit breaker design:** If 30 A enters a PCB power plane and only 28 A exits through load traces, 2 A is flowing through an unintended path (fault current). KCL makes this visible.
- **Sensor node biasing:** An op-amp summing junction uses KCL explicitly — currents from multiple input resistors sum at the inverting input node.

## Intuition
KCL is conservation of charge applied to a single point. Think of a junction in a water pipe system: water flowing in through any pipe must flow out through the other pipes. None of it vanishes or accumulates at the junction (assuming the junction isn't a tank — a capacitor is the circuit equivalent of a tank). 

The water analogy is exact here. Three pipes meeting at a junction: if 5 L/s enters from the left and 3 L/s enters from above, then 8 L/s must exit to the right. No "magic" — conservation law.

**Why it's powerful:** KCL works at every timescale and every topology. Even in AC circuits with reactive components, KCL holds instantaneously at every node. It is the starting point of nodal analysis — the most general circuit-solving technique.

## Derivation
**From conservation of charge:**

The charge at a node $q_{node}(t)$ changes only if current flows in or out:
$$\frac{dq_{node}}{dt} = \sum_k I_k$$

For a pure node (not a capacitor plate), charge cannot accumulate: $dq_{node}/dt = 0$:
$$\sum_k I_k = 0$$

This is KCL. The constraint $dq_{node}/dt = 0$ is the lumped-circuit approximation: we assume that the node has no physical extent, so it stores no charge. (This breaks down at microwave frequencies where physical dimensions become comparable to wavelength — but for all DC and low-frequency circuits, it holds exactly.)

**Formal statement with sign convention:**

Assign a reference direction to each current. If current $I_k$ flows into the node, it contributes $+I_k$; if it flows out, $-I_k$:
$$I_1 + I_2 + \ldots + I_n = 0$$

If some $I_k$ values come out negative during solving, the actual current flows opposite the assumed direction — the sign tells you which way.

## Worked Example
**Problem:** In Baymax's sensor array, three currents meet at a power bus node:
- $I_1 = 120\,\text{mA}$ from the voltage regulator (entering node)
- $I_2 = 75\,\text{mA}$ into the Raspberry Pi (leaving node)
- $I_3 = ?$ into the motor controller (leaving node)

Applying KCL (define entering as positive):
$$I_1 - I_2 - I_3 = 0$$
$$120 - 75 - I_3 = 0$$
$$I_3 = 45\,\text{mA}$$

**More complex example:** At a node, four branches:
- $I_A = 2\,\text{A}$ entering
- $I_B = 0.8\,\text{A}$ entering
- $I_C = 1.5\,\text{A}$ leaving
- $I_D = ?$

$$2 + 0.8 - 1.5 - I_D = 0 \implies I_D = 1.3\,\text{A}\text{ (leaving)}$$

**Nodal analysis setup:** For a node V1 connected to:
- Ground through $R_1 = 1\,\text{k}\Omega$
- A 5 V source through $R_2 = 2\,\text{k}\Omega$
- Another node V2 through $R_3 = 1\,\text{k}\Omega$

KCL at V1 (currents out = 0):
$$\frac{V_1 - 0}{R_1} + \frac{V_1 - 5}{R_2} + \frac{V_1 - V_2}{R_3} = 0$$
This is one equation in the system. Apply KCL at V2 for a second equation; solve simultaneously.

## See Also
- [[Electric Charge]] — KCL is the direct expression of charge conservation at a node
- [[Electric Current]] — KCL is a statement about currents; understanding current is prerequisite
- [[Kirchhoff's Voltage Law]] — the companion law; KCL handles nodes, KVL handles loops
- [[Parallel Circuit]] — parallel circuits are directly analyzed using KCL (currents split between branches)
- [[Current Divider]] — the current divider formula is derived by applying KCL at a parallel junction
- [[Series Circuit]] — in series circuits, KCL trivially confirms same current everywhere (only one path)
- [[Conservation of Momentum]] — analogous conservation principle in mechanics; both arise from symmetry
