# Q-Learning

**One-liner:** A model-free, off-policy reinforcement learning algorithm that learns the optimal action-value function Q*(s,a) directly from experience, without ever needing to know the environment's transition probabilities.

## Core Idea
$$Q(s,a) \;\leftarrow\; Q(s,a) + \alpha\!\left[\underbrace{r + \gamma \max_{a'} Q(s',a')}_{\text{TD target}} - Q(s,a)\right]$$
At each timestep the agent takes action a in state s, receives reward r, lands in s', then nudges its estimate Q(s,a) toward the **TD target** — a bootstrap estimate of what the true Q should be. α is the learning rate. The max over a' makes this **off-policy**: the update assumes optimal future behaviour regardless of the actual policy used to collect data.

## Why It Exists
Solving an MDP with value iteration requires knowing P(s'|s,a) and R(s,a) for the entire environment — a luxury rarely available for real robots or games. Q-learning bypasses this by substituting the unknown expectation $\sum_{s'} P(s'|s,a)[\ldots]$ with a single sampled transition (s, a, r, s'). Over millions of samples the empirical average converges to the true expectation.

## Real-World Applications
- **Atari games (DQN, 2013):** DeepMind replaced the Q-table with a neural network — the first time deep learning + RL matched human performance on raw pixels.
- **Robot manipulation:** Q-learning used to learn pick-and-place policies in simulation before transferring to the real arm.
- **Baymax obstacle avoidance:** tabular Q-learning is a natural first pass; states = discretised laser scan, actions = {forward, left, right, stop}.
- **Network routing:** packets are actions, delay is negative reward, Q-learning finds optimal routing policies online.

## Intuition
Think of Q(s,a) as a scorecard: "if I am in state s and take action a, how much total future reward do I expect?" Initially all scores are wrong. Every time we experience a real transition, we compare what we thought Q(s,a) was with what the data says it should be (r + γ max Q(s',·)), and adjust our estimate a little in that direction. Eventually every (s,a) pair has been visited enough times that the scorecard reflects reality. The key insight: we do not need to know the rules of the world — just live in it and keep notes.

## Derivation

**Start from the [[Bellman Equation]] for Q*:**
$$Q^*(s,a) = \mathbb{E}_{s'}\!\left[R(s,a) + \gamma \max_{a'} Q^*(s',a')\right]$$

The expectation is over s' ~ P(·|s,a). We cannot compute it without knowing P. **Stochastic approximation** (Robbins–Monro, 1951) says: if we have noisy samples of a target f(θ) and we update

$$\theta \leftarrow \theta + \alpha\left[f(\theta) - \theta\right]$$

with α satisfying $\sum \alpha_t = \infty$, $\sum \alpha_t^2 < \infty$, then θ converges to the fixed point of f. Applying this to Q:

- Target: $y_t = r_t + \gamma \max_{a'} Q(s'_t, a')$
- Update: $Q(s_t, a_t) \leftarrow Q(s_t, a_t) + \alpha_t [y_t - Q(s_t, a_t)]$

**Convergence theorem (Watkins & Dayan, 1992):** Q-learning converges to Q* with probability 1 if:
1. Every (s,a) pair is visited infinitely often.
2. The learning rates satisfy the Robbins–Monro conditions.
3. Rewards are bounded.

**ε-greedy exploration** ensures condition 1: with probability ε take a random action; otherwise take argmax_a Q(s,a). ε is decayed over time.

**From tabular to Deep Q-Networks (DQN):**

When the state space is huge (e.g., 84×84 pixel images), a Q-table is impossible. DQN replaces Q(s,a) with a neural network Q(s,a;θ). The loss:
$$\mathcal{L}(\theta) = \mathbb{E}\!\left[\left(r + \gamma \max_{a'} Q(s', a';\, \theta^-) - Q(s, a;\, \theta)\right)^2\right]$$

Two stabilisation tricks:
- **Experience replay:** store transitions in a buffer, sample random mini-batches to break correlations.
- **Target network θ⁻:** a frozen copy of θ, updated every N steps, so the target y is not a moving target.

The network is trained with [[Gradient Descent]] (specifically Adam) to minimise the Bellman error.

## Worked Example

```python
import numpy as np
import random

# Frozen Lake-style: 4 states, 2 actions
# States: {0=start, 1=hole, 2=safe, 3=goal}
# Actions: {0=left, 1=right}

def step(s, a):
    """Deterministic env for clarity."""
    transitions = {
        (0, 0): (0, -0.01), (0, 1): (2, -0.01),
        (1, 0): (1,  0.00), (1, 1): (1,  0.00),  # absorbing hole
        (2, 0): (0, -0.01), (2, 1): (3,  1.00),
        (3, 0): (3,  0.00), (3, 1): (3,  0.00),  # absorbing goal
    }
    return transitions[(s, a)]

Q = np.zeros((4, 2))
alpha = 0.1
gamma = 0.95
epsilon = 1.0

for episode in range(2000):
    s = 0
    for _ in range(50):  # max steps per episode
        # ε-greedy action selection
        if random.random() < epsilon:
            a = random.randint(0, 1)
        else:
            a = np.argmax(Q[s])

        s_next, r = step(s, a)

        # Q-learning update
        td_target = r + gamma * np.max(Q[s_next])
        Q[s, a] += alpha * (td_target - Q[s, a])

        s = s_next
        if s in (1, 3):   # terminal
            break

    epsilon = max(0.01, epsilon * 0.995)  # decay exploration

print("Learned Q-table:")
print(Q.round(3))
print("Optimal policy:", ["left" if np.argmax(Q[s]) == 0 else "right" for s in range(4)])
# Expected: [right, -, right, -]  (left/right ignored in absorbing states)
```

## See Also
- [[Bellman Equation]] — Q-learning is a sampled, online version of the Bellman backup
- [[Markov Decision Process]] — the formal framework Q-learning operates within
- [[Value Function]] — Q is the state-action value function; V(s) = max_a Q(s,a)
- [[Policy]] — the ε-greedy policy drives exploration; the greedy policy argmax Q is the output
- [[Reward]] — the r signal that drives every update
- [[Loss Function]] — the Bellman error that DQN minimises
- [[Gradient Descent]] — used in DQN to update the neural network weights
- [[Neural Network]] — the function approximator in DQN
- [[Backpropagation]] — how DQN updates neural network weights via the Bellman error loss
