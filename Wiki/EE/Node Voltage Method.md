# Node Voltage Method

**One-liner:** The node voltage method is a systematic technique for solving any resistive circuit by assigning unknown voltages to each non-reference node and writing a KCL equation at each one, producing a solvable linear system.

## Core Idea
$$\sum_k \frac{V_{node} - V_{adjacent,k}}{R_k} = I_{source,in}$$
Choose one node as the reference (ground, 0 V). Assign variable $V_1, V_2, \ldots, V_n$ to every other node. At each unknown node, write KCL: the sum of currents *leaving* via resistors equals the sum of currents *entering* from current sources. Each "current leaving through resistor $R_k$" is expressed as $(V_{node} - V_{adjacent})/R_k$ by Ohm's Law. The result is $n$ equations in $n$ unknowns — solve by algebra or matrix methods.

## Why It Exists
In circuits with many components, ad-hoc application of KVL and KCL produces a tangle of equations with redundant variables. The node voltage method is a *guaranteed* systematic procedure that always generates exactly the right number of independent equations to solve the circuit. It is the foundation of all circuit simulators — SPICE internally builds a conductance matrix from nodal equations and inverts it numerically. For hand analysis, it is usually faster than mesh current methods when there are fewer nodes than loops.

## Real-World Applications
- **SPICE/LTspice simulation:** Every circuit simulator uses a variant of modified nodal analysis (MNA) — an extension of the node voltage method that handles voltage sources directly. Understanding the node voltage method means understanding what SPICE is actually computing.
- **Multi-load power distribution (Baymax):** To find the exact voltage at each subsystem's supply pin when there are PCB trace resistances between them, set up a nodal system: each trace is a resistor, each subsystem is a current source (constant current load), solve for node voltages.
- **Op-amp circuit analysis:** The inverting amplifier's gain is derived by node voltage method at the inverting input node (virtual ground). The summing amplifier uses KCL at a summing node — it's just nodal analysis with the op-amp's virtual-short constraint.
- **Sensor array biasing:** When multiple sensors share supply and return traces with non-zero resistance, node analysis reveals whether each sensor receives adequate voltage.

## Intuition
Every node in the circuit has a specific voltage relative to ground — this is physical reality. The node voltage method simply *names* those voltages as unknowns and then enforces the two physical laws that constrain them:
1. **Ohm's Law** tells you the current in each resistor: $(V_{high} - V_{low})/R$.
2. **KCL** tells you those currents must balance at every node.

Together they uniquely determine every node voltage. The beauty is that once you know all node voltages, you immediately know every current ($I = (V_a - V_b)/R$) and every power ($P = V \cdot I$) in the circuit.

**When to prefer nodal analysis over mesh analysis:**
- Circuits with many parallel branches (more loops than nodes) → nodal is faster
- Circuits driven by current sources → nodal is natural (current sources appear directly in KCL)
- Circuits already have a natural ground node → nodal is cleanest
- Circuits driven primarily by voltage sources → mesh may be simpler (use supernodes for voltage sources in nodal)

**Supernode:** When an independent voltage source connects two non-reference nodes ($V_a - V_b = V_s$), treat both nodes together as a "supernode." Write KCL for the combined supernode and add the voltage source constraint as a second equation.

## Derivation
**General procedure:**

**Step 1 — Identify nodes and choose reference.**
A node is any point where two or more components connect. Choose the node with the most connections (often the ground/negative rail) as the reference node (voltage = 0).

**Step 2 — Assign node voltages.**
Label remaining nodes $V_1, V_2, \ldots, V_n$. Nodes directly connected to voltage sources relative to ground are *known* immediately (e.g., $V_1 = V_s$ if a source connects from ground to node 1).

**Step 3 — Write KCL at each unknown node.**
Convention: sum of currents *leaving* the node = sum of currents *entering* from external sources.

For node $V_k$ connected to nodes $V_1, V_2, \ldots$ through resistors $R_{k1}, R_{k2}, \ldots$ and receiving current source $I_s$:
$$\frac{V_k - V_1}{R_{k1}} + \frac{V_k - V_2}{R_{k2}} + \cdots = I_s$$

**Step 4 — Organize as a linear system and solve.**
Group by unknowns:
$$V_k \left(\frac{1}{R_{k1}} + \frac{1}{R_{k2}} + \cdots\right) - \frac{V_1}{R_{k1}} - \frac{V_2}{R_{k2}} - \cdots = I_s$$

In matrix form: $\mathbf{G}\,\mathbf{V} = \mathbf{I}$, where $\mathbf{G}$ is the conductance matrix, $\mathbf{V}$ is the node voltage vector, and $\mathbf{I}$ is the source current vector. Solve: $\mathbf{V} = \mathbf{G}^{-1}\mathbf{I}$.

**Step 5 — Back-calculate branch currents and power.**
Once all $V_k$ are known: $I_{branch} = (V_a - V_b)/R_{branch}$, $P_{branch} = I_{branch}^2 R_{branch}$.

