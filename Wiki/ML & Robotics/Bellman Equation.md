# Bellman Equation

**One-liner:** A recursive equation stating that the value of a state equals the best immediate reward plus the discounted value of the best next state, turning infinite-horizon planning into a one-step look-ahead.

## Core Idea
$$V^*(s) = \max_a\!\left[R(s,a) + \gamma \sum_{s'} P(s'\mid s,a)\, V^*(s')\right]$$
The optimal value V*(s) is defined self-referentially: the right-hand side contains V*(s') for successor states. This is not circular — the recurrence has a unique fixed point, and dynamic programming exploits it to compute V* iteratively without enumerating all trajectories.

## Why It Exists
Computing the value of a state naively requires summing over infinitely many future trajectories, which is intractable. The Bellman equation collapses this to a single look-ahead step by using the value estimates of successor states as placeholders — exactly what dynamic programming does for sequential problems.

## Real-World Applications
- **[[Markov Decision Process]] solvers:** value iteration is just repeated Bellman backups.
- **[[Q-Learning]]:** the TD update rule is the Bellman equation applied sample-by-sample without a model.
- **Deep Q-Networks (DQN):** the neural network is trained so its outputs satisfy the Bellman equation approximately.
- **Baymax path planning:** at every timestep, the robot selects the motor command that maximises R(s,a) + γ V(s_next), where V was precomputed offline.

## Intuition
Imagine you are at the top of a decision tree. Instead of summing up the infinite branches below you, the Bellman equation says: "Trust your estimate of what each child node is worth, take the best action, and that is your value." If your child-node estimates are accurate, your own estimate is accurate. Starting from terminal states where V = 0 and working backwards, each backup propagates certainty one level up — until the whole tree is correctly valued. The discount factor γ ensures that errors in the far future matter less than errors nearby, which is also why the iteration converges.

## Derivation

**Step 1 — Define cumulative return:**
$$G_t = \sum_{k=0}^{\infty} \gamma^k R_{t+k}$$

**Step 2 — Define optimal value function:**
$$V^*(s) = \max_\pi \mathbb{E}_\pi[G_0 \mid s_0 = s]$$

**Step 3 — Unroll one step using linearity of expectation:**
$$V^*(s) = \max_\pi \mathbb{E}_\pi\!\left[R_0 + \gamma G_1 \mid s_0 = s\right]$$

**Step 4 — Apply the [[Markov Property]] (future depends only on s₁, not history):**
$$V^*(s) = \max_a \mathbb{E}\!\left[R(s,a) + \gamma \max_{\pi'}\mathbb{E}_{\pi'}[G_1 \mid s_1]\right]$$

Since the inner expectation over G₁ is exactly V*(s₁):
$$\boxed{V^*(s) = \max_a \sum_{s'} P(s'\mid s,a)\!\left[R(s,a) + \gamma V^*(s')\right]}$$

**Why this has a unique solution — contraction proof:**

Define the Bellman operator $\mathcal{T}$:
$$(\mathcal{T}V)(s) = \max_a\left[R(s,a) + \gamma \sum_{s'} P(s'|s,a)V(s')\right]$$

For any two value functions V, U:
$$\|\mathcal{T}V - \mathcal{T}U\|_\infty \leq \gamma \|V - U\|_\infty$$

Since γ < 1, T is a γ-contraction in the sup-norm. By the Banach fixed-point theorem, repeated application $V \leftarrow \mathcal{T}V$ converges to the unique fixed point V*.

**Q-value form** (state–action value, used in [[Q-Learning]]):
$$Q^*(s,a) = R(s,a) + \gamma \sum_{s'} P(s'|s,a) \max_{a'} Q^*(s',a')$$

The relationship between them: $V^*(s) = \max_a Q^*(s,a)$.

## Worked Example

```python
import numpy as np

# Two-state MDP with known P and R
# States: {0=safe, 1=goal}
# Actions: {0=wait, 1=move}
# P[s, a, s']
P = np.array([
    [[0.8, 0.2], [0.2, 0.8]],   # from state 0: wait keeps you, move goes to 1
    [[0.0, 1.0], [0.0, 1.0]],   # state 1 is absorbing
])
R = np.array([
    [0.0, -0.1],   # small cost to move
    [1.0,  1.0],   # big reward in goal
])
gamma = 0.95

# Bellman backup by hand for V(0) and V(1)
V = np.zeros(2)
print("Iterating Bellman backups:")
for i in range(30):
    Q = R + gamma * P.dot(V)   # shape (2, 2)
    V_new = Q.max(axis=1)
    print(f"  iter {i+1}: V = {V_new}")
    if np.max(np.abs(V_new - V)) < 1e-8:
        print("Converged.")
        break
    V = V_new

print(f"\nV*(safe) = {V[0]:.4f},  V*(goal) = {V[1]:.4f}")
# V*(safe) ≈ 14.25,  V*(goal) ≈ 20.0
# Optimal policy in state 0 = argmax_a Q[0, a] = action 1 (move)
```

## See Also
- [[Markov Decision Process]] — provides the (S, A, P, R, γ) that the equation is defined over
- [[Q-Learning]] — samples the Bellman equation stochastically without knowing P
- [[Value Function]] — V(s) is the quantity the equation defines
- [[Policy]] — the argmax over Q gives the greedy policy
- [[Markov Property]] — the memorylessness that lets the one-step recursion work
- [[Gradient Descent]] — used in deep RL to minimise Bellman error instead of solving exactly
