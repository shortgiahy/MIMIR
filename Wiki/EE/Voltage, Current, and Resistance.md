# Voltage, Current, and Resistance

**One-liner:** Voltage is the pressure that drives charges, current is the flow of charges, and resistance is what opposes that flow — three quantities that together describe every classical circuit.

## Why It Exists

Electric charge exists. Opposite charges attract, like charges repel. This is just nature. But engineers needed a way to quantify *how hard* charges are being pushed, *how much* charge is actually moving, and *how difficult* a material makes that movement. Without these three quantities, you cannot predict what any circuit will do. They are not conventions — they are direct measurements of physical reality.

The formalization came slowly. Charles-Augustin de Coulomb characterized electric force in the 1780s. Alessandro Volta built the first practical battery around 1800 and had to invent the concept of "potential difference" to explain it. André-Marie Ampère characterized current flow in the 1820s. Georg Ohm experimentally discovered the relationship between all three in 1827. Each quantity was invented because something real needed to be described.

## The Concept

### Electric Charge

Start from the bottom. Matter is made of atoms. Atoms contain protons (+) and electrons (−). In most materials, electrons are bound tightly to nuclei and stay put. In conductors — metals especially — some electrons (called *conduction electrons* or *free electrons*) are delocalized. They float around in a sea shared by all the atoms. They can move.

Charge is measured in **coulombs (C)**. One coulomb is the charge of approximately $6.242 \times 10^{18}$ protons (or the magnitude of charge of the same number of electrons). The elementary charge $e = 1.602 \times 10^{-19}$ C per electron.

### Voltage (Electric Potential Difference)

Place two regions of charge at different "charge densities" — one with excess positive charge, one with excess negative charge. The electric field between them will push positive charges from the + region toward the − region. This separation of charge represents stored potential energy, exactly like a compressed spring or a ball held above the ground.

**Voltage is electric potential energy per unit charge.** It tells you: if I place a 1-coulomb test charge at this point in space, how much potential energy does it have?

$$V = \frac{U}{q}$$

where $U$ is potential energy in joules, $q$ is charge in coulombs, and $V$ is voltage in volts (V). 1 volt = 1 joule per coulomb.

The critical insight: **voltage is always a difference between two points.** There is no such thing as "the voltage at point A" in isolation — only "the voltage at point A *relative to* point B." When you say "the battery is 9V," you mean the voltage across its terminals (+ terminal relative to − terminal) is 9V. The − terminal is your reference, often called *ground* and assigned 0V by convention.

Think of it like altitude: the height of a mountain only makes sense relative to some reference (sea level). A 3000m mountain isn't "3000m" in any absolute sense — it's 3000m above sea level.

### Current

When voltage exists across a conductor, the electric field pushes free electrons. They drift — slowly, en masse — toward the higher potential. This collective movement is **electric current**.

$$I = \frac{dq}{dt}$$

Current is the rate of charge flow: coulombs per second. 1 ampere (A) = 1 coulomb per second.

**Conventional current vs. electron flow:** This is one of the great confusions in EE. Benjamin Franklin guessed wrong about which charges move — he assumed positive charges flow. By the time it was discovered that electrons (negative charges) are the actual movers in metals, the convention was locked in. 

**Conventional current** flows from + to −, from high voltage to low voltage. **Electron flow** is the opposite, from − to +. Both describe the same physics. In circuit analysis, always use conventional current — it matches the direction assumed by Ohm's Law, KVL, KCL, and every circuit diagram convention. Just remember: when an electron moves left, conventional current flows right.

In semiconductors, both positive holes and negative electrons can be charge carriers, so the distinction gets murkier. For basic circuits: conventional current, always.

### Resistance

Different materials resist charge flow differently. Copper has ~$1.7 \times 10^{-8}\ \Omega \cdot m$ resistivity. Glass has ~$10^{10}\ \Omega \cdot m$ resistivity — roughly 18 orders of magnitude more resistive. Why?

Electrons moving through a lattice of atoms collide with those atoms. The more collisions, the harder it is to maintain current. Resistivity $\rho$ is a material property. Resistance $R$ also depends on geometry:

$$R = \rho \frac{L}{A}$$

where $L$ is the length of the conductor and $A$ is its cross-sectional area. Longer path → more collisions → more resistance. Fatter wire → more parallel paths → less resistance.

Resistance is measured in **ohms (Ω)**. 1 Ω = 1 volt per ampere.

Temperature matters: in most metals, resistance increases with temperature because thermal vibration increases atomic motion, causing more electron collisions. This is why a light bulb filament has very different resistance when cold vs. at operating temperature — a key gotcha in circuit design.

### Physical Picture in a Circuit

A battery separates charges and maintains a voltage difference between its terminals using a chemical reaction. Connect a wire between the terminals: electrons experience the electric field, drift through the wire, and the chemical energy is converted into kinetic energy of moving charges and ultimately into heat (in the wire) or work (in a motor, LED, etc.).

