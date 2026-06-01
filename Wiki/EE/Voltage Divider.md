# Voltage Divider

**One-liner:** A voltage divider uses two resistors in series to produce an output voltage that is a fixed fraction of the input voltage.

## Core Idea
$$V_{out} = V_{in} \cdot \frac{R_2}{R_1 + R_2}$$
Two resistors $R_1$ (top) and $R_2$ (bottom) are connected in series between $V_{in}$ and ground. The output is taken across $R_2$. The ratio $R_2/(R_1+R_2)$ is always between 0 and 1, so $V_{out}$ is always less than or equal to $V_{in}$. The output fraction depends only on the ratio of the resistors — not their absolute values (for an unloaded divider).

## Why It Exists
Sensors, microcontrollers, and signal processing circuits operate at different voltage levels. A voltage divider is the simplest and cheapest way to:
1. Scale a voltage down to a safe level (5 V sensor → 3.3 V ADC)
2. Create a reference voltage for biasing
3. Convert a variable resistance into a voltage (with a thermistor or potentiometer)

It is arguably the single most-used subcircuit in all of analog electronics. Understanding it deeply means understanding why it fails under load — the loaded divider is where intuition gets developed.

## Real-World Applications
- **Sensor level shifting (Baymax):** IR sensors output 0–5 V. The Raspberry Pi ADC only tolerates 3.3 V. A divider with $R_1 = 1\,\text{k}\Omega$, $R_2 = 2\,\text{k}\Omega$ gives $V_{out} = V_{in} \times 2/3 = 3.33\,\text{V}$ max — just within spec.
- **Battery voltage monitoring:** Baymax's 7.4 V LiPo must be monitored by a 3.3 V ADC. A divider with ratio $3.3/7.4 \approx 0.446$ scales the full battery voltage into the ADC range.
- **Thermistor temperature sensing:** A thermistor paired with a fixed resistor in a divider converts temperature-dependent resistance to a voltage. The ADC reads the voltage; firmware looks up temperature.
- **Potentiometer (rotary sensor):** A potentiometer is a continuously variable voltage divider — the wiper taps off a fraction of the applied voltage. Used for position sensing and manual control.
- **Biasing transistors:** BJT base bias networks are voltage dividers setting the base voltage for the transistor's operating point.

## Intuition
Imagine two people pulling on a rope (current) in opposite directions (one towards $V_{in}$, one towards ground). The output tap is wherever you grab the rope. If $R_2$ is twice $R_1$, the "grab point" is 2/3 of the way from ground — $V_{out} = 2/3 \cdot V_{in}$.

**The loading problem — the critical real-world insight:** The formula $V_{out} = V_{in} \cdot R_2/(R_1+R_2)$ assumes no current is drawn from the output. The moment a load $R_L$ is connected, it appears in parallel with $R_2$, creating an effective $R_2' = R_L \| R_2 < R_2$. This pulls $V_{out}$ down:
$$V_{out,loaded} = V_{in} \cdot \frac{R_2 \| R_L}{R_1 + R_2 \| R_L}$$

**Rule of thumb:** For the loaded output to be within 1% of ideal, the load resistance must be at least 100× the parallel resistor ($R_L \geq 100 R_2$). For ADC inputs (high impedance), this is usually fine. For driving other resistors, it may not be.

## Derivation
**From Ohm's Law and KVL:**

Two resistors in series carrying the same current $I$ (series circuit):
$$I = \frac{V_{in}}{R_1 + R_2}$$

Voltage across $R_2$ (Ohm's Law):
$$V_{out} = I \cdot R_2 = \frac{V_{in}}{R_1 + R_2} \cdot R_2 = V_{in} \cdot \frac{R_2}{R_1 + R_2}$$

**Loaded divider derivation:**

With load $R_L$ across $R_2$, the parallel combination is:
$$R_2' = \frac{R_2 R_L}{R_2 + R_L}$$

Substituting into the divider formula:
$$V_{out,loaded} = V_{in} \cdot \frac{R_2'}{R_1 + R_2'} = V_{in} \cdot \frac{R_2 R_L}{R_1(R_2 + R_L) + R_2 R_L}$$

When $R_L \gg R_2$: $R_2' \approx R_2$, and the loaded formula collapses to the unloaded formula.

## Worked Example
**Problem:** Baymax uses a thermistor to measure ambient temperature for thermal management. The thermistor has $R_{25°C} = 10\,\text{k}\Omega$ and $R_{85°C} = 1.2\,\text{k}\Omega$ (NTC type — decreases with temperature). A 3.3 V supply and a fixed resistor $R_1 = 10\,\text{k}\Omega$ are used. The ADC reads $V_{out}$.

Setup: $V_{in} = 3.3\,\text{V}$, $R_1 = 10\,\text{k}\Omega$ (top), thermistor $R_{th}$ (bottom = $R_2$).

At 25°C: $R_{th} = 10\,\text{k}\Omega$
$$V_{out} = 3.3 \times \frac{10\,000}{10\,000 + 10\,000} = 3.3 \times 0.5 = 1.65\,\text{V}$$

At 85°C: $R_{th} = 1.2\,\text{k}\Omega$
$$V_{out} = 3.3 \times \frac{1200}{10\,000 + 1200} = 3.3 \times \frac{1200}{11200} = 3.3 \times 0.107 = 0.354\,\text{V}$$

ADC range: 0.354 V (hot) to 1.65 V (cold). The firmware maps ADC reading → voltage → temperature using a lookup table of the thermistor's resistance-temperature curve.

**Loading check:** The Raspberry Pi ADC input impedance is approximately 10 kΩ. At 25°C, $R_L = 10\,\text{k}\Omega$, $R_2 = 10\,\text{k}\Omega$:
$$R_2' = \frac{10\,000 \times 10\,000}{20\,000} = 5\,\text{k}\Omega$$
$$V_{out,loaded} = 3.3 \times \frac{5000}{15000} = 1.10\,\text{V}$$
Significantly less than 1.65 V! This is the loading error — it matters. Fix: add a voltage follower (op-amp buffer) between the divider and ADC, or reduce divider resistances so $R_L \gg R_2$.

## See Also
- [[Series Circuit]] — the voltage divider is fundamentally two resistors in series
- [[Ohm's Law]] — used to derive the divider formula from the common series current
- [[Kirchhoff's Voltage Law]] — confirms the voltage drop distribution
- [[Voltage]] — the output is always a fraction of the input voltage; understanding voltage is essential
- [[Resistance]] — the ratio of resistances determines the division ratio
- [[Current Divider]] — the dual: same topology but analyzing current split in parallel
- [[Parallel Circuit]] — loading analysis requires understanding parallel resistance
- [[Analog to Digital Conversion]] — voltage dividers feed ADC inputs throughout sensor systems
