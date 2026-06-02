# Electric Charge

**One-liner:** Charge is the fundamental property of matter that determines how strongly a particle interacts with electric and magnetic fields.

## Core Idea
$$Q = n \cdot e, \quad e \approx 1.602 \times 10^{-19} \text{ C}$$
Charge comes in discrete packets — each electron carries exactly $-e$, each proton carries exactly $+e$. The Coulomb (C) is the SI unit. A single coulomb is an enormous amount of charge: about $6.24 \times 10^{18}$ elementary charges. Charge is always conserved — you can redistribute it, but never create or destroy net charge.

## Why It Exists
At the atomic level, matter is made of protons and electrons. Two protons repel each other; a proton and electron attract. This force — the electromagnetic force — is one of the four fundamental forces of nature, and charge is the quantity that "sources" it. Without formalizing charge, there is no way to predict how electrons move through a conductor, how capacitors store energy, or how transistors switch. Every circuit is, at bottom, a controlled flow of electrons from regions of high potential to low.

## Real-World Applications
- **Current in wires:** When you connect a battery, the voltage pushes free electrons through the conductor. The rate of that charge flow is current.
- **Capacitors:** Charge is physically separated and stored on capacitor plates — the basis of every filter, power rail, and ADC in embedded systems.
- **Baymax sensor circuits:** Analog sensors (IR distance, current sensors) output tiny voltages produced by redistributed charge. Understanding charge is prerequisite to understanding why sensors saturate or need biasing.
- **Static ESD:** A microburst of charge (electrostatic discharge) can destroy a GPIO pin — knowing charge quantization explains why grounding matters during assembly.

## Intuition
Think of charge like a fluid — but one that has two flavors: positive and negative. Unlike water, opposite flavors attract and annihilate each other's effects when mixed in equal amounts (neutrality). Like flavors push each other away. The "pressure" this repulsion creates is what we call voltage. The key physical reality: charge is not just an abstract number. It's a property that causes force. Two electrons one centimeter apart push each other with $F = ke^2/r^2 \approx 2.3 \times 10^{-24}$ N — tiny but real and the source of all electrical behavior.

## Derivation
**Coulomb's Law from field concept:**

The electric force between two point charges $q_1$ and $q_2$ separated by distance $r$:
$$F = k_e \frac{q_1 q_2}{r^2}, \quad k_e = \frac{1}{4\pi\epsilon_0} \approx 8.99 \times 10^9 \text{ N·m}^2/\text{C}^2$$

**Conservation of charge:**
In any closed system: $\sum Q_i = \text{constant}$

This follows from gauge invariance in quantum mechanics (Noether's theorem), but the engineering take: charge cannot appear from nothing. In a circuit, every electron that leaves the negative terminal of a battery must return through the positive terminal. This is the physical basis for Kirchhoff's Current Law.

**Quantization:**
Millikan's oil drop experiment (1909) showed charge only appears in integer multiples of $e$. At the circuit level, this discreteness is invisible — you're dealing with $10^{18}$ or more electrons — but it becomes critical in quantum devices and nanoscale transistors.

## Worked Example
**Problem:** A capacitor in Baymax's motor driver is charged to 24 V. It has capacitance $C = 100\,\mu\text{F}$. How many electrons are stored on the negative plate?

$$Q = CV = (100 \times 10^{-6}\,\text{F})(24\,\text{V}) = 2.4 \times 10^{-3}\,\text{C} = 2.4\,\text{mC}$$

Number of electrons:
$$n = \frac{Q}{e} = \frac{2.4 \times 10^{-3}}{1.602 \times 10^{-19}} \approx 1.5 \times 10^{16}\,\text{electrons}$$

That's 15 quadrillion electrons sitting on a plate the size of a thumbnail. This is why capacitor discharge is dangerous — all that charge moves almost instantaneously if shorted.

## See Also
- [[Electric Current]] — current is the rate of charge flow; $I = dQ/dt$
- [[Voltage]] — voltage is the potential energy difference per unit charge that drives charge movement
- [[Kirchhoff's Current Law]] — charge conservation applied to circuit nodes
- [[Conservation of Momentum]] — analogous conservation law in mechanics; both come from deeper symmetry principles (Noether's theorem)
- [[Electric Field]] — the field that exerts force on charges and mediates all electrical interactions
- [[Conservation of Energy]] — charge conservation and energy conservation are sibling Noether conservation laws; together they underpin all of circuit analysis (KCL from charge, KVL from energy)