## Worked Example
**Problem:** Baymax's sensor power bus has three nodes. A 12 V regulator feeds node $V_1$ through trace resistance $R_1 = 0.5\,\Omega$. Node $V_1$ also connects to ground through the main processor modeled as current source $I_A = 0.8\,\text{A}$ (constant current load). Node $V_1$ connects to node $V_2$ through trace resistance $R_2 = 1\,\Omega$. Node $V_2$ connects to ground through sensor load $R_3 = 50\,\Omega$. Find $V_1$, $V_2$, and the voltage actually reaching the sensor.

**Circuit summary:**
- Voltage source: $V_s = 12\,\text{V}$ in series with $R_1 = 0.5\,\Omega$ feeding node $V_1$
- At $V_1$: processor draws $I_A = 0.8\,\text{A}$ to ground; trace $R_2 = 1\,\Omega$ connects to $V_2$
- At $V_2$: sensor $R_3 = 50\,\Omega$ to ground

**Step 1 — Set reference:** Ground = 0 V. Source voltage at node "S" before $R_1$: $V_S = 12\,\text{V}$ (known).

**Step 2 — Node voltages:** $V_1$ and $V_2$ are unknown.

**Step 3 — KCL at $V_1$** (currents leaving = currents entering):

Currents leaving $V_1$:
- Through $R_1$ back toward source: $(V_1 - 12)/0.5$ (this is negative if $V_1 < 12$, meaning current actually flows *into* $V_1$)
- Through $R_2$ to $V_2$: $(V_1 - V_2)/1$
- Processor current source: $I_A = 0.8\,\text{A}$ drawn to ground

Rewrite as (currents leaving) − (currents entering) = 0, or equivalently write currents entering = currents leaving:
$$\frac{12 - V_1}{R_1} = I_A + \frac{V_1 - V_2}{R_2}$$
$$\frac{12 - V_1}{0.5} = 0.8 + \frac{V_1 - V_2}{1}$$
$$24 - 2V_1 = 0.8 + V_1 - V_2$$
$$-3V_1 + V_2 = 0.8 - 24 = -23.2 \quad \cdots (1)$$

**Step 4 — KCL at $V_2$** (currents entering = currents leaving):
$$\frac{V_1 - V_2}{R_2} = \frac{V_2}{R_3}$$
$$\frac{V_1 - V_2}{1} = \frac{V_2}{50}$$
$$50(V_1 - V_2) = V_2$$
$$50V_1 - 50V_2 = V_2$$
$$50V_1 - 51V_2 = 0 \implies V_1 = \frac{51}{50}V_2 = 1.02\,V_2 \quad \cdots (2)$$

**Step 5 — Solve the system.**

Substitute (2) into (1):
$$-3(1.02\,V_2) + V_2 = -23.2$$
$$-3.06\,V_2 + V_2 = -23.2$$
$$-2.06\,V_2 = -23.2$$
$$V_2 = \frac{23.2}{2.06} \approx 11.26\,\text{V}$$

From (2): $V_1 = 1.02 \times 11.26 \approx 11.49\,\text{V}$

**Step 6 — Interpret results.**

- Supply node: 12 V. After trace $R_1$: $V_1 = 11.49\,\text{V}$ — $0.51\,\text{V}$ drop due to 0.8 A processor load through $0.5\,\Omega$ trace. ✓ Check: $(12 - 11.49)/0.5 = 1.02\,\text{A}$ into $V_1$. KCL: $0.8\,\text{A}$ to processor $+ (11.49-11.26)/1 = 0.23\,\text{A}$ to $V_2$ = $1.03\,\text{A}$ ✓ (rounding error).
- Sensor node: $V_2 = 11.26\,\text{V}$. Sensor current: $11.26/50 = 0.225\,\text{A}$.
- Sensor receives 11.26 V — if sensor needs ≥ 11 V, it's within spec.

**Power check:** Source supplies $I_{total} = (12 - 11.49)/0.5 \approx 1.02\,\text{A}$. $P_{source} = 12 \times 1.02 = 12.24\,\text{W}$. $P_{R1} = 1.02^2 \times 0.5 = 0.52\,\text{W}$. $P_{processor} = 11.49 \times 0.8 = 9.19\,\text{W}$. $P_{R2} = 0.23^2 \times 1 = 0.05\,\text{W}$. $P_{sensor} = 11.26^2/50 = 2.54\,\text{W}$. Total absorbed ≈ $0.52 + 9.19 + 0.05 + 2.54 = 12.30\,\text{W}$ ≈ $P_{source}$ ✓ (small rounding error).

## See Also
- [[Kirchhoff's Current Law]] — nodal analysis is KCL applied systematically to every node; the two are inseparable
- [[Ohm's Law]] — each branch current $(V_a - V_b)/R$ is Ohm's Law; node voltage method is Ohm's Law organized at scale
- [[Kirchhoff's Voltage Law]] — KVL is used to handle supernodes (voltage sources between nodes)
- [[Voltage]] — the node voltage method works because voltage is a well-defined potential at every point
- [[Electric Power]] — once node voltages are found, power in every element follows immediately
- [[Series Circuit]] — degenerate case: only one node between two resistors; nodal trivially gives the voltage divider
- [[Parallel Circuit]] — multiple branches between same two nodes; nodal analysis handles naturally via conductance summation
- [[Voltage Divider]] — the two-resistor series case is a single-node nodal analysis in disguise
