# Ohm's Law

**One-liner:** Ohm's Law states that for an ohmic material, the voltage across a resistor is directly proportional to the current through it: $V = IR$.

## Core Idea
$$V = IR$$
Voltage (V), current (I), and resistance (R) are related linearly in ohmic materials. Rearranged: $I = V/R$ (current is voltage divided by resistance) and $R = V/I$ (resistance is the ratio of voltage to current). The law is empirical for many materials over limited ranges, but it follows from a microscopic derivation via the Drude model for ideal conductors.

## Why It Exists
Every practical circuit requires predicting how much current will flow when a voltage is applied. Ohm's Law is that prediction tool. Before it, circuit design was guesswork. After it, engineers could calculate — precisely — operating currents, voltage drops across components, power dissipation, and biasing points. It is the single most-used relationship in analog electronics. Every resistor calculation, LED current limit, sensor biasing network, and motor driver operating point starts here.

## Real-World Applications
- **Current limiting for Baymax LEDs:** $R = (5V - 2.1V) / 20\,\text{mA} = 145\,\Omega$ — a direct Ohm's Law calculation.
- **MOSFET on-resistance:** In motor drivers, the MOSFET has $R_{DS(on)} \approx 5\text{–}50\,\text{m}\Omega$. At 10 A: $V_{drop} = IR_{DS(on)} = 10 \times 0.005 = 50\,\text{mV}$. Knowing this keeps the motor voltage accurate.
- **Voltage drop on supply rails:** Every trace, connector, and battery internal resistance drops voltage under load. Ohm's Law predicts how much.
- **ADC input protection:** A 10 kΩ series resistor limits fault current if a voltage spike hits the ADC pin: $I_{fault} = 5V / 10\,\text{k}\Omega = 0.5\,\text{mA}$ — safely below the ADC's 10 mA absolute max.

## Intuition
Ohm's Law is the hydraulic pressure–flow analogy made quantitative:
- Voltage is the pressure differential between two points
- Current is the flow rate
- Resistance is the pipe's narrowness and roughness

Double the pressure (voltage) → double the flow (current). Double the friction (resistance) → half the flow. This works perfectly for water in laminar flow, and for electrons in ohmic conductors at moderate fields and temperatures.

**Where the analogy breaks down — when Ohm's Law fails:**
Ohm's Law is not a universal law of nature. It's an approximation that holds when:
1. The material is ohmic (linear $J$–$E$ relationship)
2. Temperature is roughly constant (resistance doesn't change)
3. Frequency is low (no inductive/capacitive effects)

**Non-ohmic components** where it fails:
- **Diodes:** $I = I_0(e^{V/V_T} - 1)$ — exponential, not linear
- **Transistors:** Current is controlled by a third terminal, not just $V/R$
- **Light bulbs:** Filament resistance rises 10× from cold to hot
- **Thermistors:** Resistance changes dramatically with temperature
- **Plasmas and arcs:** $I$–$V$ characteristic is wildly nonlinear

## Derivation
**Macroscopic derivation from Drude model:**

Free electrons in a conductor experience:
1. Electric force: $F = eE$ (where $E = V/L$ for uniform field)
2. Drag from lattice collisions, characterized by mean free time $\tau$

At steady state (constant drift), force equals drag:
$$eE = \frac{m_e v_d}{\tau} \implies v_d = \frac{eE\tau}{m_e}$$

Current density: $J = nev_d = \frac{ne^2\tau}{m_e} E = \sigma E$

Integrating over the conductor geometry (length $L$, area $A$):
$$I = JA = \sigma \frac{V}{L} A$$

Solving for $V$:
$$V = \frac{L}{\sigma A} I = \underbrace{\rho \frac{L}{A}}_{R} \cdot I$$

So $V = IR$ emerges from the microscopic electron collision model. The linearity ($V \propto I$) comes from the linearity of $J = \sigma E$ — which itself comes from the linear drag approximation in the Drude model.

## Worked Example
**Problem:** Baymax's ultrasonic sensor is powered from a 5 V rail. The sensor datasheet says: supply current 15 mA at 5 V. There's a 0.5 Ω parasitic resistance in the power trace. What voltage actually reaches the sensor?

Step 1 — voltage drop across trace:
$$V_{trace} = I \cdot R_{trace} = 0.015\,\text{A} \times 0.5\,\Omega = 7.5\,\text{mV}$$

Step 2 — voltage at sensor:
$$V_{sensor} = 5.000 - 0.0075 = 4.9925\,\text{V} \approx 4.99\,\text{V}$$

Acceptable — the sensor's operating range is 4.5–5.5 V. But if the trace resistance were 10 Ω (thin, long trace or bad connector): $V_{drop} = 0.015 \times 10 = 150\,\text{mV}$, $V_{sensor} = 4.85\,\text{V}$ — still okay but worth monitoring.

**Nonlinear example:** For a diode with $V_f = 0.7\,\text{V}$ in a 5 V, 220 Ω circuit:
$$I = \frac{V - V_f}{R} = \frac{5 - 0.7}{220} = \frac{4.3}{220} \approx 19.5\,\text{mA}$$
Note: this is not pure Ohm's Law — we had to subtract the diode's forward voltage first. Ohm's Law only applies across the resistor, not the whole nonlinear branch.

## See Also
- [[Voltage]] — the V in $V = IR$; potential difference is the driving force
- [[Electric Current]] — the I in $V = IR$; what flows in response to voltage
- [[Resistance]] — the R in $V = IR$; the material and geometry property that limits current
- [[Series Circuit]] — uses Ohm's Law on total and individual resistances with shared current
- [[Parallel Circuit]] — uses Ohm's Law on each branch with shared voltage
- [[Electric Power]] — $P = IV = I^2R = V^2/R$; all three forms derived from Ohm's Law
- [[Kirchhoff's Voltage Law]] — used together with Ohm's Law to solve any resistive circuit
- [[Voltage Divider]] — direct application: two resistors, Ohm's Law twice, ratio of voltages
- [[Newton's Second Law]] — both are linear relationships between a driving quantity and a response: $F = ma$ (force → acceleration) mirrors $V = IR$ (voltage → current); linearity is the key shared structure
- [[Gradient Descent]] — linear relationships like $V = IR$ are the foundation of linear models; understanding linearity here prepares the intuition for why gradient descent converges smoothly on linear (convex) problems
