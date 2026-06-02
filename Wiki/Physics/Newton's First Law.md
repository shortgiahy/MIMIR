# Newton's First Law

**One-liner:** An object stays at rest or moves in a straight line at constant speed unless a net external force acts on it — this is the law of inertia.

## Core Idea
$$\vec{F}_{\text{net}} = 0 \implies \vec{a} = 0 \implies \vec{v} = \text{constant}$$
"Constant" includes zero velocity (rest). There is no need to apply a force to *maintain* motion — only to *change* it. This was radical: before Newton, Aristotelian physics required a force to keep things moving.

## Why It Exists
The First Law defines what we mean by an **inertial reference frame** — one in which [[Newton's Second Law]] holds in its standard form. It also establishes inertia as the tendency of matter to resist changes in velocity. Without it, we couldn't determine when $F = ma$ is valid (it fails in accelerating reference frames like a carousel).

## Real-World Applications
- **Seat belts:** your body tends to keep moving forward when the car decelerates suddenly. The seat belt provides the net force needed to decelerate your body with the car — without it, inertia carries you into the windshield.
- **Puck on ice:** a hockey puck glides with nearly constant velocity because friction is low — a near-ideal demonstration of the First Law.
- **Satellite orbits:** a satellite in deep space (no air drag) would travel in a straight line forever without gravity. Gravity is the net force that curves that straight path into an orbit.
- **Robotics (Baymax):** robot arms must account for inertia during rapid joint movements — heavy links resist velocity changes and require larger forces (torques) to start and stop.
- **Spacecraft attitude control:** in the vacuum of space, thrusters must fire briefly to change orientation; there is no drag to stop the rotation, so a second opposing burst is needed.

## Intuition
Imagine pushing a box on a frictionless surface and then removing your hand. The box keeps moving at the same speed forever — not because something is pushing it, but because nothing is *stopping* it. Motion is the natural state; forces are needed only to *change* that state. Friction is why this isn't obvious in everyday life — it hides the First Law by constantly decelerating things.

## Derivation
The First Law is a special case of [[Newton's Second Law]] (or, historically, its logical predecessor):
$$\vec{F}_{\text{net}} = m\vec{a} \implies \text{if } \vec{F}_{\text{net}} = 0, \text{ then } \vec{a} = 0$$
Zero acceleration means $\frac{d\vec{v}}{dt} = 0$, which means $\vec{v}$ is constant (could be any constant vector, including zero).

**Non-inertial frames:** If you observe from an accelerating frame (an elevator accelerating upward, a rotating carousel), objects appear to accelerate even with no external force. "Fictitious forces" (like the centrifugal force) appear to explain this. The First Law tells you that such frames are non-inertial — the law only holds when the observer is not accelerating.

## Worked Example
A 5 kg book sits on a table.

**Step 1 — Identify forces:** Gravity pulls down with $F_g = mg = 49\text{ N}$. The table pushes up with normal force $N$.

**Step 2 — Apply First Law:** The book is at rest ($v = 0 = \text{const}$), so $\vec{F}_{\text{net}} = 0$.

**Step 3 — Solve for N:**
$$N - F_g = 0 \implies N = 49\text{ N}$$

**Step 4 — Key insight:** This is not the action-reaction pair from [[Newton's Third Law]]. The book pushes down on the table, and the table pushes up on the book — that *is* a third-law pair. The normal force equals gravity here because of the First Law (zero net force on a stationary object), not because of the Third Law.

## See Also
- [[Newton's Second Law]] — the general law; First Law is the special case $F_{\text{net}} = 0$
- [[Net Force]] — the vector sum that must be zero for the First Law to hold
- [[Inertia]] — the property described by the First Law; resistance to velocity change
- [[Newton's Third Law]] — explains force pairing; distinct from why $N = mg$ in the above example
- [[Acceleration]] — zero net force means zero acceleration
