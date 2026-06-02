# Work

**One-liner:** Work is the transfer of energy from a force to an object — it equals the force component along the direction of motion, multiplied by the displacement.

## Core Idea
$$W = \vec{F} \cdot \vec{d} = Fd\cos\theta = \int \vec{F} \cdot d\vec{r}$$
Work is a scalar measured in joules (J = N·m). The dot product means only the component of force *along the direction of motion* does work. A perpendicular force (like the normal force on a horizontal surface) does zero work. Work can be positive (force aids motion), negative (force opposes motion), or zero (force perpendicular to motion).

## Why It Exists
[[Newton's Second Law]] predicts acceleration from force, but it doesn't directly answer questions about energy. Work is the bridge: it's how forces transfer energy into (or out of) an object's motion. Without work, we couldn't derive [[Kinetic Energy]] or build the [[Work-Energy Theorem]], and energy methods in physics would be inaccessible.

## Real-World Applications
- **Weightlifting:** lifting a 100 kg barbell 2 m overhead does $W = mgh = 100(9.8)(2) = 1960\text{ J}$ of work against gravity — stored as [[Gravitational Potential Energy]].
- **Electric motors:** the electrical work input to a motor equals (ideally) the mechanical work output on the load.
- **Robotics:** the power consumed by a robot arm joint is the work done per unit time — critical for battery life estimation on mobile platforms like Baymax.
- **Compression:** a piston compressing gas does work on the gas ($W = \int P\, dV$) — this is the same formula with force replaced by pressure and displacement replaced by volume change.
- **Braking:** brakes do negative work on a car — friction force opposes motion, removing kinetic energy and converting it to heat.

## Intuition
Imagine pushing a heavy box. If you push horizontally and it moves horizontally, you're doing positive work — all your effort goes into the motion. If you push sideways (perpendicular to its motion), you're doing zero work on the box's motion — all your force goes to bending it, not moving it. If you're trying to hold it back (like a brake), you're doing negative work — removing energy from its motion.

## Derivation
**Constant force, straight path:**
Work is defined as:
$$W = \vec{F} \cdot \vec{d} = |\vec{F}||\vec{d}|\cos\theta$$
where $\theta$ is the angle between force and displacement.

**Variable force or curved path (calculus form):**
Divide the path into infinitesimal segments $d\vec{r}$. The work done over each tiny segment is $dW = \vec{F} \cdot d\vec{r}$. Sum (integrate) over the whole path:
$$W = \int_{\vec{r}_i}^{\vec{r}_f} \vec{F} \cdot d\vec{r}$$
For a spring (force varies with position, $F = -kx$):
$$W_{\text{spring}} = \int_0^x (-kx')\, dx' = -\tfrac{1}{2}kx^2$$
(The spring does negative work on whatever compressed it, removing kinetic energy. The object does $+\tfrac{1}{2}kx^2$ of work on the spring.)

## Worked Example
A person pushes a 20 kg box with a 60 N force at 30° below horizontal, moving it 5 m across a flat floor.

**Step 1 — Identify the component of force along displacement:**
$$F_x = F\cos\theta = 60\cos(30°) = 60 \times 0.866 = 52.0\text{ N (horizontal)}$$

**Step 2 — Calculate work done by the applied force:**
$$W_{\text{applied}} = F_x \cdot d = 52.0 \times 5 = 260\text{ J}$$

**Step 3 — Note what does zero work:** The normal force (vertical, perpendicular to horizontal motion) does $W = 0$. Gravity does $W = 0$ (same reason).

**Step 4 — If friction is 20 N opposing motion, work done by friction:**
$$W_{\text{friction}} = (20\text{ N})(\cos 180°)(5\text{ m}) = -100\text{ J}$$
(Negative because force and displacement are antiparallel.)

**Step 5 — Net work:**
$$W_{\text{net}} = 260 + 0 + 0 + (-100) = 160\text{ J}$$
By the [[Work-Energy Theorem]], the box gains 160 J of kinetic energy.

## See Also
- [[Work-Energy Theorem]] — net work equals change in kinetic energy
- [[Kinetic Energy]] — work done on an object appears as kinetic energy
- [[Force]] — work is force times displacement component
- [[Gravitational Potential Energy]] — work done against gravity becomes potential energy
- [[Conservative Force]] — a force where the work done is path-independent
- [[Integral]] — the general formula for work requires integration
- [[Dot Product]] — $W = \vec{F}\cdot\vec{d}$; work is a dot product of two vectors, the same operation used in neural network weighted sums
- [[Electric Power]] — power is the rate of work: $P = dW/dt$; this is exactly how electrical power ($P = IV$) relates to energy delivered by a circuit over time
