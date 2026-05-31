# Work and Kinetic Energy

**One-liner:** Work is the energy transferred to an object by a force acting over a displacement; the work-energy theorem says the net work done on an object equals its change in kinetic energy.

## Why It Exists

[[Newton's Second Law]] is powerful, but it has a weakness: it trades in forces and accelerations, which requires you to track motion *at every instant* in time. For many real problems — a roller coaster, a spring launcher, a pendulum — this is brutal. The path is curved, the forces change direction, and integrating through every instant would take pages.

Energy methods are the shortcut. Instead of asking "what is the force and acceleration at each moment?", you ask "what is the total energy input and output between start and finish?" The path between those two points doesn't matter. You skip directly from initial state to final state.

The work-energy theorem is the bridge that connects the force-based world of Newton's laws to the energy-based world of [[Conservation of Energy]]. Understanding *where it comes from* — rather than memorizing it as a fact — reveals why energy methods work at all.

## The Concept

### Defining Work

Intuitively, "work" means effort. But physics needs a precise definition that is measurable and consistent. Two observations from Newton's laws motivate the formal definition:

1. A force in the direction of motion speeds an object up (increases its energy).
2. A force perpendicular to motion changes direction but does *not* change speed.

This suggests that only the component of force *along the direction of motion* matters for energy transfer. The formal definition of work done by a constant force $\vec{F}$ over a displacement $\vec{d}$ is:

$$W = \vec{F} \cdot \vec{d} = F d \cos\theta$$

where $\theta$ is the angle between the force vector and the displacement vector.

Breaking this down:
- If $\theta = 0°$ (force and motion in same direction): $W = Fd$ — maximum positive work, maximum speedup.
- If $\theta = 90°$ (force perpendicular to motion): $W = 0$ — no work done, speed unchanged. (Example: normal force on a horizontally moving object.)
- If $\theta = 180°$ (force opposite to motion): $W = -Fd$ — negative work, the object slows down.

The unit of work is the **joule**: $1\ \text{J} = 1\ \text{N} \cdot \text{m} = 1\ \text{kg} \cdot \text{m}^2/\text{s}^2$.

### Work by a Variable Force

When force varies with position — as with a spring, for instance — you can't just multiply $F \times d$, because $F$ is different at every point. You have to *sum* the infinitesimal contributions:

$$W = \int_{x_1}^{x_2} F(x)\, dx$$

Geometrically, this is the area under the $F$-vs-$x$ graph. For a spring with $F = kx$ (Hooke's Law), the work done compressing it from $0$ to $x$ is the area of a triangle:

$$W_\text{spring} = \frac{1}{2}kx^2$$

This is why [[Calculus - Integrals]] is not optional in physics — it is literally the tool that extends the work concept to realistic situations.

### Kinetic Energy

Kinetic energy is the energy an object possesses *because* it is moving:

$$K = \frac{1}{2}mv^2$$

The factor of $\frac{1}{2}$ is not arbitrary — it falls out of the derivation below. The $v^2$ dependence means doubling speed quadruples kinetic energy. This is why high-speed collisions are so much more destructive than low-speed ones.

Units: $[K] = \text{kg} \cdot (\text{m/s})^2 = \text{kg} \cdot \text{m}^2/\text{s}^2 = \text{J}$ — same as work. Consistent.

### Deriving the Work-Energy Theorem

This derivation is the key insight. It shows that the $\frac{1}{2}mv^2$ definition and the $F \cdot d$ definition are not two separate postulates — one *causes* the other.

Start with Newton's Second Law in one dimension:

$$F_\text{net} = ma$$

Use the chain rule to rewrite acceleration in terms of velocity and position:

$$a = \frac{dv}{dt} = \frac{dv}{dx}\cdot\frac{dx}{dt} = v\frac{dv}{dx}$$

Substitute:

$$F_\text{net} = mv\frac{dv}{dx}$$

Multiply both sides by $dx$:

$$F_\text{net}\, dx = mv\, dv$$

Integrate both sides — left side from $x_1$ to $x_2$, right side from $v_1$ to $v_2$:

$$\int_{x_1}^{x_2} F_\text{net}\, dx = \int_{v_1}^{v_2} mv\, dv$$

Left side is the definition of net work: $W_\text{net}$.

Right side integrates to:

$$\int_{v_1}^{v_2} mv\, dv = \frac{1}{2}mv_2^2 - \frac{1}{2}mv_1^2 = \Delta K$$

Therefore:

$$\boxed{W_\text{net} = \Delta K = K_f - K_i}$$

This is the **work-energy theorem**. It says: the total work done on an object by all forces equals the change in its kinetic energy. Full stop. Every step followed from $F = ma$ plus the definition of work — there are no new assumptions.

The $\frac{1}{2}$ in $K = \frac{1}{2}mv^2$ appeared automatically from integrating $mv\,dv$. It was never arbitrary.

### What the Theorem Does Not Say

The work-energy theorem tells you about *changes* in kinetic energy — how fast the object is going at two points. It does not tell you about the path taken, how long it took, or where exactly the object is. For those, you still need kinematics or [[Newton's Second Law]] directly.

Also: the work-energy theorem accounts for *all* forces, including friction. When friction does negative work, kinetic energy decreases — the object slows. The energy didn't vanish; it became thermal energy. The theorem still holds; you just need to track where the energy went (see [[Conservation of Energy]]).

### Work Done by Individual Forces

When multiple forces act, you can find the net work two ways:
1. Find $F_\text{net}$ first, then compute $W = F_\text{net} \cdot d$.
2. Find work done by each force separately, then add: $W_\text{net} = W_1 + W_2 + W_3 + \ldots$

Method 2 is usually more useful because you can identify which forces do positive, negative, or zero work:

| Force | Typical case | Work done |
|-------|-------------|-----------|
| Applied force (along motion) | Pushing a box | Positive |
| Friction (kinetic) | Sliding object | Negative (always) |
| Normal force | Horizontal surface | Zero (perpendicular to motion) |
| Gravity | Falling object | Positive (force and motion both downward) |
| Gravity | Rising object | Negative (force down, motion up) |

## Intuition

Imagine two cars with the same engine (same force). One car is a Mini Cooper (light), one is a semi-truck (heavy). The same engine force does the same *work* over the same distance — but the Mini Cooper ends up going much faster, because all that work went into a smaller kinetic energy $\frac{1}{2}mv^2$.

Another way: work is *force times distance*, not force times time. You can hold a heavy box for an hour without doing any work in the physics sense (no displacement). But carrying it up a flight of stairs? You did work equal to $mgh$.

The dot product $F\cos\theta$ is telling you: only the part of the force that is *lined up with the motion* actually accelerates the object. The perpendicular component just pushes sideways against a constraint (like the normal force) and contributes nothing.

## Key Formula / Rule

Work done by a constant force:

$$W = \vec{F} \cdot \vec{d} = Fd\cos\theta$$

Work done by a variable force:

$$W = \int_{x_1}^{x_2} F(x)\, dx$$

Kinetic energy:

$$K = \frac{1}{2}mv^2$$

Work-energy theorem:

$$W_\text{net} = \Delta K = \frac{1}{2}mv_f^2 - \frac{1}{2}mv_i^2$$

## Worked Example

**Problem:** A $3.0\ \text{kg}$ box is pushed horizontally along a floor by a $15\ \text{N}$ force. Kinetic friction acts with $\mu_k = 0.25$. The box starts from rest. After traveling $4.0\ \text{m}$, what is the box's speed?

**Step 1 — Identify all forces and which do work.**

Forces on the box:
- Applied force: $F_\text{app} = 15\ \text{N}$ (horizontal, in direction of motion → does positive work)
- Kinetic friction: $f_k = \mu_k N = \mu_k mg = 0.25 \times 3.0 \times 9.8 = 7.35\ \text{N}$ (opposing motion → does negative work)
- Normal force: $N = mg$ (perpendicular to motion → zero work)
- Weight: $mg$ (downward, perpendicular to horizontal motion → zero work)

**Step 2 — Calculate work done by each force.**

$$W_\text{app} = F_\text{app} \cdot d = 15 \times 4.0 = 60\ \text{J}$$

$$W_\text{friction} = -f_k \cdot d = -7.35 \times 4.0 = -29.4\ \text{J}$$

Negative because friction opposes displacement ($\theta = 180°$).

**Step 3 — Find net work.**

$$W_\text{net} = 60 + (-29.4) = 30.6\ \text{J}$$

**Step 4 — Apply the work-energy theorem.**

$$W_\text{net} = \frac{1}{2}mv_f^2 - \frac{1}{2}mv_i^2$$

$$30.6 = \frac{1}{2}(3.0)v_f^2 - 0$$

$$v_f^2 = \frac{2 \times 30.6}{3.0} = 20.4\ \text{m}^2/\text{s}^2$$

$$v_f = \sqrt{20.4} \approx 4.5\ \text{m/s}$$

**Sanity check:** Using Newton's Second Law:
$F_\text{net} = 15 - 7.35 = 7.65\ \text{N}$, $a = 7.65/3.0 = 2.55\ \text{m/s}^2$.
From kinematics: $v^2 = 0 + 2(2.55)(4.0) = 20.4\ \text{m}^2/\text{s}^2$. Matches exactly.

**Answer:** The box is moving at approximately $4.5\ \text{m/s}$.

## Gotchas

**1. Work is a scalar, not a vector.** It can be positive, negative, or zero, but it has no direction. When adding works from multiple forces, you add them as regular numbers, not as vectors.

**2. The perpendicular-force trap.** Many students compute work for forces that do zero work (normal force, centripetal force in circular motion). Always check the angle first. If $\theta = 90°$, $W = 0$, and the force has no effect on kinetic energy.

**3. Negative work means energy was removed, not that the math is wrong.** Friction always does negative work on a sliding object. This is the correct result — the object loses kinetic energy to heat. Don't second-guess a negative work answer.

**4. Work-energy theorem requires net work, not just one force's work.** If the problem has friction, gravity, and an applied force, you must include all three. Students who only compute applied force work and call it $W_\text{net}$ get wrong answers every time.

**5. The theorem gives speed, not velocity.** Since $K = \frac{1}{2}mv^2$ uses $v^2$, you get the magnitude. Use other information (direction of motion, context) to assign direction if needed.

**6. Displacement vs. path length in 2D.** For a constant force like gravity, $W = mgh$ uses the vertical displacement, not the total path length along a ramp. The work done by gravity depends only on how much height changes, regardless of the path. This is a preview of [[Conservation of Energy]] — it works because gravity is a *conservative force*.

## See Also

- [[Kinematics - 1D Motion]] — the kinematic equation $v^2 = v_0^2 + 2a\Delta x$ is a restatement of the work-energy theorem for constant force
- [[Newton's Second Law]] — the work-energy theorem is derived directly from $F = ma$; Newton's laws and energy methods are two views of the same physics
- [[Conservation of Energy]] — when friction is absent, the work done by gravity equals the change in kinetic energy, which leads to potential energy and energy conservation
- [[Impulse and Momentum]] — work integrates force over distance; impulse integrates force over time; they're parallel concepts
- [[Calculus - Integrals]] — work by a variable force is a definite integral; the work-energy derivation uses integration by substitution
- [[Dot Product]] — $W = \vec{F} \cdot \vec{d}$ is the geometric dot product; understanding this in Math prevents the angle-$\theta$ mistakes
