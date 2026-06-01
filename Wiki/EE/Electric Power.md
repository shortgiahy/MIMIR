# Electric Power

**One-liner:** Electric power is the rate at which a circuit element converts electrical energy into another form, equal to the product of voltage across it and current through it.

## Core Idea
$$P = IV$$
Power is measured in watts (W = J/s). For a resistor obeying Ohm's Law ($V = IR$), substitution gives two equivalent forms:
$$P = IV = I^2 R = \frac{V^2}{R}$$
All three are the same quantity — choose whichever form matches what you already know. Positive $P$ means the element *absorbs* power (converts it to heat, mechanical work, light, etc.). Negative $P$ means the element *supplies* power (a source).

## Why It Exists
Voltage and current alone don't tell you how fast energy is being transferred. A 1 A current at 1 V transfers 1 J/s; the same 1 A at 1000 V transfers 1000 J/s — radically different implications for component ratings, heat generation, and battery life. Power unifies these into a single trackable quantity. Engineers formalized $P = IV$ because every component has a maximum power rating, and exceeding it causes failure — often thermal.

## Real-World Applications
- **Component ratings:** Resistors are sold by power rating (1/8 W, 1/4 W, 1/2 W, 1 W). A 1 kΩ resistor carrying 30 mA dissipates $P = I^2 R = (0.03)^2 \times 1000 = 0.9\,\text{W}$ — a 1/4 W resistor would immediately burn out; use a 1 W or 2 W version.
- **Battery life (Baymax):** Total power draw from Baymax's battery determines runtime: $t = E_{battery}/P_{total}$. A 7.4 V, 2000 mAh LiPo stores $E = 7.4 \times 2 = 14.8\,\text{Wh}$. If total system power is 8 W, runtime ≈ 1.85 hours.
- **Motor efficiency:** A DC motor receiving 12 V at 2 A inputs 24 W. If mechanical output is 18 W, efficiency = 18/24 = 75%. The remaining 6 W goes to heat in the motor windings — $P_{heat} = I^2 R_{winding}$.
- **Voltage regulator heat dissipation:** A linear regulator dropping 7.4 V → 5 V at 500 mA dissipates $P = (7.4 - 5) \times 0.5 = 1.2\,\text{W}$ as heat — this requires a heatsink.
- **Trace width on PCBs:** PCB design tools use $P = I^2 R_{trace}$ to determine whether a copper trace will overheat. Thicker/wider traces have lower resistance and dissipate less power per ampere.

## Intuition
Power is the *rate* of energy transfer — how fast joules are being moved or converted. Voltage is the "pressure" pushing charge; current is the flow rate of charge. Their product is energy-per-charge times charge-per-second: joules per second = watts.

Think of a waterwheel: voltage is the height of the water (potential energy per kilogram), current is the mass flow rate (kg/s). Power is the product — the rate at which gravitational potential energy is converted to rotational kinetic energy. Doubling the height *or* doubling the flow rate doubles the power.

**Passive sign convention:** This is critical for getting the sign of power right. If current enters the positive terminal of an element, that element *absorbs* power ($P = +IV > 0$). If current enters the negative terminal, it *supplies* power ($P = -IV < 0$). Batteries supply power (current exits positive terminal into the external circuit). Resistors always absorb power ($P = I^2R \geq 0$ always, since $R \geq 0$).

**Which form to use:**
- Know $I$ and $R$: use $P = I^2 R$
- Know $V$ and $R$: use $P = V^2/R$
- Know $I$ and $V$: use $P = IV$
- Know only $V$ and need to choose a resistor: $P = V^2/R$ tells you — higher R means lower power.

**Power conservation:** The total power delivered by all sources equals the total power absorbed by all loads in any circuit. This is a direct consequence of [[Conservation of Energy]]. If you sum all $P = IV$ values across every element (with correct sign convention), the total is zero.

## Derivation
**From energy and the definition of voltage and current:**

Voltage is defined as energy per unit charge: $V = dW/dq$ (joules per coulomb).

Current is defined as charge flow rate: $I = dq/dt$ (coulombs per second).

Power is energy transfer rate: $P = dW/dt$.

By the chain rule:
$$P = \frac{dW}{dt} = \frac{dW}{dq} \cdot \frac{dq}{dt} = V \cdot I$$

This derivation is model-independent — it doesn't assume resistors or any particular component. $P = IV$ holds for capacitors, inductors, motors, LEDs, and all circuit elements.

**For resistors — substituting Ohm's Law ($V = IR$):**

$$P = IV = I(IR) = I^2 R$$

$$P = IV = \frac{V}{R} \cdot V = \frac{V^2}{R}$$

Both substitutions are valid only for ohmic (resistive) elements. For capacitors and inductors, $P = IV$ still holds instantaneously, but $V$ and $I$ are no longer simply related by a constant $R$.

**Power conservation proof:**

By KVL, for any loop: $\sum_k V_k = 0$. Multiplying through by loop current $I$:
$$\sum_k V_k I = 0 \implies \sum_k P_k = 0$$

Total power delivered = total power absorbed. This generalizes to any circuit via the Tellegen theorem, which combines KVL and KCL to prove $\sum P_k = 0$ for any network.

## Worked Example
**Problem:** Baymax's servo motor is driven by a PWM signal at an average effective voltage of $V = 6\,\text{V}$. The motor's winding resistance is $R_{coil} = 2\,\Omega$, and at full load it draws $I = 1.5\,\text{A}$.

**Part A — Input power:**
$$P_{in} = IV = 1.5 \times 6 = 9\,\text{W}$$

**Part B — Heat dissipated in winding resistance:**
$$P_{heat} = I^2 R_{coil} = (1.5)^2 \times 2 = 2.25 \times 2 = 4.5\,\text{W}$$

**Part C — Mechanical power output:**
$$P_{mech} = P_{in} - P_{heat} = 9 - 4.5 = 4.5\,\text{W}$$

**Efficiency:** $\eta = P_{mech}/P_{in} = 4.5/9 = 50\%$. Poor — this motor is heavily loaded. Reducing load or using a lower-resistance winding would improve efficiency.

**Part D — Resistor selection for a sense resistor.**
You want to place a current-sense resistor in series with the motor to measure current via voltage drop. You choose $R_{sense} = 0.1\,\Omega$. Power dissipated:
$$P_{sense} = I^2 R_{sense} = (1.5)^2 \times 0.1 = 0.225\,\text{W}$$

Use a 1/2 W rated resistor for a safe 2× margin.

## See Also
- [[Voltage]] — one of the two factors in $P = IV$; potential difference drives energy transfer
- [[Electric Current]] — the other factor; charge flow rate determines how fast energy moves
- [[Ohm's Law]] — allows substitution to get $P = I^2R$ and $P = V^2/R$ for resistors
- [[Joule Heating]] — the specific phenomenon of power becoming heat in a resistor; $P = I^2R$ is its formula
- [[Resistance]] — determines how power distributes between series/parallel elements
- [[Conservation of Energy]] — power conservation in circuits is a direct application of energy conservation
- [[Series Circuit]] — power adds across series elements; $P_{total} = P_1 + P_2 + \cdots$
- [[Parallel Circuit]] — power adds across parallel branches; each branch dissipates independently
- [[Work]] — $P = dW/dt$; electric power is the rate of doing work, exactly as in mechanics
- [[Kinetic Energy]] — energy conservation links electrical power to mechanical output: motor input power = KE gain + heat losses
- [[Loss Function]] — both electric power (in a sense resistor or winding) and ML loss functions are quantities engineers track and minimize during system optimization
