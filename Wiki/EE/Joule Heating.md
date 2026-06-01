# Joule Heating

**One-liner:** Joule heating is the irreversible conversion of electrical energy into thermal energy in a resistor, quantified by $P = I^2R$ — the power dissipated as heat by a current $I$ through resistance $R$.

## Core Idea
$$P = I^2 R$$
Any resistive element carrying current generates heat. The power dissipated is proportional to the *square* of current and linearly proportional to resistance. The temperature rise above ambient is governed by thermal resistance $\theta$ (°C/W):
$$\Delta T = \theta \cdot P = \theta \cdot I^2 R$$
Joule heating is irreversible — unlike energy stored in a capacitor or inductor, thermal energy dissipated in a resistor cannot be recovered electrically.

## Why It Exists
Electrons collide with lattice atoms as they drift through a conductor. Each collision transfers kinetic energy to the lattice — vibrating the crystal structure, which manifests as heat. The rate of these collisions scales with current (more electrons per second = more collisions per second) and with resistance (more obstructions = more collisions per electron). Joule heating is not a design flaw; it is an unavoidable consequence of current flowing through any non-zero resistance. Engineers formalized it to predict component temperatures, set current ratings, and design thermal management systems.

## Real-World Applications
- **Wire current ratings (ampacity):** Every wire gauge (AWG) has a maximum current rating based on allowable temperature rise. A 24 AWG wire (resistance ≈ 84 mΩ/m) carrying 3 A generates $P = (3)^2 \times 0.084 = 0.76\,\text{W/m}$ of heat. Exceed the rating → insulation melts → fire. Wire gauges for Baymax's motor leads must be sized for the stall current, not nominal.
- **Motor driver MOSFETs:** A MOSFET with $R_{DS(on)} = 10\,\text{m}\Omega$ carrying 5 A dissipates $P = (5)^2 \times 0.01 = 0.25\,\text{W}$. Manageable without a heatsink. At 20 A: $P = 400 \times 0.01 = 4\,\text{W}$ — requires heatsinking or a lower $R_{DS(on)}$ device.
- **PCB trace thermal design:** Thin copper traces have measurable resistance. A 1 mm wide, 35 μm thick, 10 cm long trace has $R \approx 50\,\text{m}\Omega$. At 2 A: $P = 4 \times 0.05 = 0.2\,\text{W}$, raising trace temperature by $\Delta T = \theta \cdot P$. PCB thermal resistance for a trace is roughly 70 °C/W (in free air) — giving $\Delta T \approx 14\,°\text{C}$ above ambient.
- **Baymax motor winding heating:** DC motors have winding resistance $R_{coil}$. At stall, back-EMF = 0 and all voltage drives current through $R_{coil}$. Stall current can be 5–10× running current; stall power $I_{stall}^2 R_{coil}$ can instantly overheat windings. Baymax's firmware must detect stall and cut power within milliseconds.
- **Heating elements (intentional):** Toasters, electric heaters, and 3D printer hot-ends deliberately use Joule heating. $P = V^2/R$ is used here: fixed voltage, chosen resistance to hit target power.
- **Fuses:** A fuse wire is designed to melt (open circuit) when current exceeds a threshold. The melting point is reached when $\Delta T = \theta \cdot I^2 R_{fuse}$ hits the wire's melting temperature.

## Intuition
Picture electrons (a flowing crowd) in a corridor (the conductor). The corridor is lined with obstacles (lattice atoms). Every time a person bumps an obstacle, they transfer some kinetic energy to it — the obstacle jiggles more vigorously, which is heat. Double the crowd density (double $I$): twice as many collisions per second per obstacle. But also each person in a denser crowd has more frequent collisions — so total collision rate goes as $I^2$. This is why power scales as the *square* of current, not linearly.

**Thermal resistance analogy — the thermal Ohm's Law:**
Just as $V = IR$ describes voltage drop due to electrical resistance, there is a direct thermal analog:
$$\Delta T = \theta \cdot P$$
- $\Delta T$ is the temperature difference (°C) — analogous to voltage
- $P$ is the power (W) — analogous to current
- $\theta$ is the thermal resistance (°C/W) — analogous to electrical resistance

Thermal resistances add in series (junction → case → heatsink → air) and combine in parallel when there are multiple heat dissipation paths. Every datasheet-quoted $\theta_{JA}$ (junction-to-ambient thermal resistance) is this quantity.

**The squaring is the critical insight:** Doubling the current quadruples the heat. This means a 10% overload on current causes a 21% increase in heat ($1.1^2 = 1.21$). Current ratings are hard limits for this reason.

