# Series Circuit

**One-liner:** In a series circuit, all components share the same current, and the total voltage is the sum of individual voltage drops.

## Core Idea
$$R_{total} = R_1 + R_2 + \cdots + R_n$$
$$I_{total} = I_1 = I_2 = \cdots = I_n$$
$$V_{total} = V_1 + V_2 + \cdots + V_n$$
Components are connected end-to-end, forming a single current path. There is only one route for current to flow. Total resistance is additive. Each component drops a portion of the total voltage proportional to its resistance (via Ohm's Law).

## Why It Exists
Not all circuits have a single load — real systems have multiple components connected in various configurations. The series configuration is the simplest non-trivial circuit topology. It appears everywhere: current-limiting resistors in series with LEDs, filter networks, battery cells stacked to increase voltage, and wiring harnesses. Understanding series behavior is prerequisite to analyzing any multi-component circuit.

## Real-World Applications
- **LED current limiting:** LED + resistor in series. Single current path ensures the same current flows through both. Resistor limits current; LED drops its forward voltage; Ohm's Law on the resistor determines current. Classic series circuit.
- **Battery stacking:** Two 3.7 V LiPo cells in series = 7.4 V. Identical current flows through both cells. Voltage adds. Baymax uses this for higher motor voltage.
- **Fuse + motor:** Fuse and motor in series on a power rail. Any overcurrent blows the fuse, protecting the motor. The fuse's low resistance (tens of mΩ) drops negligible voltage in normal operation.
- **Voltage sensing:** A series resistor with a sensor measures current indirectly (via Ohm's Law on the resistor). The same current flows through sensor and sense resistor.

## Intuition
Think of a single-lane road: all cars (electrons) must pass through every checkpoint (component) in sequence. If you add more checkpoints (more resistors), the road gets more congested (more resistance), slowing all cars equally. There's no "shortcut" — every electron must go through every component.

The voltage drops tell you where the energy goes: if most resistance is in one component, most voltage drops across it, most energy is dissipated there. A series resistor "steals" voltage from the load — this is why voltage regulators and supply rails need low series resistance.

## Derivation
**From KVL:**

Traversing the loop (single path), KVL states:
$$V_s - V_1 - V_2 - \cdots - V_n = 0$$
$$V_s = V_1 + V_2 + \cdots + V_n$$

Applying Ohm's Law to each: $V_k = I \cdot R_k$ (same current $I$ through all):
$$V_s = I R_1 + I R_2 + \cdots + I R_n = I(R_1 + R_2 + \cdots + R_n)$$

Defining total resistance:
$$R_{total} = R_1 + R_2 + \cdots + R_n$$

So: $V_s = I \cdot R_{total}$, confirming Ohm's Law for the series combination.

**Why current is the same:** KCL at any intermediate node shows no other branches — current entering equals current leaving, so $I$ is constant throughout.

## Worked Example
**Problem:** Baymax uses an LED indicator (red, $V_f = 2.0\,\text{V}$, $I_f = 20\,\text{mA}$) powered from a 5 V GPIO pin. What series resistor is needed?

Step 1 — voltage across resistor (KVL):
$$V_R = V_{supply} - V_f = 5.0 - 2.0 = 3.0\,\text{V}$$

Step 2 — resistor value (Ohm's Law):
$$R = \frac{V_R}{I_f} = \frac{3.0}{0.020} = 150\,\Omega$$

Use standard value: $150\,\Omega$ (exact match available in E24 series).

Step 3 — power in resistor:
$$P_R = I^2 R = (0.020)^2 \times 150 = 0.06\,\text{W} = 60\,\text{mW}$$

Use a 1/8 W resistor (125 mW rated) — fine with 2× margin.

**Three-element series:** $V = 12\,\text{V}$, $R_1 = 100\,\Omega$, $R_2 = 200\,\Omega$, $R_3 = 300\,\Omega$.

$$R_{total} = 600\,\Omega,\quad I = 12/600 = 20\,\text{mA}$$
$$V_1 = 0.02 \times 100 = 2\,\text{V},\quad V_2 = 4\,\text{V},\quad V_3 = 6\,\text{V}$$
$$2 + 4 + 6 = 12\,\text{V} \checkmark$$

## See Also
- [[Kirchhoff's Voltage Law]] — series circuits are the direct application of KVL: voltages add
- [[Kirchhoff's Current Law]] — confirms same current everywhere (no nodes to split at)
- [[Ohm's Law]] — used to find voltage across each element: $V_k = I R_k$
- [[Voltage Divider]] — two resistors in series with a tap point form a voltage divider
- [[Parallel Circuit]] — the other fundamental topology; currents add instead of voltages
- [[Resistance]] — series resistances add directly
- [[Electric Power]] — total power equals the sum of powers in each series element
