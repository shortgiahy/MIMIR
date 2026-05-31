# Series and Parallel Circuits

**One-liner:** Resistors in series add directly because the same current flows through all of them; resistors in parallel add as reciprocals because the same voltage appears across all of them — both rules derive directly from Kirchhoff's Laws.

## Why It Exists

Real circuits rarely have a single component. You need to combine resistors, capacitors, and other elements into networks. Without a way to simplify these networks, even a circuit with five resistors would require solving a system of equations from scratch every time.

Series and parallel combination rules let you collapse a network of components into a single equivalent component — one with the same terminal behavior as the whole network. This is the first step in circuit simplification, and it recurs at every level: from combining resistors in a lab circuit to combining impedances in a filter design to combining subsystems in a robot power distribution network.

## The Concept

### Series Connection

Two or more elements are in **series** if they share exactly one node with each other and that node connects to nothing else — meaning the same current must flow through every element in the chain.

```
     I →     I →     I →
──[R₁]──[R₂]──[R₃]──
```

All three resistors carry the same current $I$ (by KCL: there's only one path, so no current can go anywhere else).

By KVL around the loop with a voltage source $V_s$:

$$V_s = V_1 + V_2 + V_3 = IR_1 + IR_2 + IR_3 = I(R_1 + R_2 + R_3)$$

The equivalent resistance $R_{eq}$ that would produce the same current from $V_s$ is:

$$R_{eq} = R_1 + R_2 + R_3$$

**General rule for $n$ resistors in series:**

$$R_{eq} = \sum_{k=1}^{n} R_k = R_1 + R_2 + \cdots + R_n$$

**Why does resistance increase?** Each resistor is an additional obstacle for the current. Longer path → more collisions → more resistance. Adding resistors in series is like adding length to a wire.

### Parallel Connection

Two or more elements are in **parallel** if they share *both* nodes — meaning the same voltage appears across every element.

```
        ┌──[R₁]──┐
   A ───┤──[R₂]──├─── B
        └──[R₃]──┘
```

Voltage across each resistor: $V_{AB} = V$ (same for all, because they share the same two terminal nodes).

By KCL at node A, total current from the source splits:

$$I = I_1 + I_2 + I_3 = \frac{V}{R_1} + \frac{V}{R_2} + \frac{V}{R_3} = V\left(\frac{1}{R_1} + \frac{1}{R_2} + \frac{1}{R_3}\right)$$

The equivalent resistance:

$$\frac{1}{R_{eq}} = \frac{1}{R_1} + \frac{1}{R_2} + \frac{1}{R_3}$$

$$R_{eq} = \frac{1}{\dfrac{1}{R_1} + \dfrac{1}{R_2} + \dfrac{1}{R_3}}$$

**Special case — two resistors in parallel:** The "product over sum" formula:

$$R_{eq} = \frac{R_1 R_2}{R_1 + R_2}$$

**Why does resistance decrease?** Parallel paths give current more routes to flow. Adding a parallel resistor is like widening a pipe — more cross-section, less resistance. The equivalent resistance is always *less than the smallest individual resistor* in the parallel combination.

### Conductance Form (More Elegant for Parallel)

Conductance $G = 1/R$ is measured in siemens (S). In terms of conductance:

- **Series:** $\frac{1}{G_{eq}} = \frac{1}{G_1} + \frac{1}{G_2} + \cdots$ (conductances in series add as reciprocals)
- **Parallel:** $G_{eq} = G_1 + G_2 + \cdots$ (conductances in parallel add directly)

This is the mathematical dual of the resistance rules — series and parallel are symmetric in this sense. Some textbooks use admittance (complex conductance) in AC circuit analysis, which makes parallel impedance combination clean.

### The Voltage Divider

The **voltage divider** is the single most important series circuit configuration in electronics. It appears in sensor interfaces, signal conditioning, ADC input scaling, biasing circuits, and Wheatstone bridges.

Two resistors in series with a voltage source $V_{in}$:

```
      V_in
       │
      [R₁]
       │
       ●──── V_out
       │
      [R₂]
       │
      GND
```

The same current $I = V_{in}/(R_1 + R_2)$ flows through both resistors. The output voltage (across $R_2$):

$$V_{out} = I \cdot R_2 = \frac{V_{in}}{R_1 + R_2} \cdot R_2 = V_{in} \cdot \frac{R_2}{R_1 + R_2}$$

$$\boxed{V_{out} = V_{in} \cdot \frac{R_2}{R_1 + R_2}}$$

This is a fraction of $V_{in}$ — always less than (or equal to) $V_{in}$. The output voltage is determined by the ratio of the resistors, not their absolute values.

**Critical warning:** This formula is only valid when *no significant current is drawn from the output node* (the "load" is high impedance). When a load $R_L$ is connected, it appears in parallel with $R_2$, changing the effective resistance and therefore the output voltage. This is called **loading effect**, and it is a pervasive source of errors in circuit design.

With load $R_L$ connected:

$$V_{out,loaded} = V_{in} \cdot \frac{R_2 \| R_L}{R_1 + R_2 \| R_L}$$

where $R_2 \| R_L = R_2 R_L / (R_2 + R_L)$.

### The Current Divider

The dual of the voltage divider: two resistors in parallel, with a known total current $I_{in}$ entering the parallel combination.

```
        ┌──[R₁]──┐
I_in ───┤         ├─── (returns to source)
        └──[R₂]──┘
```

Both resistors share the same voltage $V = I_{in} \cdot R_{eq} = I_{in} \cdot \frac{R_1 R_2}{R_1 + R_2}$.

Current through $R_1$:

$$I_1 = \frac{V}{R_1} = I_{in} \cdot \frac{R_2}{R_1 + R_2}$$

Current through $R_2$:

$$I_2 = \frac{V}{R_2} = I_{in} \cdot \frac{R_1}{R_1 + R_2}$$

Note: each branch current is proportional to the *other* resistor. The smaller resistor carries more current — it's the easier path.

### Series-Parallel Reduction

Most real circuits combine series and parallel configurations. The strategy is to repeatedly simplify sub-networks, replacing groups of resistors with their equivalent resistance, until the whole circuit reduces to a single equivalent resistor.

**Algorithm:**
1. Identify groups that are clearly in series (same current) or clearly in parallel (same two terminal nodes).
2. Combine each group into its equivalent.
3. Redraw the simplified circuit.
4. Repeat until fully simplified.
5. Use Ohm's Law to find the total current from the source.
6. Work backwards to find voltages and currents in each original branch.

This backwards expansion step is critical: once you know the total current, you can find the voltage across each sub-network, then the currents within each sub-network, and so on down to individual components.

### Robotic Applications

**Voltage dividers for sensors:** Many sensors (thermistors, photoresistors, strain gauges, potentiometers) work as variable resistors. A voltage divider with a fixed resistor converts the sensor's changing resistance into a changing voltage that a microcontroller can read with an ADC.

**Motor winding resistance:** A brushed DC motor winding can be modeled as a resistor (winding resistance) in series with a voltage source (back-EMF). The combination rules apply to analyze stall current vs. running current.

**Power supply distribution:** Multiple loads in parallel on a power rail — the total current the supply must provide is the sum of all individual load currents (KCL / parallel combination).

**Wheatstone bridge:** Four resistors in a diamond configuration (a combination of series and parallel) used to detect tiny resistance changes in strain gauges. This is how force and torque sensors in robot joints work.

## Intuition

**Series: resistors don't share.** In series, there is only one path for current. Every single electron must pass through every single resistor. Each resistor adds its own opposition. Total opposition = sum of individual oppositions.

**Parallel: resistors share the burden.** In parallel, current has multiple paths. Some electrons go through $R_1$, others go through $R_2$. The voltage (the driving force) is the same on all paths, but the total current is split. Adding more paths makes it easier overall. It is impossible to make a parallel combination *more* resistive by adding another parallel resistor — you can only make it equal (open circuit in parallel with $R$ gives $R$) or less resistive.

For the voltage divider: think of two resistors as competing for the voltage. $R_2$ gets a fraction of the total voltage proportional to its share of the total resistance. If $R_2 \ll R_1$, it gets almost no voltage. If $R_2 \gg R_1$, it gets almost all of it.

A useful sanity check: **for two equal resistors in parallel, the equivalent resistance is exactly half of one of them.** $R \| R = R/2$. This follows directly from the formula: $R^2 / (2R) = R/2$.

## Key Formula / Rule

**Series:**
$$R_{series} = R_1 + R_2 + \cdots + R_n$$

**Parallel:**
$$\frac{1}{R_{parallel}} = \frac{1}{R_1} + \frac{1}{R_2} + \cdots + \frac{1}{R_n}$$

**Two resistors in parallel (shortcut):**
$$R_1 \| R_2 = \frac{R_1 R_2}{R_1 + R_2}$$

**Voltage divider:**
$$V_{out} = V_{in} \cdot \frac{R_2}{R_1 + R_2}$$

**Current divider (two resistors):**
$$I_1 = I_{total} \cdot \frac{R_2}{R_1 + R_2} \qquad I_2 = I_{total} \cdot \frac{R_1}{R_1 + R_2}$$

## Worked Example

**Problem:** In the circuit below, find the equivalent resistance seen by the source, the total current drawn, and the current through the 6Ω resistor.

```
          2Ω         
   12V ──[===]──┬──[4Ω]──┬──── GND
                │         │
               [6Ω]      [12Ω]
                │         │
                └─────────┘
```

So: 2Ω in series with the parallel combination of [4Ω in series with 12Ω] and [6Ω].

Wait — let me re-read the topology. The circuit is:

- 12V source
- 2Ω resistor in series with the rest
- After the 2Ω: node splits into two branches back to GND
  - Branch 1: 6Ω to GND
  - Branch 2: 4Ω then 12Ω to GND (4Ω and 12Ω are in series)

**Step 1: Simplify branch 2.**

4Ω and 12Ω are in series:

$$R_{branch2} = 4 + 12 = 16\ \Omega$$

**Step 2: Combine the two parallel branches.**

$6\ \Omega \| 16\ \Omega$:

$$R_{parallel} = \frac{6 \times 16}{6 + 16} = \frac{96}{22} = \frac{48}{11} \approx 4.36\ \Omega$$

**Step 3: Add the series 2Ω.**

$$R_{eq} = 2 + \frac{48}{11} = \frac{22}{11} + \frac{48}{11} = \frac{70}{11} \approx 6.36\ \Omega$$

**Step 4: Find total current from source.**

$$I_{total} = \frac{V_s}{R_{eq}} = \frac{12}{\frac{70}{11}} = \frac{12 \times 11}{70} = \frac{132}{70} = \frac{66}{35} \approx 1.886\text{ A}$$

**Step 5: Find voltage across the parallel combination.**

$$V_{parallel} = I_{total} \times R_{parallel} = \frac{66}{35} \times \frac{48}{11} = \frac{66 \times 48}{35 \times 11} = \frac{3168}{385} = \frac{288}{35} \approx 8.23\text{ V}$$

Alternatively: $V_{parallel} = V_s - I_{total} \times R_{2\Omega} = 12 - \frac{66}{35} \times 2 = 12 - \frac{132}{35} = \frac{420 - 132}{35} = \frac{288}{35}$ ✓

**Step 6: Find current through 6Ω.**

Both parallel branches share $V_{parallel}$:

$$I_{6\Omega} = \frac{V_{parallel}}{6} = \frac{288/35}{6} = \frac{288}{210} = \frac{48}{35} \approx 1.37\text{ A}$$

**Step 7: Verify with KCL.**

$$I_{branch2} = \frac{V_{parallel}}{16} = \frac{288/35}{16} = \frac{288}{560} = \frac{18}{35} \approx 0.514\text{ A}$$

$$I_{6\Omega} + I_{branch2} = \frac{48}{35} + \frac{18}{35} = \frac{66}{35} = I_{total} \checkmark$$

## Gotchas

**"In parallel" requires sharing *both* nodes.** Two resistors sharing only one node are not in parallel. Check carefully. A common error is assuming two resistors are in parallel because they are drawn next to each other, when in fact there is another element in the path.

**Loading effect destroys the voltage divider formula.** The standard voltage divider formula $V_{out} = V_{in} \cdot R_2/(R_1+R_2)$ assumes nothing is connected to the output node drawing current. Any load creates a parallel combination with $R_2$ and changes the ratio. For a voltage divider to "hold" under load, either: (a) the source impedance (effectively $R_1$) must be much smaller than the load, or (b) you add a buffer amplifier (op-amp voltage follower) after the divider to isolate it from the load.

**Equivalent resistance is always less than the smallest resistor in a parallel combination.** If you calculate a parallel equivalent and it comes out larger than any of the individual resistors, you made an arithmetic error.

**Order of simplification matters for back-solving.** After reducing to find $R_{eq}$ and total current, you must carefully reverse your simplification steps to find voltages and currents in each branch. A common error is forgetting what was combined with what.

**Series/parallel reduction doesn't work on all circuits.** Circuits with bridges (like the Wheatstone bridge) cannot be simplified by series/parallel reduction alone. They require the full KCL/KVL analysis or the use of delta-wye (Δ-Y) transformations. Don't assume every multi-resistor circuit can be reduced by inspection.

## See Also
- [[Kirchhoff's Laws]] — the derivation of series and parallel rules comes directly from KVL (series) and KCL (parallel)
- [[Ohm's Law]] — every voltage divider and current divider calculation uses V = IR
- [[Voltage, Current, and Resistance]] — what the quantities being divided actually are
- [[Power in Circuits]] — power dissipation in series vs. parallel configurations has important design implications
