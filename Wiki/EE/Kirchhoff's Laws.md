# Kirchhoff's Laws

**One-liner:** KCL says current into a node equals current out; KVL says voltages around any closed loop sum to zero — together, these two laws let you solve any resistive circuit systematically.

## Why It Exists

Ohm's Law gives you a relationship for a single resistor. Real circuits have dozens or hundreds of components — multiple voltage sources, branching paths, feedback loops. You need a systematic method that can handle any network topology, not just a single component.

Gustav Kirchhoff published these laws in 1845, building on the work of Ohm and Faraday. He was studying electrical networks and realized that the "rules" engineers used ad hoc could be derived rigorously from two fundamental conservation laws:

- **KCL** follows from **conservation of charge**: charge cannot appear or disappear at a point in a circuit.
- **KVL** follows from **conservation of energy**: the work done moving a charge around any closed loop must be zero (otherwise you could extract unlimited energy by going around the loop repeatedly — perpetual motion).

These are not approximations or heuristics. They are exact consequences of Maxwell's equations applied to lumped circuit elements (see Gotchas for when lumped models break down).

## The Concept

### Kirchhoff's Current Law (KCL)

**Statement:** The algebraic sum of all currents entering and leaving a node is zero.

$$\sum_{k} I_k = 0 \quad \text{at every node}$$

A **node** is any junction point in a circuit — any point where two or more components meet.

Define a sign convention: currents *into* the node are positive, currents *out of* the node are negative (or vice versa — pick one and be consistent).

```
        I₁ →     I₂ →
    ──────────●──────────
              |
              ↓ I₃
```

KCL says: $I_1 - I_2 - I_3 = 0$, or equivalently $I_1 = I_2 + I_3$.

The physical content: charge does not accumulate at a node. Whatever charge per second flows in, the same amount per second flows out. This is direct from $\nabla \cdot \mathbf{J} = -\partial \rho / \partial t$; in steady state ($\partial \rho / \partial t = 0$), the divergence of current density is zero — no net charge accumulation anywhere.

### Kirchhoff's Voltage Law (KVL)

**Statement:** The algebraic sum of all voltages around any closed loop is zero.

$$\sum_{k} V_k = 0 \quad \text{around any closed loop}$$

A **loop** is any closed path you can trace through a circuit, starting and ending at the same node.

Sign convention: as you traverse the loop in a chosen direction, a voltage rise (going from − to + through a source, or entering a resistor against the current direction) is positive; a voltage drop (going from + to − through a source, or entering a resistor in the current direction) is negative.

```
    +  V₁  -     +  V₂  -
  ──[  R₁  ]────[  R₂  ]──┐
  │                         │
  │+                        │
 [Vs]  (battery)            │
  │-                        │
  └─────────────────────────┘
```

Traversing the loop clockwise starting at the bottom-left: 
Going up through battery: $+V_s$
Going right through $R_1$: $-V_1$ (voltage drops in direction of current)
Going right through $R_2$: $-V_2$

KVL: $V_s - V_1 - V_2 = 0 \implies V_s = V_1 + V_2$

The physical content: electric potential is a conservative field (at low frequencies, where radiation effects are negligible). The work done moving a unit charge around a closed path in a conservative field is exactly zero — you return to where you started with the same potential energy. KVL is a direct statement of this.

### Systematic Circuit Analysis: Node Voltage Method

For complex circuits, you need a systematic procedure. The **node voltage method** is the most broadly applicable.

**Procedure:**
1. Identify all nodes. Choose one as the reference node (ground, 0V).
2. Label the unknown node voltages $V_1, V_2, \ldots$
3. Write a KCL equation at each non-reference node: sum of currents leaving the node = 0. Express each branch current using Ohm's Law: $I = (V_{node} - V_{adjacent}) / R$.
4. Solve the resulting system of linear equations.

**Example circuit:**

```
     V₁        V₂
  ●──────[R₁]──────●
  │                │
 [Vs]             [R₂]    [R₃ between V₁ and V₂]
  │                │
  ●────────────────●
        GND (0V)
```

At node $V_1$ (KCL, currents leaving):

$$\frac{V_1 - V_s}{0} + \frac{V_1 - V_2}{R_1} = 0$$

Wait — if $V_s$ is an ideal voltage source, then $V_1$ is *fixed* by the source: $V_1 = V_s$. This illustrates that a voltage source between a node and ground makes that node voltage known immediately — you don't need a KCL equation there.

