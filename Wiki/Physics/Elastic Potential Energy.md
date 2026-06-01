# Elastic Potential Energy

**One-liner:** Elastic potential energy is the energy stored in a compressed or stretched spring (or any elastic material), equal to the work done against the spring's restoring force.

## Core Idea
$$U_e = \frac{1}{2}kx^2$$
where $k$ is the spring constant (stiffness, in N/m) and $x$ is the displacement from the natural length (equilibrium). The $\frac{1}{2}$ appears because the spring force is not constant — it increases linearly from 0 to $kx$ as you stretch it. The formula is always positive because $x^2 \geq 0$.

## Why It Exists
Springs (and all elastic materials) store energy when deformed and return it when released. This is a [[Conservative Force]] situation — the work done against a spring is fully recoverable. Elastic PE is the energy accounting for that stored work. Without it, spring problems would require integrating force over displacement every time.

## Real-World Applications
- **Suspension systems (cars, bikes):** springs absorb impact energy from bumps ($U_e = \frac{1}{2}kx^2$ absorbed) and release it gradually, smoothing the ride.
- **Archery:** a drawn bow stores elastic PE in the limbs; release converts it to kinetic energy of the arrow.
- **Earthquake engineering:** seismic isolators under buildings act as springs, storing earthquake energy and returning it slowly to reduce peak forces on the structure.
- **Robotics (Baymax):** soft pneumatic actuators in inflatable robots store and release elastic energy; series elastic actuators (SEA) use spring-like compliance to measure and limit forces, making robots safer around humans.
- **Clock mechanisms:** mainspring stores elastic PE (wound up), releases it slowly to drive gears.
- **DNA mechanics:** at the molecular level, base-pair bonds stretch and store elastic energy when DNA is read by enzymes.

## Intuition
Think of squeezing a rubber ball. The first millimeter costs little force; the last millimeter costs a lot. The *average* force over the whole compression is $\frac{1}{2}kx$ (halfway between 0 and $kx$), so the total work is that average times the displacement: $\frac{1}{2}kx \cdot x = \frac{1}{2}kx^2$. The factor of $\frac{1}{2}$ is literally the average of a ramp that starts at zero.

## Derivation
Hooke's Law says the restoring force is:
$$F_{\text{spring}} = -kx$$
(The negative sign means the force opposes displacement — it's a restoring force.)

The work done against the spring (i.e., the work *you* do to stretch/compress it from 0 to $x$) requires integrating the opposing force:
$$W_{\text{you}} = \int_0^x kx'\, dx' = k\cdot\frac{x^2}{2} = \frac{1}{2}kx^2$$
This work is stored as elastic potential energy:
$$U_e = \frac{1}{2}kx^2$$

The work done *by* the spring when released:
$$W_{\text{spring}} = -\Delta U_e = -\left(\frac{1}{2}kx_f^2 - \frac{1}{2}kx_i^2\right)$$
(Negative because when spring does positive work, PE decreases.)

## Worked Example
A spring with $k = 200\text{ N/m}$ is compressed 0.15 m. A 0.5 kg ball is placed against it and released. Find the ball's speed when the spring returns to its natural length.

**Step 1 — Initial stored elastic PE:**
$$U_{e,i} = \frac{1}{2}(200)(0.15)^2 = \frac{1}{2}(200)(0.0225) = 2.25\text{ J}$$

**Step 2 — Initial kinetic energy:** Ball starts at rest, $K_i = 0$.

**Step 3 — Final state:** Spring at natural length, $U_{e,f} = 0$.

**Step 4 — Apply [[Conservation of Energy]]:**
$$U_{e,i} + K_i = U_{e,f} + K_f$$
$$2.25 + 0 = 0 + \frac{1}{2}(0.5)v_f^2$$

**Step 5 — Solve for $v_f$:**
$$v_f^2 = \frac{2.25}{0.25} = 9 \implies v_f = 3\text{ m/s}$$

## See Also
- [[Conservative Force]] — spring force is conservative; that's why potential energy is definable for it
- [[Conservation of Energy]] — $U_e$ participates alongside $U_g$ and $K$
- [[Gravitational Potential Energy]] — analogous concept for gravity
- [[Work]] — elastic PE is work done against the spring, stored
- [[Kinetic Energy]] — what $U_e$ converts into when the spring releases
- [[Integral]] — $U_e = \frac{1}{2}kx^2$ comes from integrating the spring force
