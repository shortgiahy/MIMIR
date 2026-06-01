# Electric Current

**One-liner:** Electric current is the rate at which electric charge flows past a point in a conductor.

## Core Idea
$$I = \frac{dQ}{dt}$$
Current is measured in Amperes (A), where 1 A = 1 C/s. The derivative form $I = dQ/dt$ captures that current is instantaneous charge flow rate — it can vary in time. For steady (DC) circuits, $I = Q/t$. Current has direction: by convention, current flows from + to − (conventional current), which is opposite to the actual electron drift direction.

## Why It Exists
Current is the quantity engineers actually control and measure in circuits. Voltage drives it, resistance opposes it — but current is what heats wires, spins motors, charges capacitors, and triggers sensors. Without formalizing current, you can't specify wire gauges, fuse ratings, transistor operating points, or motor torque. Every circuit specification — from a 5 mA signal trace to a 30 A motor driver — is a current specification.

## Real-World Applications
- **Motor control (Baymax):** DC motor torque is directly proportional to current. The H-bridge driver controls how much current flows through motor windings. A current-sense resistor measures $I$ to implement closed-loop torque control.
- **LED forward current:** LEDs are current-controlled devices. Run 20 mA → nominal brightness and lifetime. Run 200 mA → dead LED in seconds. Current-limiting resistors are literally designed using $I = V/R$.
- **Sensor biasing:** IR sensors, thermistors, and strain gauges all require a known current or voltage for proper operation — the circuit designer sets this using current dividers and voltage dividers.
- **Battery capacity:** Rated in amp-hours (Ah). A 2 Ah battery can supply 2 A for 1 hour, or 200 mA for 10 hours. Power budget for Baymax is fundamentally a current budget.

## Intuition
Think of a river. The river bed is the conductor. The water is charge. Current is the flow rate — how many gallons per second pass under a bridge. A wider river (lower resistance) moves more water for the same pressure (voltage). A dam (open switch) stops flow entirely. The key subtlety: in a copper wire, the electrons drift incredibly slowly — about 0.1 mm/s — yet current "happens" everywhere in the circuit almost instantly. This is because the electromagnetic field propagates at near light speed; the electrons just shuffle slightly, like people in a full tube train when someone boards at one end.

**Conventional vs. electron flow:** Benjamin Franklin defined positive charge direction before electrons were discovered. Conventional current flows from + to −. Electrons actually flow from − to +. The math works out identically — just be consistent. In circuit analysis, always use conventional current.

## Derivation
**From charge flow:**
If $N$ electrons cross a cross-section in time $\Delta t$:
$$Q = N \cdot e \implies I = \frac{Ne}{\Delta t}$$

**Microscopic model (Drude):**
In a conductor with free electron density $n$ (electrons/m³), cross-sectional area $A$, and drift velocity $v_d$:
$$I = nqv_d A$$
where $q = e$ for electrons. This shows current scales with carrier density (why copper conducts better than silicon), drift speed (why temperature matters), and wire cross-section (why thicker wires carry more current).

**Differential form:**
Current density $J$ (A/m²) is the current per unit area:
$$\vec{J} = nq\vec{v}_d = \sigma\vec{E}$$
where $\sigma$ is conductivity. This is the microscopic Ohm's Law, from which the circuit version $V = IR$ is derived by integrating over the conductor geometry.

## Worked Example
**Problem:** Baymax's arm motor draws 1.8 A during a lift. The motor runs for 3.2 seconds. How much charge passed through the motor windings?

$$Q = I \cdot t = 1.8\,\text{A} \times 3.2\,\text{s} = 5.76\,\text{C}$$

How many electrons is that?
$$n = \frac{Q}{e} = \frac{5.76}{1.602 \times 10^{-19}} \approx 3.6 \times 10^{19}\,\text{electrons}$$

**Follow-up:** The motor driver uses a 0.1 Ω current-sense resistor. What voltage appears across it?
$$V = IR = 1.8\,\text{A} \times 0.1\,\Omega = 0.18\,\text{V} = 180\,\text{mV}$$
This 180 mV signal feeds an ADC (via amplification) for closed-loop current control.

## See Also
- [[Electric Charge]] — current is the time derivative of charge; the two are inseparable
- [[Voltage]] — the potential difference that drives current through a circuit
- [[Ohm's Law]] — the linear relationship between current, voltage, and resistance
- [[Kirchhoff's Current Law]] — at any node, the sum of currents entering equals the sum leaving
- [[Electric Power]] — $P = IV$; current times voltage gives the power transferred
- [[Series Circuit]] — same current flows through every element in a series chain
- [[Parallel Circuit]] — current splits between branches