### Systematic Circuit Analysis: Mesh Current Method

The **mesh current method** is the KVL-based dual of the node voltage method.

**Procedure:**
1. Identify all independent meshes (loops that contain no other loops inside them — the "windows" of the circuit).
2. Assign a mesh current to each mesh (typically clockwise).
3. Write a KVL equation for each mesh: traverse the loop, summing voltage rises and drops in terms of the mesh currents.
4. Solve the resulting system.

The number of mesh equations equals the number of independent loops. The number of node equations equals (number of nodes − 1). Which method is easier depends on circuit topology.

### Why KCL and KVL Are Sufficient

Any linear resistive circuit can be fully solved using only KCL, KVL, and Ohm's Law. These three together form a complete system:
- $N-1$ independent KCL equations (where $N$ = number of nodes)
- $B - N + 1$ independent KVL equations (where $B$ = number of branches)
- Ohm's Law for each resistor: $V_k = I_k R_k$

The total number of equations exactly equals the total number of unknowns (branch currents and branch voltages). The system is exactly determined.

### Connection to Robotics and Sensors

In robot sensor circuits, KCL appears constantly:
- **Current summing:** Multiple sensors sharing a power rail — KCL tells you the total current the supply must provide.
- **Op-amp virtual ground:** The inverting amplifier uses KCL at the op-amp's inverting input (the "virtual ground" node) to derive the gain equation.
- **Motor driver H-bridge:** Analyzing which current path is active requires KCL at the motor node.

KVL appears in:
- **Voltage divider analysis:** The output voltage of a voltage divider is derived by KVL around the divider loop.
- **Battery and load modeling:** A real battery has internal resistance; KVL around the battery-load loop gives $V_{load} = V_{battery} - I \cdot R_{internal}$, explaining why battery voltage sags under load.

## Intuition

**KCL is traffic conservation.** If you stand at an intersection and count every car entering and leaving, the counts must balance — cars don't appear or disappear in the middle of the intersection. Electrons are the cars. Nodes are intersections.

**KVL is altitude accounting.** If you walk a hiking loop and return to your starting point, your total elevation change must be zero. Every climb is offset by a descent. Voltage is altitude. Going "uphill" through a battery and "downhill" through resistors, you must return to zero net change. If you could traverse a loop and end up at higher voltage than where you started, you would extract unlimited energy — which contradicts conservation of energy.

For a deeper model: voltage is a scalar field. In electrostatics, $\oint \mathbf{E} \cdot d\mathbf{l} = 0$ around any closed path — the electric field is conservative. KVL is just this statement written in terms of lumped circuit voltages. The "lumped" assumption (that all the field effects are contained within the components, not floating around in space) is what allows you to use KVL as a circuit law rather than needing to solve full Maxwell's equations.

## Key Formula / Rule

**KCL** (at any node):

$$\sum I_{in} = \sum I_{out}$$

or equivalently:

$$\sum_{k} I_k = 0 \quad \text{(with sign convention)}$$

**KVL** (around any closed loop):

$$\sum_{k} V_k = 0 \quad \text{(algebraic sum, with sign convention)}$$

Combined with Ohm's Law $V_k = I_k R_k$, these three equations are sufficient to solve any linear resistive circuit.

## Worked Example

**Problem:** In the circuit below, find the current through each resistor and the voltage at node A.

```
        A
   ┌────●────┐
   │    │    │
  [12V] [4Ω] [6Ω]
   │    │    │
   └────●────┘
       GND
```

A 12V voltage source on the left, a 4Ω resistor in the middle, and a 6Ω resistor on the right. All three share the top node A and the bottom node GND.

**Step 1: Label and identify.**

- Node A is the top node (unknown voltage $V_A$)
- GND = 0V (reference)
- The voltage source forces $V_A = 12\text{ V}$ directly

Wait — with an ideal voltage source from GND to node A, $V_A$ is fixed at 12V. This is the simplest case.

**Let me modify for a more instructive example:**

```
     R₁=3Ω      A       R₂=6Ω
┌────[===]────●────[===]────┐
│                            │
│+                           │
[12V]                       [R₃=4Ω]
│-                           │
└────────────────────────────┘
                GND
```