## Derivation
**From the Drude model and power:**

From the derivation of Ohm's Law via the Drude model: electrons drift at velocity $v_d = eE\tau/m_e$. The power delivered by the electric field to the electrons per unit volume is:
$$p = \vec{J} \cdot \vec{E} = \sigma E^2$$

In steady state, all this power is transferred to lattice vibrations (heat) — electrons don't accelerate indefinitely because the energy gained from the field is lost in each collision.

**Macroscopic derivation from $P = IV$:**

From [[Electric Power]]: $P = IV$. For a resistor, $V = IR$ (Ohm's Law). Substituting:
$$P = I \cdot (IR) = I^2 R$$

Or equivalently, substituting $I = V/R$:
$$P = IV = \frac{V}{R} \cdot V = \frac{V^2}{R}$$

**Energy dissipated over time:**

If current $I$ is constant over time interval $t$:
$$E_{heat} = P \cdot t = I^2 R t \quad \text{(joules)}$$

For time-varying current $i(t)$:
$$E_{heat} = \int_0^T i^2(t) \, R \, dt = R \int_0^T i^2(t) \, dt$$

This integral is why RMS current (root-mean-square) is used for AC circuits: $I_{RMS}^2 = \frac{1}{T}\int_0^T i^2(t)\,dt$, so $P = I_{RMS}^2 R$ — the same formula works for AC if you use RMS current.

**Thermal model:**

At steady state, the heat generated equals heat dissipated to the environment:
$$P_{generated} = \frac{\Delta T}{\theta}$$

Solving for temperature rise:
$$\Delta T = \theta \cdot P = \theta \cdot I^2 R$$

For multiple thermal resistances in series (package → heatsink → ambient):
$$\theta_{total} = \theta_{JC} + \theta_{CS} + \theta_{SA}$$

where JC = junction-to-case, CS = case-to-sink, SA = sink-to-ambient.

## Worked Example
**Problem:** Baymax's left-arm servo motor has winding resistance $R_{coil} = 3\,\Omega$. The motor controller allows a maximum winding temperature of 85 °C. Ambient temperature is 25 °C. The motor's thermal resistance (winding to ambient) is $\theta = 12\,\text{°C/W}$.

**Step 1 — Maximum allowable power dissipation:**
$$\Delta T_{max} = 85 - 25 = 60\,\text{°C}$$
$$P_{max} = \frac{\Delta T_{max}}{\theta} = \frac{60}{12} = 5\,\text{W}$$

**Step 2 — Maximum allowable current:**
$$P = I^2 R \implies I_{max} = \sqrt{\frac{P_{max}}{R}} = \sqrt{\frac{5}{3}} = \sqrt{1.67} \approx 1.29\,\text{A}$$

**Step 3 — Verify stall current safety.**
At stall, back-EMF = 0. If supply voltage is 6 V:
$$I_{stall} = \frac{V}{R_{coil}} = \frac{6}{3} = 2\,\text{A}$$
$$P_{stall} = I_{stall}^2 R = 4 \times 3 = 12\,\text{W}$$
$$\Delta T_{stall} = \theta \cdot P_{stall} = 12 \times 12 = 144\,\text{°C above ambient} \rightarrow 169\,\text{°C winding temperature}$$

Far above the 85 °C limit. Baymax's firmware must detect stall (via current spike or encoder stall) and shut down within approximately:
$$t_{safe} = \frac{E_{thermal}}{P_{stall} - P_{max}} \approx \frac{C_{th} \cdot \Delta T_{allowed}}{P_{stall} - P_{max}}$$

In practice: motor datasheet specifies maximum stall duration. Implement a current-time protection curve (like a thermal fuse in firmware).

## See Also
- [[Electric Power]] — $P = I^2R$ is one of three equivalent forms of $P = IV$; Joule heating is the thermal manifestation
- [[Ohm's Law]] — used to substitute $V = IR$ into $P = IV$, producing the $I^2R$ form
- [[Resistance]] — the material property that determines how much heat is generated per ampere squared
- [[Electric Current]] — the quadratic dependence on current is what makes current ratings so critical
- [[Conservation of Energy]] — Joule heating is irreversible energy conversion; electrical potential energy becomes thermal energy with no return path
- [[Series Circuit]] — in series, all resistors carry the same current; the highest-resistance element dissipates the most power
- [[Parallel Circuit]] — in parallel, all resistors share the same voltage; the lowest-resistance element dissipates the most power