The battery is the pump. Voltage is the pressure. Current is the flow rate. Resistance is the restriction. All three interact — governed by Ohm's Law.

## Intuition

The water analogy is standard for good reason, but here is a precise version: 

- A tank of water elevated above a pipe outlet is like a battery — the height difference (voltage) creates pressure (potential difference). The higher the tank, the more "pressure."
- The flow rate of water out the pipe (liters/second) is current. 
- A narrow pipe resists flow — that is resistance. A wide pipe lets more flow for the same pressure.

Where the analogy breaks down: water is continuous and visible; electric charge is discrete and invisible. Water always flows downhill; electrons flow opposite to conventional current. The analogy is a scaffold — use it to build the real model, then discard it.

A better intuition for resistance: imagine a crowd of people trying to move through a corridor. If the corridor is long and narrow, progress is slow. If it is short and wide, the crowd moves easily. Add obstacles (thermal vibration of atoms) and the crowd (electrons) moves even more slowly. Resistance is crowd control for electrons.

For voltage: two concepts that help MIT-level students stop confusing voltage and current:
1. Voltage exists *between* nodes. Current flows *through* a branch. These are different things located at different topological features of a circuit.
2. Voltage can exist with zero current (open circuit — no path). Current requires a complete loop. You can have voltage without current, but you cannot have current without voltage driving it (in a purely resistive circuit).

## Key Formula / Rule

$$V = I \cdot R \quad \text{(Ohm's Law, for linear resistors)}$$

$$R = \rho \frac{L}{A} \quad \text{(resistance from geometry and material)}$$

$$I = \frac{dq}{dt} \quad \text{(current as rate of charge flow)}$$

$$V = \frac{U}{q} \quad \text{(voltage as potential energy per charge)}$$

## Worked Example

**Problem:** A 12V car battery powers a tail light with a resistance of 6Ω. What current flows through the bulb, and how much charge passes through it in 5 minutes?

**Step 1: Apply Ohm's Law to find current.**

The voltage across the bulb is 12V (the full battery voltage, assuming the wires have negligible resistance). Using $V = IR$:

$$I = \frac{V}{R} = \frac{12\text{ V}}{6\ \Omega} = 2\text{ A}$$

Two amperes of conventional current flows through the bulb from the + terminal through the bulb to the − terminal.

**Step 2: Find total charge flow in 5 minutes.**

Current is charge per unit time: $I = q/t$, so $q = I \cdot t$.

Five minutes = 300 seconds.

$$q = I \cdot t = 2\text{ A} \times 300\text{ s} = 600\text{ C}$$

Six hundred coulombs of charge passes through the filament. To put that in electron count:

$$n = \frac{600\text{ C}}{1.602 \times 10^{-19}\text{ C/electron}} \approx 3.75 \times 10^{21}\text{ electrons}$$

That is roughly 4 sextillion electrons per 5-minute interval — which illustrates why we use amperes instead of counting individual charges.

**Step 3: Physical check.**

A 2A current through a 6Ω resistor seems reasonable for a car tail light. Power = IV = 2A × 12V = 24W — plausible for an older incandescent bulb (modern LED tail lights draw much less). The units work: A × Ω = (C/s) × (V/A) = V. ✓

## Gotchas

**Voltage is not current.** This is the #1 beginner error. "The voltage going through the resistor" is physically meaningless — current goes *through*, voltage appears *across*. High voltage does not mean high current. A Van de Graaff generator produces millions of volts at microamperes — you can touch it safely. A car battery is 12V but can deliver hundreds of amperes into a fault — that will kill you.

**Ground is arbitrary.** Labeling a node "0V" or "ground" is a human convenience, not a physical constraint. The laws of physics only care about voltage *differences*. You can move ground anywhere and the currents and voltage differences remain unchanged. Changing the reference doesn't change the physics, only the labels.

**Resistance is not constant.** Ohm's Law (V = IR) is only valid for *linear* (ohmic) resistors. Diodes, transistors, LEDs, and many other components have resistance that varies with voltage, current, and temperature. The formula V = IR always holds as a definition (resistance is always V/I at any operating point), but $R$ may not be a fixed constant.

**Conventional current direction.** Always use conventional current (+ to −) in circuit analysis. Forgetting this and using electron flow produces wrong answers, because all formulas (V = IR, KVL, KCL) assume conventional current.

**Temperature dependence.** Resistance changes with temperature. For precision work — or when something could burn out — you cannot ignore this. Thermistors exploit this effect deliberately. Tungsten (light bulb filaments) has resistance that changes by ~10× between room temperature and operating temperature.

## See Also
- [[Ohm's Law]] — the fundamental relationship between voltage, current, and resistance
- [[Kirchhoff's Laws]] — the laws governing how voltages and currents distribute across multiple elements
- [[Series and Parallel Circuits]] — how resistance, voltage, and current behave when elements are combined
- [[Power in Circuits]] — how voltage and current together determine energy dissipation