**Step 1: Identify nodes.** Two non-reference nodes: one at the junction between $V_s$ and $R_1$ (call it $V_B = 12\text{ V}$, because the source fixes it), and node A between $R_1$, $R_2$, and $R_3$. The source is between GND and $V_B$, so $V_B = 12\text{ V}$. Node A is unknown.

**Step 2: Write KCL at node A.**

Currents leaving node A through each resistor sum to zero:

$$\frac{V_A - V_B}{R_1} + \frac{V_A - 0}{R_2} + \frac{V_A - 0}{R_3} = 0$$

Wait — let me be precise. $R_2$ and $R_3$ both connect node A to GND. $R_1$ connects node A to $V_B = 12V$.

KCL at node A (currents leaving):

$$\frac{V_A - 12}{3} + \frac{V_A}{6} + \frac{V_A}{4} = 0$$

**Step 3: Solve.**

Multiply through by 12 (the LCM of 3, 6, 4):

$$4(V_A - 12) + 2V_A + 3V_A = 0$$

$$4V_A - 48 + 2V_A + 3V_A = 0$$

$$9V_A = 48$$

$$V_A = \frac{48}{9} = \frac{16}{3} \approx 5.33\text{ V}$$

**Step 4: Find branch currents.**

$$I_{R_1} = \frac{V_B - V_A}{R_1} = \frac{12 - 16/3}{3} = \frac{(36-16)/3}{3} = \frac{20/3}{3} = \frac{20}{9} \approx 2.22\text{ A}$$

$$I_{R_2} = \frac{V_A}{R_2} = \frac{16/3}{6} = \frac{16}{18} = \frac{8}{9} \approx 0.89\text{ A}$$

$$I_{R_3} = \frac{V_A}{R_3} = \frac{16/3}{4} = \frac{16}{12} = \frac{4}{3} \approx 1.33\text{ A}$$

**Step 5: Verify with KCL.**

$I_{R_1}$ enters node A; $I_{R_2}$ and $I_{R_3}$ leave it:

$$I_{R_1} = I_{R_2} + I_{R_3}$$

$$\frac{20}{9} = \frac{8}{9} + \frac{12}{9} = \frac{20}{9} \checkmark$$

**Step 6: Verify with KVL.**

Going around the outer loop (source → $R_1$ → $R_3$ → back to source):

$$+12 - I_{R_1} \cdot R_1 - I_{R_3} \cdot R_3 = 0$$

$$+12 - \frac{20}{9} \cdot 3 - \frac{4}{3} \cdot 4 = 12 - \frac{60}{9} - \frac{16}{3} = 12 - \frac{20}{3} - \frac{16}{3} = 12 - \frac{36}{3} = 12 - 12 = 0 \checkmark$$

## Gotchas

**KVL assumes a lumped circuit model.** At high frequencies (radio frequency and above), the electric and magnetic fields are no longer entirely contained within the circuit components — they radiate into space. In that regime, $\oint \mathbf{E} \cdot d\mathbf{l} \neq 0$ and KVL breaks down. This is why RF circuit design requires transmission line theory, not just KVL.

**Faraday's law can appear to violate KVL.** If a changing magnetic flux threads a loop, an EMF is induced (Faraday's law) and KVL in the naive form seems to give the wrong answer. The resolution is that you must include the EMF source explicitly as a component, or use the full Maxwell's equations form. This is a common confusion in EE courses.

**Sign conventions must be consistent.** When writing KCL, decide once whether currents into or out of a node are positive. When writing KVL, decide once which traversal direction gives positive voltage rises. Mixing conventions mid-problem guarantees wrong answers. Checking units and signs after every step is good practice.

**Current can be negative.** If you assume a current direction and solve KCL/KVL to get a negative value, that means the actual current flows in the opposite direction from your assumption. This is perfectly valid — the math handles it automatically. Don't re-do the problem assuming a different direction; just interpret the negative sign correctly.

**KCL applies to any closed surface, not just points.** A useful generalization: the sum of currents entering any closed surface is zero. This lets you analyze subsections of a circuit without solving the whole thing.

## See Also
- [[Voltage, Current, and Resistance]] — the fundamental quantities that KCL and KVL reason about
- [[Ohm's Law]] — combines with KVL/KCL to give a complete system of equations for resistive circuits
- [[Series and Parallel Circuits]] — the simplest applications of KVL (series) and KCL (parallel)
- [[Power in Circuits]] — once you have currents and voltages from KVL/KCL, P = IV gives power in each element
