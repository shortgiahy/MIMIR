# EE Wiki — Index

This is the navigation hub for the Electrical Engineering knowledge base. Built for deep understanding, not just exam prep — every entry is designed to connect concepts to physical reality, mathematical structure, and robotics applications.

**Target:** SLCC → MIT/Stanford transfer in robotics. These notes go beyond what is needed to pass courses — they go to the level needed to do original work.

---

## How to Use This Wiki

Each entry follows a standard template: motivation → concept → intuition → formula → worked example → gotchas → cross-links. Read the *Why It Exists* section first — understanding motivation prevents the cargo-cult learning that plagues engineering students.

Cross-links use `[[Concept Name]]`. EE does not exist in isolation — follow links into [[Physics]], [[Math]], and [[ML & Robotics]] aggressively.

---

## Cluster 1: Fundamentals

The irreducible foundation. Every EE concept traces back to these.

| Entry | What It Is |
|-------|-----------|
| [[Voltage, Current, and Resistance]] | The three fundamental quantities of circuit analysis — what they physically are, and why students constantly confuse voltage and current |
| [[Ohm's Law]] | V = IR: the proportional relationship between voltage and current for linear resistors, why it holds microscopically, and where it breaks down |
| [[Kirchhoff's Laws]] | KCL (conservation of charge at nodes) and KVL (conservation of energy around loops) — the two laws that make any circuit solvable |
| [[Series and Parallel Circuits]] | How and why resistors combine in each configuration, derived from Kirchhoff's laws; voltage dividers and current dividers |
| [[Power in Circuits]] | P = IV: where energy goes in a circuit, thermal design, battery life, and why this determines whether real circuits survive |

---

## Cluster 2: Circuit Analysis Techniques

Systematic methods for solving circuits of arbitrary complexity.

*Coming soon:*
- Nodal Analysis — the systematic node voltage method using KCL
- Mesh Analysis — the systematic mesh current method using KVL
- Superposition — solving circuits with multiple sources by turning them on one at a time
- Thevenin and Norton Equivalents — reducing any linear circuit to a simple two-element model
- Wheatstone Bridge — the precision resistance measurement configuration used in strain gauges and force sensors

---

## Cluster 3: Essential Components

Beyond the resistor — the components that make circuits do interesting things.

*Coming soon:*
- Capacitors — energy storage in electric fields; frequency-dependent behavior; coupling and filtering
- Inductors — energy storage in magnetic fields; back-EMF; why motors and relays need flyback diodes
- Diodes — one-way current valves; the Shockley equation; rectification; Zener regulation
- Transistors (BJT) — amplification and switching; the workhorse of analog and digital electronics
- MOSFETs — voltage-controlled switches; gate drive; the transistor of choice for motor control
- Op-Amps — ideal voltage amplifiers; inverting/non-inverting configurations; integrators; comparators

---

## Cluster 4: AC Circuits and Frequency Domain

Most signals in the real world are not DC. Sensors output AC, motors produce AC back-EMF, and every signal has a frequency spectrum.

*Coming soon:*
- Sinusoids and Phasors — representing AC quantities as rotating vectors in the complex plane
- Impedance — the generalization of resistance to frequency-dependent components (Z = V/I for AC)
- RC Circuits — low-pass and high-pass filters; time constants; the exponential charge/discharge curve
- RL Circuits — inductive filters; motor winding behavior; the $L(dI/dt)$ voltage
- RLC Circuits and Resonance — bandpass and notch filters; quality factor; resonant frequency
- Bode Plots — visualizing gain and phase vs. frequency on logarithmic axes
- Fourier Analysis — decomposing any periodic signal into sinusoids

---

## Cluster 5: Power Electronics

The interface between electricity and mechanical work — essential for robotics.

*Coming soon:*
- DC-DC Converters — buck (step-down), boost (step-up), and buck-boost; switching regulators vs. linear
- H-Bridge Motor Drivers — bidirectional DC motor control; PWM speed control; shoot-through protection
- Brushed DC Motors — electrical model; torque-speed curve; stall current; back-EMF
- Brushless DC Motors (BLDC) — how commutation works; ESCs; field-oriented control overview
- Battery Technology — LiPo, Li-ion, NiMH comparison; C-rating; BMS; safe charging practices
- Power Factor and Efficiency — reactive power in AC systems; why efficiency matters for battery life

---

## Cluster 6: Robotics Applications

Where EE meets the Baymax project. These entries connect circuit theory to specific robot subsystems.

*Coming soon:*
- Sensor Interfaces — voltage dividers for resistive sensors; ADC input conditioning; I2C and SPI protocols
- PWM — pulse-width modulation for motor speed control and servo position control
- Current Sensing — shunt resistors and hall-effect sensors for motor current feedback
- Motor Control Loops — the role of EE in PID control; current loop, velocity loop, position loop
- Embedded Power Architecture — designing a robot power distribution board
- EMI and Noise — why motors are electrically noisy; filtering; ground planes; why your sensor glitches when the motor runs

---

## Connection to Other Courses

### Physics

EE is applied physics. Specifically:
- **[[Electrostatics]]** (Physics 2 / Physics 1150) → electric force, electric field, electric potential — the foundations of voltage and capacitance
- **[[Electromagnetism]]** (Physics 2) → magnetic fields, induction, Faraday's law → inductors, motors, transformers
- **[[Maxwell's Equations]]** → the deep foundation of all EE; Kirchhoff's laws are approximations that emerge from Maxwell's equations in the lumped circuit limit

If you take Physics 2 seriously — not just for the grade, but for the conceptual depth — EE circuit theory will feel like you are reading the same ideas in a different language.

### Math

Circuit analysis requires real mathematical fluency:
- **[[Algebra and Ohm's Law]]** — solving $V = IR$ and systems of linear equations for KVL/KCL
- **[[Linear Algebra]]** — large circuit networks become matrix equations; nodal analysis is literally solving $\mathbf{G}\mathbf{v} = \mathbf{i}$
- **[[Differential Equations]]** — capacitors and inductors introduce $dV/dt$ and $dI/dt$ terms; RC and RL circuits are first-order ODEs; RLC circuits are second-order ODEs
- **[[Complex Numbers]]** — phasors and impedance in AC circuits live in the complex plane
- **[[Integration by Parts]]** and [[Trigonometric Substitution]] — appear in energy calculations and Fourier analysis

Calc 2 and Differential Equations are prerequisites for the full frequency-domain analysis needed for filter design. Take them seriously — they are the language EE is written in.

### ML & Robotics

- **[[NumPy Arrays]]** — simulation of circuit networks, sensor data processing
- Motor control fundamentals learned in EE → PID controllers in control theory → reinforcement learning policies in [[Reinforcement Learning]]
- Signal processing learned in AC circuits → feature extraction from sensor data → ML pipelines

---

## Study Sequence

**If you are starting from zero:**
1. [[Voltage, Current, and Resistance]] — internalize the physical meaning before doing any math
2. [[Ohm's Law]] — one formula, deep understanding of when it applies and when it doesn't
3. [[Kirchhoff's Laws]] — the methodology for solving anything more complex than a single resistor
4. [[Series and Parallel Circuits]] — the first simplification technique; practice until it is automatic
5. [[Power in Circuits]] — always check power; this is the link to real-world survival of components

**For the Baymax project specifically:**
Power in Circuits → DC-DC Converters → H-Bridge Motor Drivers → Brushless DC Motors → Motor Control Loops

**For MIT/Stanford transfer preparation:**
Work every derivation yourself, not just the formulas. Understand every step. When you understand *why* KVL works (conservative electric field), not just *that* it works, you can handle the novel problems that MIT admissions expects.
