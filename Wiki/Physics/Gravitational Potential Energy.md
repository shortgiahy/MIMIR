# Gravitational Potential Energy

**One-liner:** Gravitational potential energy is the energy stored in an object by virtue of its height above a reference point — it is the work done against gravity to lift it there.

## Core Idea
$$U_g = mgh$$
where $m$ is mass, $g \approx 9.8\text{ m/s}^2$, and $h$ is height above a chosen reference level. The reference level (where $h = 0$) is arbitrary — only *changes* in $U_g$ matter physically. This formula holds near Earth's surface (constant $g$).

## Why It Exists
When a force is [[Conservative Force|conservative]], the work it does can be "stored" and recovered. Gravity is conservative — lifting a ball and dropping it returns all the energy. Potential energy is the accounting system for stored work: instead of recalculating the work done by gravity at every instant, we assign an energy value to each height and track changes.

## Real-World Applications
- **Hydroelectric power:** water stored at height has gravitational PE. Releasing it converts that PE to kinetic energy, then to electrical energy via turbines. A 1000 kg/s flow from 100 m height delivers $1000 \times 9.8 \times 100 = 980\text{ kW}$ (ignoring losses).
- **Pendulums and roller coasters:** height converts to speed and back; engineers use $mgh = \frac{1}{2}mv^2$ to find speed at any point without solving differential equations.
- **Trebuchets/catapults:** a heavy counterweight falls, converting gravitational PE to kinetic energy of the projectile.
- **Robotics:** lifting a robot arm or payload requires work against gravity; power calculations and battery life estimates require knowing $\Delta U_g$ for each move.
- **Building design:** elevators do work against gravity; the electrical energy consumed per floor is determined by $\Delta U_g = mg\Delta h$.

## Intuition
Gravitational PE is deferred work — you did the lifting work earlier (against gravity), and gravity is "promising" to return it when the object falls. The higher you lift something, the bigger gravity's IOU. When the object falls, gravity cashes in that IOU, delivering kinetic energy. Nothing is created or destroyed; the energy just changes form.

## Derivation
Gravity exerts a downward force $F_g = mg$ on an object. To lift it slowly (quasi-statically, no kinetic energy gain) by height $h$, you apply an upward force equal to $mg$ over displacement $h$:
$$W_{\text{you}} = F\cdot d = mg\cdot h = mgh$$
This work didn't go into kinetic energy (object moved slowly). It must have gone *somewhere* — we say it became stored as gravitational potential energy:
$$U_g = W_{\text{against gravity}} = mgh$$
Equivalently, the work done *by* gravity when the object falls a height $h$ is:
$$W_{\text{gravity}} = mgh = -\Delta U_g$$
(When the object falls, $U_g$ decreases, and work done by gravity is positive — consistent with this formula.)

**For large heights** where $g$ varies, the exact form is:
$$U_g = -\frac{GMm}{r}$$
(discussed in advanced courses — near Earth's surface, $mgh$ is an excellent approximation.)

## Worked Example
A 70 kg skier starts at the top of a 45 m hill (from rest) and skis to the bottom. Ignoring friction, find their speed at the bottom.

**Step 1 — Set reference height:** Bottom of the hill, $h = 0$, $U_g = 0$.

**Step 2 — Initial energy:**
$$E_i = K_i + U_{g,i} = 0 + mgh = (70)(9.8)(45) = 30{,}870\text{ J}$$

**Step 3 — Final energy (all PE converted to KE, since no friction):**
$$E_f = K_f + U_{g,f} = \frac{1}{2}mv_f^2 + 0$$

**Step 4 — Apply Conservation of Energy ($E_i = E_f$):**
$$30{,}870 = \frac{1}{2}(70)v_f^2 \implies v_f^2 = \frac{30{,}870}{35} = 882 \implies v_f = 29.7\text{ m/s}$$

**Step 5 — Note the mass canceled:** $mgh = \frac{1}{2}mv_f^2 \implies v_f = \sqrt{2gh}$ — the speed at the bottom is independent of mass (Galileo's result).

## See Also
- [[Conservation of Energy]] — $U_g$ participates in the total conserved energy
- [[Work]] — $U_g$ is work done against gravity, stored
- [[Conservative Force]] — gravity is conservative; that's why potential energy is definable
- [[Elastic Potential Energy]] — energy stored in springs (analogous concept)
- [[Kinetic Energy]] — what $U_g$ converts into when the object falls
- [[Work-Energy Theorem]] — the bridge between work and energy change
