# Geometric Series

**One-liner:** A series where each term is a constant multiple (ratio r) of the previous one — the most important series with a closed-form sum.

## Core Idea
$$\sum_{n=0}^{\infty} ar^n = \frac{a}{1-r}, \quad |r| < 1$$
The series $a + ar + ar^2 + ar^3 + \cdots$ converges to $a/(1-r)$ if and only if $|r| < 1$. If $|r| \geq 1$, the series diverges. This is the only infinite series most students will ever evaluate in closed form without special tricks.

## Why It Exists
Geometric series arise whenever a quantity is repeatedly scaled by a fixed factor — compound interest, signal reflections, radioactive decay chains, probability trees. The closed-form sum is extraordinarily useful: it converts an infinite process into a single fraction. Without this result, computing the present value of an annuity, the total distance traveled in an infinite-bounce scenario, or the sum of a signal's reflections would require numerical approximation.

## Real-World Applications
- **Finance — compound interest and perpetuities:** The present value of an annuity paying $P per period at rate $r$ is $PV = P/(r) — a geometric series. Every mortgage amortization calculation uses this. Infinite payment streams (perpetuities) have finite present value precisely because the geometric series converges.
- **Signal attenuation / dB:** In transmission lines and fiber optics, signal power drops by factor $r$ per segment. Total power in an infinite chain of reflections is $P_0/(1-r)$ — a geometric series.
- **Zeno's paradox:** The runner covers 1/2 the distance, then 1/4, then 1/8... Total = $\sum 1/2^n = 1$. The paradox is resolved: infinitely many steps can sum to a finite distance in finite time.
- **Computer graphics — ray tracing:** Reflections of reflections: a ray can bounce infinitely between mirrors. The geometric series gives the total light contribution assuming intensity drops by factor $r$ each bounce. Used in global illumination algorithms.
- **Robotics — convergence guarantees:** Repeated application of a contraction mapping (used in iterative inverse kinematics solvers) converges via geometric series bounds on error.
- **Economics:** Keynesian multiplier effect — an initial government spending $G$ circulates through the economy, with each round contributing $cG, c^2G, \ldots$ (c = marginal propensity to consume). Total = $G/(1-c)$.

## Intuition
Fold a piece of paper in half repeatedly. Each fold halves the thickness: $1 + 1/2 + 1/4 + \cdots$. After infinitely many folds, the total accumulated thickness is exactly 2 (the original). The reason: each remaining gap is exactly the same size as what you've already accumulated, so each step closes half the remaining distance. You converge on 2.

For $|r| > 1$: each term is larger than the last. There's no chance of the sum stabilizing.

For $|r| = 1$: either the terms are constant (diverges) or oscillate and never cancel perfectly.

## Derivation
**Closed-form partial sum:** Let $S_N = a + ar + ar^2 + \cdots + ar^N$.

Multiply both sides by $r$:
$$r S_N = ar + ar^2 + \cdots + ar^{N+1}$$

Subtract:
$$S_N - r S_N = a - ar^{N+1}$$
$$S_N(1 - r) = a(1 - r^{N+1})$$
$$S_N = a \cdot \frac{1 - r^{N+1}}{1 - r}, \quad r \neq 1$$

**Take the limit:** If $|r| < 1$, then $r^{N+1} \to 0$ as $N \to \infty$:
$$\sum_{n=0}^\infty ar^n = \lim_{N\to\infty} S_N = \frac{a}{1-r}$$

If $|r| > 1$: $|r|^{N+1} \to \infty$, so $S_N$ diverges.
If $r = 1$: $S_N = (N+1)a \to \infty$, diverges.
If $r = -1$: $S_N$ alternates between $a$ and $0$, so the limit doesn't exist — diverges.

## Worked Example
**Problem:** An infinite rubber ball is dropped from 1 meter and bounces to 3/4 of its previous height each time. Find the total vertical distance traveled.

**Step 1 — Model the bounces.** The ball falls 1 m, rises 3/4, falls 3/4, rises $(3/4)^2$, ...

**Step 2 — Sum.** Total distance = 1 (first fall) + $2 \cdot \frac{3/4}{1 - 3/4}$ (all subsequent up+down):
$$D = 1 + 2\sum_{n=1}^\infty \left(\frac{3}{4}\right)^n = 1 + 2 \cdot \frac{3/4}{1 - 3/4} = 1 + 2 \cdot 3 = 7 \text{ meters}$$

**The infinite bounces cover exactly 7 meters.**

## See Also
- [[Series]] — general theory of infinite sums
- [[Partial Sum]] — the closed-form $S_N$ formula derived here
- [[Convergence]] — why $|r| < 1$ is the key condition
- [[Ratio Test]] — generalizes the ratio $|r|$ idea to arbitrary series
- [[Power Series]] — geometric series is the prototype power series
- [[Maclaurin Series]] — $1/(1-x) = \sum x^n$ is a Maclaurin series
- [[p-Series]] — another canonical series; compare convergence conditions
- [[Value Function]] — the discounted return Σγᵗr in reinforcement learning is a geometric series with ratio γ (discount factor); convergence requires γ < 1
- [[Reward]] — cumulative reward over an infinite horizon is defined as a geometric series sum, giving it a finite value only when |γ| < 1
- [[Conservation of Energy]] — energy loss in damped systems (each bounce or cycle losing a fixed fraction) sums as a geometric series to a finite total
