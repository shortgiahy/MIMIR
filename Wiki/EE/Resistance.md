# Resistance

**One-liner:** Resistance is the opposition a material offers to the flow of electric current, determined by the material's intrinsic resistivity and its physical geometry.

## Core Idea
$$R = \rho \frac{L}{A}$$
Resistance ($R$) is measured in Ohms (Ω). The formula links macroscopic resistance to the material's resistivity $\rho$ (Ω·m), the conductor length $L$, and cross-sectional area $A$. Longer, thinner, and more resistive materials yield higher resistance. This is not just a circuit abstraction — it's a direct consequence of how frequently electrons collide with the material's atomic lattice.

## Why It Exists
Not all materials let current flow equally. A copper wire passes enormous current; a glass rod passes essentially none. Between these extremes are semiconductors, carbon composites, metal alloys — each with different resistivity spanning 25 orders of magnitude. Resistance lets engineers specify exactly how much a material impedes current, enabling precise control of current, voltage division, power dissipation, and signal conditioning. Without it, no transistor biasing, no sensor conditioning, no current limiting.

## Real-World Applications
- **Current limiting:** Every LED needs a series resistor. $R = (V_{supply} - V_f) / I_{forward}$. Without it, the LED pulls unlimited current and dies.
- **Sensor circuits:** A thermistor (temperature-sensitive resistor) changes resistance with temperature. Paired with a fixed resistor in a voltage divider, this converts temperature to a measurable voltage — the basis of Baymax's thermal sensing.
- **PCB trace resistance:** Copper traces have resistance. At 30 A (motor driver), a 1 mΩ trace resistance drops 30 mV and dissipates 0.9 W — enough to cause thermal problems. PCB designers use $R = \rho L/A$ to spec trace widths.
- **Pull-up/pull-down resistors:** GPIO pins float without a defined voltage when disconnected. A 10 kΩ pull-up to VCC ensures a defined logic HIGH. Resistance chosen: low enough to override noise, high enough not to waste significant current.

## Intuition
Imagine electrons as balls rolling through a tube filled with fixed obstacles (lattice atoms). More obstacles (higher temperature, impure metal, longer tube) means more collisions, more resistance. A wider tube allows more balls to squeeze through simultaneously — lower resistance. At each collision, kinetic energy transfers to the obstacle as heat — this is Joule heating.

**Why resistance is a material property, not just a circuit property:** Two identical resistors made from different materials (say, nichrome vs. copper) with the same geometry have wildly different resistances — by a factor of ~60. This is purely because nichrome's atomic structure causes electrons to collide 60× more frequently per unit length.

**Temperature dependence:** Metals get more resistive as temperature rises (more lattice vibrations = more collisions): $R(T) = R_0[1 + \alpha(T - T_0)]$. Semiconductors (and thermistors) do the opposite: heat provides energy to free more carriers, dropping resistance. This difference is fundamental to electronics design.

## Derivation
**From microscopic Ohm's Law:**

In a conductor, current density $\vec{J} = \sigma \vec{E}$, where $\sigma$ is conductivity ($\sigma = 1/\rho$).

For a cylindrical conductor of length $L$ and area $A$, with uniform field $E = V/L$:

$$I = JA = \sigma E A = \sigma \frac{V}{L} A$$

Rearranging:
$$V = \frac{L}{\sigma A} I = \rho \frac{L}{A} I = RI$$

So $R = \rho L/A$ emerges naturally from the microscopic model. This also shows that Ohm's Law ($V = IR$) is a macroscopic consequence of the linear $J = \sigma E$ relationship in ohmic materials.

**Where does $\sigma = 1/\rho$ come from (Drude model)?**

Electrons accelerate under field $E$, then scatter off lattice ions every $\tau$ seconds (mean free time):
$$v_d = \frac{eE\tau}{m_e} \implies J = nev_d = \frac{ne^2\tau}{m_e} E$$

Therefore: $\sigma = \frac{ne^2\tau}{m_e}$ and $\rho = \frac{m_e}{ne^2\tau}$

Resistivity increases with temperature because $\tau$ (time between collisions) decreases as lattice vibrates more.

## Worked Example
**Problem:** Baymax needs a 100 kΩ resistor for a sensor pull-up but only has resistor decades in 1 kΩ, 10 kΩ, and 47 kΩ. How to get 100 kΩ using two resistors?

Two resistors in series: $R_{total} = R_1 + R_2$. Use $47\,\text{k}\Omega + 47\,\text{k}\Omega = 94\,\text{k}\Omega$ (close but not 100 k).

Better: $47\,\text{k}\Omega + 10\,\text{k}\Omega + 47\,\text{k}\Omega = 104\,\text{k}\Omega$ — three resistors in series.

Or use two 47 kΩ in series = 94 kΩ (2% off from 100 kΩ — likely acceptable if pull-up tolerance is loose).

**Trace resistance check:** A PCB trace for a motor supply is 0.035 mm thick (1 oz copper), 3 mm wide, 50 mm long. Copper: $\rho = 1.72 \times 10^{-8}\,\Omega\cdot\text{m}$.

$$R = \rho \frac{L}{A} = 1.72 \times 10^{-8} \times \frac{0.05}{(0.003)(0.000035)} = 1.72 \times 10^{-8} \times \frac{0.05}{1.05 \times 10^{-7}} \approx 8.2\,\text{m}\Omega$$

At 5 A: $V_{drop} = 5 \times 0.0082 = 41\,\text{mV}$, $P = I^2 R = 25 \times 0.0082 = 0.2\,\text{W}$. Fine for 5 A; at 30 A this becomes a 1.8 W heating problem.

## See Also
- [[Ohm's Law]] — resistance appears directly as $R = V/I$; this is the defining relationship
- [[Electric Current]] — resistance determines how much current flows for a given voltage
- [[Voltage Divider]] — resistor ratios divide voltage; depends entirely on resistance values
- [[Joule Heating]] — $P = I^2R$; energy dissipated as heat due to resistance
- [[Series Circuit]] — resistances add in series: $R_{total} = \sum R_i$
- [[Parallel Circuit]] — resistances combine as reciprocals in parallel
- [[Electric Field]] — resistivity links the microscopic field to macroscopic resistance
