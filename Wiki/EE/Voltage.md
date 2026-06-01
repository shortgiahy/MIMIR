# Voltage

**One-liner:** Voltage is the electric potential difference between two points — the amount of energy required to move one coulomb of positive charge from one point to the other.

## Core Idea
$$V_{AB} = \frac{W_{AB}}{Q} = \frac{\Delta U_E}{Q}$$
Voltage is measured in Volts (V), where 1 V = 1 J/C. The critical insight is that **voltage is always a difference** — it is meaningless to say "the voltage at point A" without specifying a reference. When engineers say "the voltage is 5 V at a node," they mean 5 V relative to ground (which is defined as 0 V by convention). $V_{AB}$ means the potential at A minus the potential at B.

## Why It Exists
Current doesn't flow without a driving force. That force — the electric potential difference — is what we call voltage. It's the electrical analog of pressure in a fluid system or height difference in a gravitational system. Without voltage, electrons have no net tendency to flow in any direction. Voltage is what every power supply, battery, and signal source provides; it's the fundamental "cause" in the voltage→current→power chain. Engineers need it to specify operating conditions, design protection circuits, and analyze signal levels.

## Real-World Applications
- **Logic levels:** A microcontroller's GPIO pin reads "high" at 3.3 V and "low" at 0 V — both relative to the board's GND rail. If GND floats, the logic level is meaningless.
- **Sensor outputs:** A temperature sensor might output 0–3.3 V proportional to 0–100°C. The voltage encodes information; the ADC samples it.
- **Battery voltage:** A LiPo cell is "3.7 V nominal" — meaning 3.7 V between its positive terminal and negative terminal. The 3.7 V doesn't exist at either terminal in isolation; it only exists across them.
- **Baymax motor driver:** The H-bridge switches 24 V across motor windings. The PWM duty cycle controls the average voltage (and thus average current and torque). Understanding voltage as a difference explains why a floating motor terminal is undefined and dangerous.

## Intuition
**The gravity analogy (and why it's incomplete):** Gravitational potential energy per unit mass is $U/m = gh$. Voltage is electric potential energy per unit charge $U/Q$. Both describe how much energy a "test particle" gains or loses moving through a field. A ball released from height $h$ converts potential energy to kinetic energy — an electron released at a higher potential converts electrical PE to kinetic energy (and then to heat in a resistor).

**Where the analogy breaks:** Height is absolute — you can say "5 m above sea level." Voltage is never absolute at a single point; it's always a difference. A wire at 100 V relative to ground is exactly equivalent to a wire at 200 V if ground is redefined to be 100 V. What matters for electron motion is the *gradient* of potential — the electric field: $\vec{E} = -\nabla V$.

**The dangerous intuition:** People say "voltage at a node is 5 V." This is shorthand for "5 V above ground." Never lose track of the reference. Measuring between two points that are both "5 V above ground" gives 0 V — no current flows, no work is done, even though both are "at 5 V."

## Derivation
**From electric field:**
The electric field $\vec{E}$ exerts force $\vec{F} = q\vec{E}$ on charge $q$.

Work done moving charge $q$ from A to B:
$$W_{A \to B} = \int_A^B \vec{F} \cdot d\vec{l} = q \int_A^B \vec{E} \cdot d\vec{l}$$

Voltage (potential difference):
$$V_{AB} = V_A - V_B = -\int_A^B \vec{E} \cdot d\vec{l} = \int_B^A \vec{E} \cdot d\vec{l}$$

The negative sign: moving against the field (from − to + through a battery) requires doing work on the charge — that's what the battery does. Moving with the field (from + to − through a resistor) releases energy — that's what the resistor dissipates.

**For a uniform field (parallel plates):**
$$V = E \cdot d$$
where $d$ is plate separation. This is the geometry of a capacitor.

**From energy conservation:**
The work done on charge $q$ moving through potential difference $V$:
$$W = qV$$
This is the root of $P = IV$ (power = charge per second × energy per charge = energy per second).

## Worked Example
**Problem:** Baymax's IR distance sensor uses a voltage divider to condition its output. The sensor's raw output swings 0–5 V, but the microcontroller ADC only accepts 0–3.3 V. What resistor ratio sets the divider?

We need $V_{out}/V_{in} = 3.3/5.0 = 0.66$.

Using the voltage divider formula (see [[Voltage Divider]]):
$$\frac{V_{out}}{V_{in}} = \frac{R_2}{R_1 + R_2} = 0.66$$

Choosing $R_2 = 6.6\,\text{k}\Omega$, $R_1 = 3.4\,\text{k}\Omega$ (use standard values: $R_2 = 6.8\,\text{k}\Omega$, $R_1 = 3.3\,\text{k}\Omega$):
$$\frac{V_{out}}{V_{in}} = \frac{6800}{3300 + 6800} = \frac{6800}{10100} = 0.673$$

Maximum output: $5.0 \times 0.673 = 3.37\,\text{V}$ — just above 3.3 V. Use $R_2 = 6.2\,\text{k}\Omega$ for $3.07\,\text{V}$ max — safely within spec with margin.

## See Also
- [[Electric Charge]] — voltage is energy per unit charge; charge is what voltage acts on
- [[Electric Current]] — voltage drives current; the two are related by resistance
- [[Ohm's Law]] — $V = IR$; the quantitative relationship between voltage, current, and resistance
- [[Kirchhoff's Voltage Law]] — the sum of voltages around any closed loop is zero
- [[Voltage Divider]] — the most important single application of voltage as a ratio
- [[Electric Power]] — $P = IV$; power is current times voltage
- [[Gravitational Potential Energy]] — exact structural analog: $U_g/m = gh$ vs $V = U_E/Q$
- [[Electric Field]] — voltage is the spatial integral of the electric field
