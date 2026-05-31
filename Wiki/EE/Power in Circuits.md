# Power in Circuits

**One-liner:** Electrical power is the rate at which energy is transferred in a circuit — P = IV — and it determines whether components survive, how hot they get, and how long a battery lasts.

## Why It Exists

Voltage and current tell you about the *state* of a circuit at an instant. But engineers need to know about *energy* — how fast is the battery draining? How hot will this resistor get? Will this wire melt? Can this motor do enough mechanical work in a given time?

Power is the bridge from the static electrical picture (voltages and currents) to the dynamic energy picture (joules per second, heat, mechanical work, electromagnetic radiation). Without it, you cannot design circuits that survive in the real world.

Power as a concept predates electrical engineering — James Watt quantified mechanical power (horsepower) in the 1780s. The application to electrical circuits came with the development of motors and generators in the mid-1800s, when engineers urgently needed to know how to build efficient machines. Joule's law (1841), which relates power dissipated in a resistor to current and resistance, was motivated directly by practical questions about telegraph wire sizing.

## The Concept

### Fundamental Definition

Power is the rate of energy transfer, or equivalently, the rate of doing work:

$$P = \frac{dW}{dt}$$

where $W$ is energy in joules (J) and $P$ is power in watts (W). 1 watt = 1 joule per second.

### Derivation of P = IV

Recall:
- Voltage is energy per unit charge: $V = dW/dq$
- Current is charge per unit time: $I = dq/dt$

Power = energy per unit time:

$$P = \frac{dW}{dt} = \frac{dW}{dq} \cdot \frac{dq}{dt} = V \cdot I$$

$$\boxed{P = IV}$$

This derivation is clean and exact — no approximations. It follows directly from the definitions of voltage and current. It applies to every electrical component: resistors, capacitors, inductors, batteries, motors, LEDs, anything.

### Forms for Resistors

For a purely resistive component obeying $V = IR$, the power dissipated can be written in three equivalent forms:

$$P = IV = I^2 R = \frac{V^2}{R}$$

Derivation of the second: $P = IV = I(IR) = I^2 R$.
Derivation of the third: $P = IV = (V/R)V = V^2/R$.

All three are equivalent — use whichever is convenient given what you know:
- Know $I$ and $R$ → use $P = I^2 R$
- Know $V$ and $R$ → use $P = V^2/R$
- Know $V$ and $I$ → use $P = IV$

### Where Does the Energy Go?

In a resistor, electrical energy is converted to **thermal energy** (heat). The process: the electric field does work on electrons, accelerating them. Electrons collide with lattice atoms, transferring kinetic energy to atomic vibration. Atomic vibration is heat. The resistor's temperature rises until the power dissipated as heat equals the power delivered to it electrically.

This is **Joule heating** (or resistive heating, or ohmic heating). It is irreversible — you cannot recover the heat and turn it back into electrical energy without violating thermodynamics.

In other components:
- **Battery (discharging):** Chemical potential energy → electrical energy (source, not load — it is supplying power, $P < 0$ by convention)
- **Motor:** Electrical energy → mechanical energy (work done against load) + heat (winding resistance losses)
- **LED:** Electrical energy → photons (light) + heat (junction resistance)
- **Capacitor:** Electrical energy stored in electric field (ideally no dissipation)
- **Inductor:** Electrical energy stored in magnetic field (ideally no dissipation)

Real capacitors and inductors have parasitic resistance, so they do dissipate some power — an important consideration at high frequencies.

### Sign Convention: Absorbing vs. Delivering Power

Power has a sign. By convention using **passive sign convention**:
- If conventional current enters the + terminal of an element, the element is **absorbing** power: $P > 0$ (it is a load)
- If conventional current enters the − terminal, the element is **delivering** power: $P < 0$ (it is a source)

In a circuit, **total power must balance**: $\sum P_{absorbed} = \sum P_{delivered}$. This is conservation of energy applied to circuits. The power delivered by all sources equals the power absorbed by all loads. If your calculation doesn't balance, you made an error.

### Power Ratings and Component Limits

Every real component has a **maximum power rating** — the maximum rate at which it can absorb energy without failing. Exceed it and the component overheats, degrades, or fails catastrophically.

Resistors come in standard power ratings: 1/8W, 1/4W, 1/2W, 1W, 2W, 5W, 10W, etc. The physical size is roughly proportional to the power rating — larger resistors have more surface area to radiate heat.

When choosing a resistor, you must:
1. Calculate the maximum power it will dissipate: $P = I^2R$ or $P = V^2/R$
2. Choose a resistor rated for at least 2× that power (a standard derating factor for reliability)
3. Verify the ambient temperature and thermal resistance of the package

