# Conservative Force

**One-liner:** A conservative force is one where the work done between two points depends only on those endpoints, never on the path taken — which is exactly what makes potential energy possible.

## Core Idea
$$W_{A \to B} = -\Delta U = U_A - U_B \quad \text{(path-independent)}$$
For a conservative force, if you move an object from point A to point B by any route — a straight line, a spiral, a figure-eight — the work done is identical. Equivalently, if you return the object to its starting point, the net work done by the force is zero for any closed loop. This path-independence is the definition of "conservative."

## Why It Exists
The concept exists to explain when potential energy can be defined at all. Potential energy $U$ is stored work — the idea that energy "waiting" at a location can be recovered later. For this to make sense, the work done by the force must not depend on how you got there. If work depended on path, you could engineer a perpetual motion machine (take the low-work path out, the high-work path back, and extract free energy). Nature forbids this: only path-independent forces allow a well-defined potential energy function.

## Real-World Applications
- **Gravity:** lifting a 10 kg box 2 m costs exactly $mgh = 196\text{ J}$ of work regardless of whether you carry it straight up, up a ramp, or up a spiral staircase. This lets structural engineers calculate load energy without tracking exact paths.
- **Springs:** the elastic restoring force $F = -kx$ is conservative — the energy stored compressing a spring is fully recoverable. Used in shock absorbers, watch mechanisms, and elastic potential energy problems.
- **Electric force:** the Coulomb force between charges is conservative (potential energy $U = kq_1q_2/r$), underpinning all of circuit theory — voltage is literally electric potential energy per charge.
- **Friction (counter-example):** friction is non-conservative. Drag a box across a rough table in a 2 m straight line vs. a 4 m detour — friction does different (more negative) work on the longer path. No potential energy for friction; its work permanently heats the surface.

## Intuition
Picture a hilly landscape. Gravity is conservative: if you carry a backpack from valley to mountaintop, the work done against gravity depends only on the height difference — not whether you took the steep trail or the winding road. When you come back down, gravity gives all that work back. The landscape "stores" the energy as gravitational potential energy.

Friction is the anti-example. Slide a box across carpet in a circle and return it to start: the box is back where it began, but friction has done net negative work the whole way. That energy is gone as heat — you cannot recover it mechanically. No potential energy exists for friction because there is no "landscape" to store it in.

**One-line test:** If $\oint \vec{F} \cdot d\vec{r} = 0$ for every closed path, the force is conservative.

## Derivation
A force $\vec{F}$ is conservative if and only if its work integral is path-independent:
$$W = \int_A^B \vec{F} \cdot d\vec{r} \quad \text{same for all paths from A to B}$$

**Equivalent condition — zero curl:** By Stokes' theorem, path-independence is equivalent to $\nabla \times \vec{F} = 0$ everywhere. For gravity: $\vec{F} = -mg\hat{j}$, so $\nabla \times \vec{F} = 0$ trivially. For friction: the friction force always opposes velocity — its direction changes with the path, so its curl is non-zero.

**Defining potential energy from a conservative force:**

If $\vec{F}$ is conservative, define $U$ by:
$$W_{A \to B} = -\Delta U = -(U_B - U_A)$$

Or equivalently, $U(x)$ is the function such that:
$$F_x = -\frac{dU}{dx} \quad \text{(in 1D)}$$
$$\vec{F} = -\nabla U \quad \text{(in 3D)}$$

**Check for gravity** ($F = -mg$ downward, taking up as positive $y$):
$$F_y = -\frac{d(mgy)}{dy} = -mg \checkmark$$

**Check for spring** ($F = -kx$):
$$F_x = -\frac{d(\frac{1}{2}kx^2)}{dx} = -kx \checkmark$$

## Worked Example
A 3 kg object moves from point A at height $h = 5\text{ m}$ to point B at height $h = 0$ (ground level). Compare the work done by gravity along two paths: (1) straight vertical drop; (2) a curved ramp of length 12 m inclined at $\theta$ that ends at the same point.

**Path 1 — Straight drop (5 m):**
$$W_{\text{gravity}} = F_y \cdot \Delta y = mg \cdot h = (3)(9.8)(5) = 147\text{ J}$$

**Path 2 — Curved ramp:**
The ramp changes direction continuously, but gravity is conservative. The work depends only on vertical displacement, which is still $\Delta y = 5\text{ m}$ downward:
$$W_{\text{gravity}} = mg \cdot h = (3)(9.8)(5) = 147\text{ J}$$

**Both paths give 147 J.** This is path-independence in action.

**Now add friction to Path 2** (coefficient $\mu_k = 0.2$, normal force $N = mg\cos\theta$):
$$W_{\text{friction}} = -\mu_k N \cdot L = -0.2 \cdot (3)(9.8)\cos\theta \cdot 12 \neq 147\text{ J}$$
Friction's work depends on the path length $L$ — it is non-conservative.

## See Also
- [[Conservation of Energy]] — requires conservative forces for the $K + U = \text{const}$ form
- [[Gravitational Potential Energy]] — defined because gravity is conservative
- [[Elastic Potential Energy]] — defined because spring force is conservative
- [[Work]] — the integral that defines whether a force is conservative
- [[Work-Energy Theorem]] — separates into conservative and non-conservative work
- [[Newton's Second Law]] — the force law each force obeys; conservativeness is an additional property
- [[Gradient]] — $\vec{F} = -\nabla U$ links conservative forces to vector calculus (Math connection)
