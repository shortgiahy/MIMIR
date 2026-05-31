# Ohm's Law

**One-liner:** For a linear resistor, voltage across it equals current through it times its resistance — the single most-used relationship in circuit analysis.

## Why It Exists

By the 1820s, physicists knew that batteries produced voltage and that wires carried current. But no one had systematically quantified *how* voltage and current were related for different materials. Georg Simon Ohm spent years threading fine wires of different lengths and materials between the poles of electrochemical cells, measuring deflections in a compass needle (the best current-measuring instrument of the day) as a proxy for current. In 1827 he published his finding: for metallic conductors, current is directly proportional to the applied voltage. Double the voltage, double the current. Halve the voltage, halve the current.

The proportionality constant is resistance. That is all Ohm's Law is — a clean, experimentally discovered proportionality that holds for a wide class of materials at practical temperatures and field strengths.

The deeper physics behind it (why does proportionality hold?) came later with the Drude model of electron conduction (1900) and then properly with quantum mechanics. But the empirical relationship is simple, powerful, and the foundation of classical circuit design.

## The Concept

### The Statement

$$V = IR$$

Voltage (V) equals current (I) times resistance (R). Equivalently:

$$I = \frac{V}{R} \qquad R = \frac{V}{I}$$

Three forms, one relationship. Which form you use depends on what is unknown.

### Physical Derivation (Drude Model)

Why does this proportionality hold at a microscopic level? The Drude model gives an approximate but illuminating answer.

Free electrons in a metal move randomly at thermal velocities (~$10^5$ m/s). When an electric field $\mathbf{E}$ is applied across the conductor, each electron feels a force $F = eE$ and accelerates. But it doesn't accelerate indefinitely — it constantly collides with lattice atoms. Between collisions, it accelerates; at each collision, it roughly randomizes its velocity.

The average time between collisions is the **mean free time** $\tau$ (typically $\sim 10^{-14}$ s in copper). Between collisions, an electron accelerates from near-zero velocity to a drift velocity:

$$v_d = \frac{eE\tau}{m_e}$$

where $e$ is electron charge, $m_e$ is electron mass. Current density is:

$$J = nev_d = \frac{ne^2\tau}{m_e} E$$

where $n$ is the number density of free electrons. The proportionality constant between $J$ and $E$ is the **conductivity** $\sigma$:

$$J = \sigma E \qquad \text{(microscopic Ohm's Law)}$$

Translating to macroscopic circuit quantities — $E = V/L$ (uniform field in a wire of length $L$), $J = I/A$ (current through cross-section $A$) — and using $R = L/(\sigma A)$:

$$I/A = \sigma (V/L) \implies V = I \cdot \frac{L}{\sigma A} = I \cdot R$$

Ohm's Law falls out of Newton's second law applied to electrons experiencing viscous-like drag from collisions. The law holds when this linear drag model holds — which is why it *fails* when field strengths are very high (electrons gain so much energy between collisions that the linear model breaks down) or when quantum effects matter.

### What "Linear" Means

A **linear** (or ohmic) component has constant resistance regardless of the voltage or current through it. Plot $V$ vs. $I$: you get a straight line through the origin. The slope is $R$.

A **nonlinear** component does not obey this. The diode is the canonical example: it allows current to flow easily in one direction, barely at all in the other. Its $V$-$I$ curve is an exponential, not a line. You can still define *instantaneous* resistance $R = V/I$ at any operating point, but that resistance changes as you move around the curve.

```
Ohmic resistor:          Diode:
I |  /                  I |        __/
  | /                     |       /
  |/____________ V         |------/_______ V
  (straight line)          (exponential, asymmetric)
```

For nonlinear components, Ohm's Law doesn't hold in general. You need the full device equations (e.g., the Shockley diode equation for a p-n junction).

### Limitations of Ohm's Law

**1. Frequency / inductance effects.** At high frequencies, inductance and capacitance in wires and components matter. A long wire that behaves like a 1Ω resistor at DC becomes a reactive impedance at GHz frequencies. Ohm's Law as $V = IR$ is strictly a DC or low-frequency approximation. The generalization for AC circuits replaces $R$ with complex impedance $Z$: $V = IZ$.

**2. Temperature dependence.** $R$ is not constant with temperature for most materials. For metals, $R$ increases with temperature. This is usually small and can be ignored for quick calculations, but matters in precision designs and in components that dissipate significant power (they heat up, changing their own resistance).

**3. Nonlinear components.** Diodes, transistors, Zener diodes, varistors, thermistors, tunnel diodes, Josephson junctions — many critically important components are not ohmic. Ohm's Law does not apply to them in any straightforward sense.

**4. Quantum regime.** At the nanoscale, resistance becomes quantized. Conductance through a quantum wire comes in integer multiples of $G_0 = 2e^2/h \approx 77.5\ \mu\text{S}$ (the quantum of conductance). Classical Ohm's Law breaks down entirely.

**5. High electric fields.** Very large fields cause breakdown — electrons gain enough energy to ionize atoms (avalanche breakdown in semiconductors, dielectric breakdown in insulators). Ohm's Law doesn't apply near breakdown.

### The Three Unknowns, Three Uses

Any time you have a resistor in a circuit, Ohm's Law gives you one equation relating the three quantities $V$, $I$, and $R$. Know any two, and you can find the third:

- **Find current:** $I = V/R$ — given a known voltage source and known resistor
- **Find voltage:** $V = IR$ — given a known current source and known resistor  
- **Find resistance:** $R = V/I$ — measure voltage and current to characterize an unknown component

This last use is how you measure an unknown resistance experimentally — apply a known voltage, measure the resulting current, divide.

### Ohm's Law in Robotics

Motor drive circuits routinely use Ohm's Law for:
- Computing current-limiting resistors for LEDs, sensors, signal lines
- Understanding voltage drops across wire resistance in power distribution
- Designing voltage dividers for sensor signal conditioning
- Calculating shunt resistor values for current sensing (measuring motor current)

A brushed DC motor has winding resistance $R_{winding}$. At stall (zero speed), the back-EMF is zero and the motor draws $I_{stall} = V/R_{winding}$ — the maximum current, often dangerously high. This is why motor controllers include current limiting.

## Intuition

The water analogy restated precisely: voltage is pressure, current is flow rate, resistance is how narrow the pipe is. Double the pressure through the same pipe, double the flow. Same pressure, narrower pipe, less flow.

A more electrical intuition: think of resistance as the *willingness of a material to let charge flow*. High resistance means the material fights the electric field — electrons collide often and lose energy as heat. Low resistance means electrons skate through with minimal loss. The ratio of how hard you push (voltage) to how much actually flows (current) is resistance.

The key insight for avoiding the classic confusion: **Ohm's Law tells you the current that *results from* a voltage applied across a fixed resistance.** It is not saying that if you know the voltage you automatically know the current — you need the resistance too. A 9V battery across a 9Ω resistor gives 1A. Across a 1Ω resistor it gives 9A. The battery voltage doesn't set the current — the voltage *and* the load resistance together determine current.

## Key Formula / Rule

$$V = IR$$

where:
- $V$ = voltage across the resistor, in volts (V)
- $I$ = current through the resistor, in amperes (A)
- $R$ = resistance, in ohms (Ω)

Valid only for **linear (ohmic) components** at constant temperature, at DC or low frequencies.

The generalized AC form: $\mathbf{V} = \mathbf{I} \cdot Z$ where $Z$ is complex impedance in ohms.

## Worked Example

**Problem:** A microcontroller GPIO pin outputs 3.3V. It is connected to an LED with a forward voltage of 2.0V and a maximum continuous current rating of 20mA. What resistor do you need in series to protect the LED?

**Step 1: Identify the voltage across the resistor.**

The total voltage in the loop must add up to the supply voltage. The LED "uses" 2.0V (its forward voltage drop). The rest must drop across the series resistor:

$$V_R = V_{supply} - V_{LED} = 3.3\text{ V} - 2.0\text{ V} = 1.3\text{ V}$$

**Step 2: Set the desired current.**

You want to run the LED at a safe but bright level — say 15mA (leaving headroom below the 20mA maximum). So $I = 0.015\text{ A}$.

**Step 3: Apply Ohm's Law to find R.**

$$R = \frac{V_R}{I} = \frac{1.3\text{ V}}{0.015\text{ A}} = 86.7\ \Omega$$

**Step 4: Choose a standard resistor value.**

Standard resistor values follow the E24 series. The nearest values are 82Ω and 91Ω. Choose 91Ω (the higher value) to keep current slightly below target rather than above — this protects the LED.

**Step 5: Verify.**

$$I_{actual} = \frac{V_R}{R} = \frac{1.3\text{ V}}{91\ \Omega} \approx 14.3\text{ mA}$$

14.3mA — below the 20mA maximum. LED is safe and will be visible. ✓

**Physical check:** This is exactly the calculation done on every Arduino tutorial in existence. The 220Ω and 330Ω resistors you often see recommended are conservative choices that run LEDs dimmer but more safely from 5V supplies.

## Gotchas

**Ohm's Law applies to resistors, not to all components.** Beginners sometimes try to apply $V = IR$ to an LED or a transistor. The equation $R = V/I$ always gives you a number, but that number changes at every operating point for nonlinear devices. You must use the component's actual device equations or datasheet curves.

**The resistor limits current; it doesn't create voltage.** Current flows *through* the resistor. Voltage appears *across* it as a consequence. The direction of the current determines the polarity of the voltage drop — conventional current flows from the + end to the − end of the resistor (in the direction of decreasing potential).

**Unit consistency.** Work in SI units: volts, amperes, ohms. The most common error is mixing milliamps and amps without converting. 20mA = 0.020A. If you plug in 20 thinking it is amps, your answer is off by 1000×.

**Ohm's Law doesn't hold at extremes.** At very high currents, resistors get hot and their resistance changes. At breakdown voltages, resistors fail. Real resistors have both a resistance rating and a power rating — exceed the power rating and you destroy the component (see [[Power in Circuits]]).

**Direction convention.** Ohm's Law assumes conventional current flows from + to − through the external circuit. If you define current in the opposite direction, you get a sign flip in voltage: $V = -IR$. Be consistent with your sign convention throughout a calculation.

## See Also
- [[Voltage, Current, and Resistance]] — the physical meaning of the three quantities in V = IR
- [[Kirchhoff's Laws]] — the system-level laws that combine with Ohm's Law to solve any resistive circuit
- [[Series and Parallel Circuits]] — applying Ohm's Law to combinations of resistors
- [[Power in Circuits]] — P = IV = I²R = V²/R, the energy consequences of Ohm's Law