Failing to check power ratings is one of the most common reasons hobby circuits smoke and die.

### Power in Series and Parallel

**Series circuit:** Same current $I$ through all resistors. Power in each:

$$P_k = I^2 R_k$$

Total power = $P_{total} = I^2 \sum R_k = I^2 R_{eq}$

Larger resistors dissipate more power in series (current is fixed, power scales with $R$).

**Parallel circuit:** Same voltage $V$ across all resistors. Power in each:

$$P_k = \frac{V^2}{R_k}$$

Total power = $P_{total} = V^2 \sum \frac{1}{R_k} = \frac{V^2}{R_{eq}}$

Smaller resistors dissipate more power in parallel (voltage is fixed, power scales with $1/R$). This is counterintuitive at first — the path of least resistance carries the most current and therefore dissipates the most power.

### Energy, Not Just Power

Power tells you the *rate* of energy transfer. Multiply by time to get total energy:

$$W = P \cdot t = I V t \quad \text{(joules, for constant power)}$$

For time-varying power, integrate:

$$W = \int_0^T P(t)\, dt$$

Utility companies bill in **kilowatt-hours (kWh)**: $1\ \text{kWh} = 3.6 \times 10^6\ \text{J}$. This is an energy unit, not a power unit — the number of kilowatts times the number of hours.

### Battery Life and Efficiency

A battery stores a finite amount of charge, rated in **ampere-hours (Ah)** or **milliampere-hours (mAh)**. A 2000mAh battery can supply 2000mA for 1 hour, or 200mA for 10 hours, or 20mA for 100 hours (approximately — real batteries are more complex).

Battery energy content:

$$W_{battery} = V \cdot Q = V \cdot I \cdot t$$

For a 3.7V, 2000mAh LiPo battery:

$$W = 3.7\text{ V} \times 2\text{ Ah} = 7.4\ \text{Wh} = 26,640\ \text{J}$$

Runtime given a known load power $P_{load}$:

$$t = \frac{W_{battery}}{P_{load}}$$

For the Baymax robot running on a 7.4Wh battery at 5W total load: $t = 7.4/5 = 1.48$ hours. Every milliwatt counts in battery-powered systems.

**Efficiency** is the ratio of useful power out to total power in:

$$\eta = \frac{P_{out}}{P_{in}} \times 100\%$$

A motor with 80% efficiency that is delivering 40W of mechanical output is drawing $40/0.80 = 50W$ electrical input. The remaining 10W becomes heat in the windings and friction losses.

### Thermal Design

When power is dissipated in a component, its temperature rises. How much it rises depends on the **thermal resistance** $\theta$ (°C/W):

$$\Delta T = P \cdot \theta$$

A transistor with thermal resistance 10°C/W dissipating 2W will be 20°C above ambient temperature. If ambient is 25°C, the junction reaches 45°C — well within limits. At 5W, it reaches 75°C — still okay. At 15W, it would reach 175°C, which for a silicon transistor (max junction temperature ~150°C) means failure.

This is why power transistors need heatsinks (which lower effective thermal resistance), and why motor controllers can get hot under load.

## Intuition

Power is the *weight on the circuit* — how hard the circuit is working at any moment. Voltage is how hard you push; current is how much flows; power is what actually gets done (or burned up).

Think of a water wheel: voltage is the height the water falls, current is the flow rate of water, and power is how fast the wheel turns (work per unit time). Double the height (voltage) and double the flow (current) and you get four times the power — $P \propto IV$.

The $I^2R$ form has an intuitive reading: for resistive heating, power scales with the *square* of current. This is why transmission lines carry power at very high voltage and very low current — high voltage means low current for the same power ($P = IV$), and low current means $I^2 R$ losses in the wires are tiny. Step-up transformers exist specifically because of this $I^2 R$ insight. A wire carrying 1A at 1000V and losing $I^2 R = 1\ \Omega \cdot 1^2 = 1\text{ W}$ is far more efficient than carrying 100A at 10V (same 1kW of power) and losing $I^2 R = 1\ \Omega \cdot 10,000 = 10,000\text{ W}$.

## Key Formula / Rule

$$P = IV \quad \text{(universal — applies to any element)}$$

For resistors specifically:

$$P = I^2 R = \frac{V^2}{R}$$

Energy over time $t$:

$$W = Pt \quad \text{(joules, if P is constant)}$$

Conservation of power (must hold in any circuit):

$$\sum P_{sources} = \sum P_{loads}$$

## Worked Example

