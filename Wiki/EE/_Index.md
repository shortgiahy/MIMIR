# EE — Index

Navigation hub for all Electrical Engineering atomic notes. One concept per file. Start here; follow links into depth.

---

## Concept Map by Cluster

### Fundamentals
The bedrock: what charge, current, voltage, and resistance actually are.

| Note | What it is |
|------|-----------|
| [[Electric Charge]] | The intrinsic property of matter (electrons) that produces electromagnetic force; measured in coulombs |
| [[Electric Current]] | The rate of charge flow past a point; $I = dq/dt$; measured in amperes |
| [[Voltage]] | Electric potential difference between two points; the energy per unit charge that drives current; measured in volts |
| [[Resistance]] | A material's opposition to current flow; $R = \rho L/A$; measured in ohms |

### Laws
The governing equations that constrain every circuit, derived from conservation of charge and energy.

| Note | What it is |
|------|-----------|
| [[Ohm's Law]] | For ohmic materials, voltage, current, and resistance are linearly related: $V = IR$ |
| [[Kirchhoff's Current Law]] | At any node, currents in equal currents out — conservation of charge at a junction |
| [[Kirchhoff's Voltage Law]] | Around any closed loop, voltages sum to zero — conservation of energy around a path |

### Circuit Configurations
How components are arranged and the rules that follow from that arrangement.

| Note | What it is |
|------|-----------|
| [[Series Circuit]] | Components in a single chain: same current everywhere, voltages add |
| [[Parallel Circuit]] | Components between the same two nodes: same voltage everywhere, currents add |
| [[Voltage Divider]] | Two series resistors that produce a fixed fraction of the input voltage; $V_{out} = V_{in} \cdot R_2/(R_1+R_2)$ |
| [[Current Divider]] | Two parallel resistors that split current inversely proportional to resistance; $I_x = I_{total} \cdot R_{other}/(R_1+R_2)$ |

### Analysis
Systematic methods for solving circuits of arbitrary complexity.

| Note | What it is |
|------|-----------|
| [[Node Voltage Method]] | Assign node voltages, write KCL at each node, solve the resulting linear system |

### Power
How energy moves through and is dissipated by circuits.

| Note | What it is |
|------|-----------|
| [[Electric Power]] | Rate of energy transfer: $P = IV = I^2R = V^2/R$; measured in watts |
| [[Joule Heating]] | Irreversible conversion of electrical energy to heat in a resistor: $P = I^2R$, $\Delta T = \theta P$ |

---

## Cross-Subject Reference Table

Understanding EE requires grounding in Physics, Math, and connects forward into ML/Robotics. Here is where the subjects meet.

| EE Concept | Physics Foundation | Math Tool | ML/Robotics Connection |
|---|---|---|---|
| [[Electric Charge]] | Coulomb's Law, electrostatics | — | Charge storage in motor driver capacitors |
| [[Electric Current]] | Charge flow, drift velocity | Differentiation ($I = dq/dt$) | Current sensors for robot motor control |
| [[Voltage]] | [[Conservation of Energy]], potential fields | — | ADC input ranges, PWM duty cycle |
| [[Resistance]] | Drude model, material properties | Geometry ($R = \rho L/A$) | Motor winding R, PCB trace resistance |
| [[Ohm's Law]] | Drude model derivation | Linear algebra (matrix $\mathbf{G}\mathbf{V} = \mathbf{I}$) | Ohm's Law in every sensor bias calculation |
| [[Kirchhoff's Current Law]] | [[Conservation of Energy]] (charge) | System of linear equations | Nodal analysis in SPICE; current sensing |
| [[Kirchhoff's Voltage Law]] | [[Conservation of Energy]] (energy) | System of linear equations | Loop equations in motor control models |
| [[Series Circuit]] | Energy partition | Summation, ratios | Voltage drop on power trace chains |
| [[Parallel Circuit]] | Flow division, conductance | Reciprocal summation | Parallel load current budget for Baymax |
| [[Voltage Divider]] | Potential division | Ratios | Sensor level-shifting (5V → 3.3V ADC) |
| [[Current Divider]] | Parallel flow split | Ratios | Shunt-based current sensing |
| [[Node Voltage Method]] | Conservation laws applied globally | [[Matrix]] inversion, linear systems | Foundation of all SPICE simulation |
| [[Electric Power]] | [[Work-Energy Theorem]], power = $dW/dt$ | Calculus (instantaneous rate) | Battery sizing, regulator selection |
| [[Joule Heating]] | Thermodynamics, heat transfer | Integration ($E = \int I^2 R\, dt$) | Motor driver thermal design, wire sizing |

---

## Baymax Power System Reading Path

Follow this sequence to build the knowledge needed to design and validate Baymax's power system from battery to motor.

**1. Ground the concepts**
- [[Electric Charge]] → [[Electric Current]] → [[Voltage]] → [[Resistance]]

**2. Learn the laws**
- [[Ohm's Law]] → [[Kirchhoff's Current Law]] → [[Kirchhoff's Voltage Law]]

**3. Understand circuit topologies**
- [[Series Circuit]] → [[Parallel Circuit]]
- [[Voltage Divider]] (sensor level-shifting) → [[Current Divider]] (shunt sensing)

**4. Learn to analyze complex circuits**
- [[Node Voltage Method]] — finds voltages at every supply rail under load

**5. Master power and thermal design**
- [[Electric Power]] — budget total power draw; size battery and regulators
- [[Joule Heating]] — set wire gauges, MOSFET heatsinks, motor stall protection

**Key design questions this path answers:**
- How long does Baymax's LiPo last given all loads? → [[Electric Power]]
- Will the motor driver MOSFET overheat? → [[Joule Heating]]
- What current does each sensor draw at its bias node? → [[Node Voltage Method]]
- How do I level-shift a 5V sensor output to the 3.3V ADC? → [[Voltage Divider]]
- How do I measure motor current with a shunt resistor? → [[Current Divider]], [[Joule Heating]]
- What wire gauge should I use for the motor leads? → [[Joule Heating]], [[Electric Current]]

---

## Notes on Scope

These notes cover DC resistive circuits — the foundation layer. Future clusters to add:

- **AC Circuits:** Capacitors, Inductors, Impedance, Phasors, Resonance
- **Semiconductor Devices:** Diodes, BJTs, MOSFETs, Op-Amps
- **Digital Logic:** Logic Gates, Flip-Flops, State Machines
- **Power Electronics:** Buck/Boost Converters, H-Bridge, PWM
- **Signals:** Fourier Transform, Filters, Transfer Functions
- **Sensors and Transducers:** ADC, DAC, I2C, SPI, UART

Each future cluster will link back to this index.
