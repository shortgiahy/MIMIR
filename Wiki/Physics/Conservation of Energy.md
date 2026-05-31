# Conservation of Energy

**One-liner:** The total mechanical energy of a system remains constant when only conservative forces act — energy is not created or destroyed, only transformed between kinetic and potential forms.

## Why It Exists

After [[Work and Kinetic Energy]] gave us the work-energy theorem, physicists noticed something remarkable: for certain forces — gravity, springs — the work done does not depend on the path taken. Only the starting and ending positions matter. Carry a book from floor to table by going straight up, or by winding around the room — gravity does the same work either way.

This "path-independence" property meant those forces could be described not by tracking work at every step, but by assigning a single number to every position in space: **potential energy**. Once you have potential energy, conservation of energy becomes an accounting identity — energy shifts from one form to another but the total never changes.

Why does this matter? Because it lets you jump from initial to final state without solving differential equations or tracking forces along a path. A roller coaster problem that would require integrating $F = ma$ along a curved track becomes: "kinetic energy gained = potential energy lost." Three lines instead of three pages.

The even deeper reason: conservation of energy is not an observation that happens to be true — it is a consequence of the *time-translation symmetry* of physics (Noether's theorem: if the laws of physics are the same today as they were yesterday, then energy must be conserved). This is why it is so universal and why it works.

## The Concept

### Conservative vs. Non-Conservative Forces

Not all forces conserve energy in a mechanical sense. The distinction is critical.

**A conservative force** is one where the work done depends *only* on the starting and ending positions, not on the path taken. Equivalently: the work done around any closed loop is zero.

Examples of conservative forces:
- Gravity (near Earth's surface and universal)
- Spring force (Hooke's Law)
- Electrostatic force

**A non-conservative force** is one where the work done *does* depend on the path — or equivalently, energy is permanently converted to an irrecoverable form (mostly heat).

Examples of non-conservative forces:
- Kinetic friction
- Air drag
- Any force where energy is "lost" as heat

For conservative forces, we can define **potential energy**. For non-conservative forces, we cannot — but we can still track energy; it just flows out of the mechanical system.

### Potential Energy: The Formal Definition

For a conservative force $\vec{F}$, the potential energy function $U(x)$ is defined by:

$$W_\text{conservative} = -\Delta U = -(U_f - U_i) = U_i - U_f$$

The negative sign is intentional and crucial. It encodes the following logic:

- When a conservative force does *positive* work (e.g., gravity pulling a falling object down), the object speeds up — kinetic energy increases. To keep total energy constant, potential energy must *decrease*.
- When a conservative force does *negative* work (e.g., gravity opposing a rising object), the object slows — kinetic energy decreases. Potential energy must *increase*.

So potential energy is the "stored" form — it decreases when the force does positive work, and increases when the force does negative work.

### Gravitational Potential Energy (Near Earth)

For constant gravitational acceleration $g$ near Earth's surface:

$$U_g = mgy$$

where $y$ is height measured from whatever reference point you choose (the choice of reference doesn't matter, because only $\Delta U$ appears in equations).

Derivation: the work done by gravity when an object falls from height $y_i$ to $y_f$ is:

$$W_\text{gravity} = mg(y_i - y_f) = -(mgy_f - mgy_i) = -\Delta U_g \checkmark$$

If the object rises ($y_f > y_i$), gravity does negative work and $\Delta U_g > 0$ — potential energy increases. If it falls ($y_f < y_i$), gravity does positive work and $\Delta U_g < 0$ — potential energy decreases. Consistent.

### Elastic Potential Energy (Spring)

For a spring compressed or stretched by $x$ from its natural length:

$$U_\text{spring} = \frac{1}{2}kx^2$$

where $k$ is the spring constant (stiffness). Note the same $\frac{1}{2}$ that appears in kinetic energy — it comes from integrating the spring force $F = -kx$ over displacement $x$:

$$W_\text{spring} = -\int_0^x kx'\, dx' = -\frac{1}{2}kx^2 = -\Delta U_\text{spring} \checkmark$$

### Deriving Conservation of Mechanical Energy

Start from the work-energy theorem:

$$W_\text{net} = \Delta K$$

Split the net work into conservative and non-conservative parts:

$$W_\text{conservative} + W_\text{non-conservative} = \Delta K$$

Use the definition $W_\text{conservative} = -\Delta U$:

$$-\Delta U + W_\text{non-conservative} = \Delta K$$

Rearrange:

$$W_\text{non-conservative} = \Delta K + \Delta U = \Delta E_\text{mechanical}$$

**If there are no non-conservative forces** (no friction, no drag), $W_\text{non-conservative} = 0$, so:

$$\Delta E_\text{mechanical} = \Delta K + \Delta U = 0$$

$$K_i + U_i = K_f + U_f$$

$$\boxed{E = K + U = \text{constant}}$$

This is **conservation of mechanical energy**. Every bit of derivation followed from the work-energy theorem — which itself came from $F = ma$.

### When Non-Conservative Forces Act

When friction is present, the mechanical energy is *not* conserved. The full equation becomes:

$$K_i + U_i + W_\text{nc} = K_f + U_f$$

where $W_\text{nc}$ is the work done by non-conservative forces (typically negative for friction, $W_\text{friction} = -f_k d$). This is sometimes written as:

$$E_\text{mech,f} = E_\text{mech,i} - \Delta E_\text{thermal}$$

where $\Delta E_\text{thermal} = f_k d > 0$ is the energy "lost" to heat. The energy is still conserved — it just left the mechanical system.

### Solving Problems with Energy Methods vs. Newton's Laws

**Use energy methods when:**
- You want to find speed at a specific point, and you don't care about the path or intermediate values
- Forces vary with position (springs, curved paths)
- The path is complicated (ramps, curves, pendulums)
- No friction, or friction with a known distance

**Use Newton's Second Law when:**
- You need forces (tension, normal force) rather than speed
- You need position or time as a function of time (not just at endpoints)
- The problem asks about forces during the motion, not just at start and end
- Acceleration is uniform and simple kinematics works

Many problems can be solved both ways — and checking that both methods give the same answer is an excellent sanity check.

### Energy Diagrams

Drawing a sketch with labeled energy values at each key position is invaluable:

```
Position:     Top of ramp    Bottom of ramp
Height:       h              0
KE:           0              ?
PE:           mgh            0
Total E:      mgh            mgh (same!)
```

At the top: all energy is potential (if released from rest, $K = 0$).
At the bottom: all energy is kinetic ($U = 0$ if reference is there).
In between: a mix, but the sum is always $mgh$.

## Intuition

Think of a bank account. Potential energy is money in your savings. Kinetic energy is cash in your wallet. You can move money between savings and wallet freely, but the total amount in both together doesn't change (assuming no fees — friction is the fee).

At the top of a roller coaster: high savings (potential), empty wallet (zero kinetic). At the bottom of the drop: empty savings, full wallet. Halfway down: half and half. The bank total never changed.

The reason this works is that conservative forces are "perfectly reversible." Gravity will give back exactly the energy it stores. Friction, in contrast, is irreversible — once the book slides across the table and heats it up, that thermal energy is scattered into trillions of molecular vibrations and will not spontaneously re-assemble into motion of the book.

## Key Formula / Rule

Conservation of mechanical energy (no friction):

$$K_i + U_i = K_f + U_f$$

Expanded:

$$\frac{1}{2}mv_i^2 + mgy_i = \frac{1}{2}mv_f^2 + mgy_f$$

With non-conservative work:

$$K_i + U_i + W_\text{nc} = K_f + U_f$$

Gravitational PE:
$$U_g = mgy$$

Spring PE:
$$U_\text{spring} = \frac{1}{2}kx^2$$

## Worked Example

**Problem:** A $2.0\ \text{kg}$ ball is launched from the top of a frictionless ramp that is $5.0\ \text{m}$ above the ground. It starts from rest. How fast is it moving at ground level?

**Setup:** No friction, no spring, only gravity. Conservation of mechanical energy applies.

Choose ground as the reference height: $y_f = 0$.

**Known values:**
- $m = 2.0\ \text{kg}$
- $v_i = 0$ (starts from rest)
- $y_i = 5.0\ \text{m}$
- $y_f = 0$

**Apply conservation of energy:**

$$\frac{1}{2}mv_i^2 + mgy_i = \frac{1}{2}mv_f^2 + mgy_f$$

$$0 + (2.0)(9.8)(5.0) = \frac{1}{2}(2.0)v_f^2 + 0$$

$$98\ \text{J} = v_f^2$$

$$v_f = \sqrt{98} \approx 9.9\ \text{m/s}$$

Notice: **mass canceled out**. The speed at the bottom of a frictionless ramp depends only on height, not on the object's mass. A $2\ \text{kg}$ ball and a $200\ \text{kg}$ boulder released from the same height reach the same speed at the bottom. This is Galileo's finding from 1590 — the energy equation explains *why* it's true.

**Extension — with friction:** Suppose the ramp has kinetic friction and $f_k d = 12\ \text{J}$ (friction does $-12\ \text{J}$ of work over the ramp length).

$$K_i + U_i + W_\text{friction} = K_f + U_f$$

$$0 + 98 + (-12) = \frac{1}{2}(2.0)v_f^2 + 0$$

$$86 = v_f^2$$

$$v_f = \sqrt{86} \approx 9.3\ \text{m/s}$$

Less than 9.9 m/s, as expected — friction robbed some energy.

## Gotchas

**1. Reference height is arbitrary — but must be consistent.** You can set $y = 0$ anywhere. What matters is that you use the *same* reference for both $y_i$ and $y_f$. If you set $y = 0$ at the ground, then an object on the ground has $U = 0$, and an object 5 m up has $U = mg(5) > 0$. Mixing references mid-problem produces garbage.

**2. "Conserved" does not mean "constant everywhere" — it means the total is constant.** Kinetic and potential energy each change throughout the motion. Their *sum* is constant. Many students think conservation of energy means nothing changes, then get confused when speeds change.

**3. Conservation of mechanical energy requires NO non-conservative forces.** Friction, drag, applied forces from engines — these all change the total mechanical energy. If any are present, use the extended form $K_i + U_i + W_\text{nc} = K_f + U_f$.

**4. Springs: $x$ is displacement from natural length, not from some arbitrary point.** When a spring is compressed by $0.10\ \text{m}$, $x = 0.10\ \text{m}$ in $U = \frac{1}{2}kx^2$. If it's then released to its natural length, $U_f = 0$, and all the stored energy becomes kinetic.

**5. Don't confuse height and distance along the ramp.** In $U_g = mgh$, the $h$ is the *vertical* height, not the length of the ramp. A ramp 10 m long at a 30° angle has vertical height $h = 10\sin(30°) = 5\ \text{m}$.

**6. Energy is not momentum.** A big slow truck and a small fast car can have the same kinetic energy but very different momenta. These are different quantities and solve different types of problems — see [[Impulse and Momentum]].

## See Also

- [[Work and Kinetic Energy]] — conservation of energy is built on the work-energy theorem; make sure that entry is solid first
- [[Newton's Second Law]] — force-based analysis is the alternative approach; knowing when each method wins is the key skill
- [[Impulse and Momentum]] — momentum is separately conserved in collisions; energy and momentum conservation together solve problems that neither can solve alone (elastic collisions)
- [[Kinematics - 1D Motion]] — for simple constant-acceleration cases, the kinematic equation $v^2 = v_0^2 + 2a\Delta x$ is equivalent to energy conservation; recognizing this prevents redundant work
- [[Calculus - Derivatives]] — the relationship between force and potential energy: $F = -dU/dx$; this is how you derive the spring force from spring PE
- [[Calculus - Integrals]] — work as an integral is the foundation for defining potential energy; $U = -\int F\, dx$