**Problem:** A robot uses a 7.4V LiPo battery (nominally 2000mAh). It has the following loads:
- Raspberry Pi compute board: 5V, 2.5A (via switching regulator with 85% efficiency)
- Two servo motors: each 6V, 1A stall (via separate regulator, 90% efficient)
- Sensor array: 3.3V, 200mA (via LDO regulator, 80% efficient)

Find: (a) total power drawn from the battery, (b) estimated runtime at these loads.

**Step 1: Power consumed by each load (at the load, not the battery).**

- Raspberry Pi: $P_{RPi} = 5\text{ V} \times 2.5\text{ A} = 12.5\text{ W}$
- Each servo at stall: $P_{servo} = 6\text{ V} \times 1\text{ A} = 6\text{ W}$; two servos = 12 W
- Sensor array: $P_{sensors} = 3.3\text{ V} \times 0.2\text{ A} = 0.66\text{ W}$

**Step 2: Power drawn from the battery (accounting for regulator inefficiency).**

Each regulator draws more power from the battery than it delivers to the load, because $\eta < 1$: $P_{battery} = P_{load}/\eta$.

- RPi regulator: $P_{bat,RPi} = 12.5 / 0.85 = 14.7\text{ W}$
- Servo regulators: $P_{bat,servos} = 12 / 0.90 = 13.3\text{ W}$
- Sensor LDO: $P_{bat,sensors} = 0.66 / 0.80 = 0.83\text{ W}$

**Step 3: Total battery power draw.**

$$P_{total} = 14.7 + 13.3 + 0.83 = 28.8\text{ W}$$

**Step 4: Battery energy content.**

$$W_{battery} = V \times Q = 7.4\text{ V} \times 2.0\text{ Ah} = 14.8\text{ Wh}$$

**Step 5: Estimated runtime.**

$$t = \frac{W_{battery}}{P_{total}} = \frac{14.8\text{ Wh}}{28.8\text{ W}} \approx 0.514\text{ hours} \approx 31\text{ minutes}$$

**Step 6: Sanity check and design implication.**

31 minutes of runtime with motors at stall — real operation would be more favorable since servos rarely run at full stall continuously. But this quantifies the fundamental constraint: power draw matters enormously for mobile robot design. Switching to more efficient regulators, using sleep modes on the compute board, and sizing motors closer to actual operating load could significantly extend runtime.

This kind of analysis is done in the early design phase of any battery-powered robot.

## Gotchas

**Forgetting regulator efficiency.** A 5V, 2A load doesn't draw $P = 5 \times 2 = 10\text{ W}$ from a 12V supply — it draws approximately $10\text{ W} / \eta_{regulator}$ from the supply, where $\eta < 1$. Linear regulators (LDOs) are particularly inefficient when there is a large voltage drop: a 12V to 5V LDO at 2A dissipates $(12-5) \times 2 = 14\text{ W}$ as heat in the regulator itself — more heat than the load consumes.

**$P = I^2 R$ vs. $P = V^2/R$ — know which to use.** For a series circuit (fixed current), use $I^2 R$ to find which resistor dissipates more power — larger $R$ loses more. For a parallel circuit (fixed voltage), use $V^2/R$ — smaller $R$ loses more. Using the wrong form gives correct math but wrong physical insight.

**Power ratings assume adequate heat removal.** A resistor rated for 0.25W is rated for 0.25W at a specific ambient temperature (often 25°C or 70°C) with adequate airflow. In a sealed enclosure, at elevated temperature, or with poor heat sinking, the actual safe power is lower. Derate generously.

**RMS power in AC circuits.** For AC circuits, $P = IV$ only gives instantaneous power. The *average* power (what determines heat and what your power meter reads) uses RMS voltage and current. For a sinusoidal source: $P_{avg} = V_{rms} I_{rms} \cos\phi$, where $\cos\phi$ is the power factor. Ignoring the power factor is a major source of errors in AC system design. For purely resistive loads, $\cos\phi = 1$ and the issue doesn't arise, but for motors and other reactive loads it matters critically.

**Energy $\neq$ power.** "This battery has 20Wh" is an energy statement. "This motor draws 50W" is a power statement. You cannot compare them directly — you need to introduce time. Mixing up energy and power units (watts vs. watt-hours, joules vs. joules per second) causes calculation errors and design failures.

## See Also
- [[Voltage, Current, and Resistance]] — P = IV uses both V and I, which are defined here
- [[Ohm's Law]] — enables the P = I²R and P = V²/R forms
- [[Series and Parallel Circuits]] — power distribution changes depending on how components are connected
- [[Kirchhoff's Laws]] — conservation of power (ΣP = 0) is a consequence of KVL and is a useful check
