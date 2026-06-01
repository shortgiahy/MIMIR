# Markov Decision Process

**One-liner:** A mathematical framework that formalizes sequential decision-making as a 5-tuple (S, A, P, R, γ), giving every modern reinforcement learning algorithm its foundation.

## Core Idea
$$\text{MDP} = (S,\, A,\, P,\, R,\, \gamma)$$
An MDP has a set of **states** S, a set of **actions** A, a **transition function** P(s'|s,a) giving the probability of landing in s' after taking action a in state s, a **reward function** R(s,a) (immediate signal), and a **discount factor** γ ∈ [0,1) that down-weights future rewards. The agent's goal is to find a **policy** π: S → A that maximises expected cumulative discounted reward.

## Why It Exists
Purely greedy decision-making (pick the action with the biggest immediate reward) fails whenever actions have delayed consequences — e.g., a robot arm must first move backward before it can reach a target. The MDP framework forces us to reason over entire trajectories and balance short-term vs. long-term payoff, giving a precise object to optimise.

## Real-World Applications
- **Game AI (AlphaGo/AlphaZero):** board = state, legal moves = actions, win/loss = reward.
- **Robot navigation (Baymax):** robot pose = state, motor commands = actions, reward = −distance to goal; solved MDP gives the navigation policy.
- **Recommendation systems:** user context = state, item to show = action, click = reward.
- **ChatGPT RLHF:** token sequences treated as states; model outputs (actions) are scored by a reward model trained from human feedback.

## Intuition
Think of the MDP as a directed weighted graph where nodes are (state, action) pairs and edges carry probabilities (where you land) and numbers (how much reward you get). The agent does a random walk on this graph — but it chooses which edge to take. The MDP says: "here are all the rules of the world." The agent's job is to find the walk that accumulates the most reward on average. The discount factor γ is just a mathematical device that makes the infinite-horizon sum finite and encodes the preference for sooner rewards.

## Derivation

**Value function under policy π:**
$$V^\pi(s) = \mathbb{E}_\pi\!\left[\sum_{t=0}^{\infty} \gamma^t R(s_t, a_t) \;\Big|\; s_0 = s\right]$$

Unrolling one step:
$$V^\pi(s) = \mathbb{E}_\pi\!\left[R(s_0, a_0) + \gamma \sum_{t=1}^{\infty} \gamma^{t-1} R(s_t, a_t)\right]$$

Because future rewards starting from s' look identical in structure to the full problem (the [[Markov Property]]):
$$V^\pi(s) = \sum_a \pi(a|s)\left[R(s,a) + \gamma \sum_{s'} P(s'|s,a)\, V^\pi(s')\right] \quad \text{(Bellman Expectation)}$$

For the **optimal** value function V* (Bellman Optimality):
$$V^*(s) = \max_a\left[R(s,a) + \gamma \sum_{s'} P(s'|s,a)\, V^*(s')\right]$$

**Value Iteration algorithm** (solves the MDP when P and R are known):
```
Initialise V(s) = 0 for all s
Repeat until convergence:
  For each s in S:
    V(s) ← max_a [ R(s,a) + γ Σ_{s'} P(s'|s,a) V(s') ]
Extract policy: π(s) = argmax_a [ R(s,a) + γ Σ_{s'} P(s'|s,a) V(s') ]
```
Convergence is guaranteed because the Bellman operator is a contraction mapping (factor γ < 1) in the sup-norm, so repeated application converges to the unique fixed point V*.

## Worked Example

```python
import numpy as np

# Tiny 3-state MDP: states {0,1,2}, actions {0,1}
# P[s, a, s'] = probability of transitioning to s' from s via a
P = np.zeros((3, 2, 3))
P[0, 0, 1] = 1.0   # state 0, action 0 → state 1
P[0, 1, 2] = 1.0   # state 0, action 1 → state 2
P[1, 0, 2] = 1.0
P[1, 1, 0] = 1.0
P[2, 0, 2] = 1.0   # absorbing
P[2, 1, 2] = 1.0

R = np.array([
    [0.0, 0.0],   # R(0, ·)
    [1.0, 0.0],   # R(1, ·)
    [5.0, 5.0],   # R(2, ·)
])

gamma = 0.9

# Value Iteration
V = np.zeros(3)
for _ in range(1000):
    V_new = np.max(R + gamma * P.dot(V), axis=1)
    if np.max(np.abs(V_new - V)) < 1e-9:
        break
    V = V_new

policy = np.argmax(R + gamma * P.dot(V), axis=1)
print("V* =", V)       # V* = [45. 45.  50.]  (approximate)
print("π* =", policy)  # π* = [1, 0, 0] → go to state 2 immediately
```

## See Also
- [[Bellman Equation]] — the recursive equation that value iteration solves
- [[Q-Learning]] — model-free algorithm when P and R are unknown
- [[Policy]] — the object an MDP solver outputs
- [[Value Function]] — V(s) is the central quantity in MDP theory
- [[Markov Property]] — the memorylessness assumption the MDP relies on
- [[Reward]] — defines what "good" means in the MDP
- [[State]] — formalises what the agent observes
- [[Action]] — formalises what the agent controls
- [[Gradient Descent]] — used in deep RL to optimise policies end-to-end
