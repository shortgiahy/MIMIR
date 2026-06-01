# Value Function

**One-liner:** V^π(s) — the expected total discounted reward the agent will accumulate from state s onward, following policy π.

## Core Idea
$$V^\pi(s) = \mathbb{E}_\pi\left[\sum_{t=0}^{\infty} \gamma^t r_{t} \;\middle|\; s_0 = s\right]$$
The value function answers: "How good is it to be in state s when following policy π?" A high V^π(s) means the agent can expect a lot of reward from here; a low value means the future looks bleak. The value function is learned (not given), and it transforms the hard problem of credit assignment into a local update rule — the Bellman equation.

## Why It Exists
The [[Reward]] signal r_t is immediate and local — it only reflects the quality of the last action. The value function aggregates over the entire future, making it a much richer signal for deciding what to do. An action that earns −1 now but leads to +100 later is obviously good, but the agent can't know this from r_t alone. V^π(s) encodes this long-horizon reasoning. With a good value function, the agent can evaluate actions by where they lead rather than what they immediately earn.

## Real-World Applications
- **AlphaGo:** A value network V(s) predicts win probability from any board position. During MCTS, it evaluates leaf nodes without playing out full games — enabling superhuman play at reasonable computational cost.
- **PPO (robot locomotion):** An actor-critic architecture where the critic learns V^π(s) and computes advantages A(s,a) = r + γV(s') − V(s). These advantages guide the policy update.
- **RLHF (ChatGPT):** PPO uses a value model (separate from the language model) to estimate the expected reward from the current partial response. It's trained alongside the policy.
- **Baymax navigation:** A value function over (position, heading, goal) learned during training in Isaac Sim. At deployment it's discarded — only the [[Policy]] runs on hardware — but it's critical for training quality.
- **Q-learning (DQN, Atari):** Uses Q^π(s,a) — the action-value function — to derive a policy. The entire DQN paper is about learning Q* well enough to play Atari with superhuman skill.

## Intuition
The value function is the agent's "crystal ball" — it looks into the future and summarizes what it sees as a single number. A chess program using only material count (piece values) has a shallow value function; one that also considers positional control, king safety, and endgame patterns has a deeper one.

**V^π vs Q^π — two flavors:**

$$Q^\pi(s, a) = \mathbb{E}_\pi\left[\sum_{t=0}^{\infty} \gamma^t r_{t} \;\middle|\; s_0=s, a_0=a\right]$$

Q^π(s,a) is the value of taking action a in state s and then following π. The relationship:
$$V^\pi(s) = \sum_a \pi(a|s) Q^\pi(s,a) = \mathbb{E}_{a \sim \pi}[Q^\pi(s,a)]$$

V is the average over actions (weighted by policy); Q is conditioned on a specific first action.

**Why V and not Q for actor-critic?** For continuous action spaces, you'd need to maximize Q over all actions (hard for continuous A). Instead, actor-critic uses V to compute the advantage:
$$A^\pi(s,a) = Q^\pi(s,a) - V^\pi(s)$$
Advantage is positive if action a is better than the average; negative if worse. It's the "relative quality" of an action.

## Derivation
**Bellman equation** — the recursive definition of V^π:
$$V^\pi(s) = \sum_a \pi(a|s) \sum_{s'} P(s'|s,a)\left[R(s,a,s') + \gamma V^\pi(s')\right]$$

In matrix form: **V^π = R^π + γP^π V^π**, giving the closed-form solution:
$$V^\pi = (I - \gamma P^\pi)^{-1} R^\pi$$

This is exact but requires inverting an |S|×|S| matrix — infeasible for large state spaces.

**Temporal Difference (TD) learning** approximates this with online updates:
$$V(s_t) \leftarrow V(s_t) + \alpha \underbrace{\left[r_t + \gamma V(s_{t+1}) - V(s_t)\right]}_{\text{TD error } \delta_t}$$

The TD error δ_t = r_t + γV(s_{t+1}) − V(s_t) is:
- Positive: V(s_t) underestimates the value — move it up
- Negative: V(s_t) overestimates — move it down
- Zero: V is consistent with the Bellman equation — converged

**Bellman optimality equation** for the optimal value V*:
$$V^*(s) = \max_a \sum_{s'} P(s'|s,a)\left[R(s,a,s') + \gamma V^*(s')\right]$$

Once V* is known, the optimal policy is:
$$\pi^*(s) = \arg\max_a \sum_{s'} P(s'|s,a)\left[R(s,a,s') + \gamma V^*(s')\right]$$

**Value function approximation** with neural network V_φ(s):
Loss: L(φ) = E[(r_t + γV_φ(s_{t+1}) − V_φ(s_t))²]
Gradient: ∇_φ L = −(δ_t)·∇_φ V_φ(s_t)   (stop gradient through target)

## Worked Example
```python
import numpy as np

# ---- Tabular value function: exact Bellman iteration ----
# Simple 5-state chain: states 0,1,2,3,4. Goal: state 4. γ=0.9
# Policy: always go right (action=1)

n_states = 5
gamma    = 0.9

# Transition matrix P[s, s'] for policy "always right"
P = np.zeros((n_states, n_states))
for s in range(n_states - 1):
    P[s, s+1] = 1.0
P[4, 4] = 1.0   # absorbing state

# Reward vector R[s] = reward received when leaving state s (rightward)
R = np.zeros(n_states)
R[3] = 1.0   # transitioning from state 3 to state 4 earns +1

# Exact solution: V = (I - gamma*P)^{-1} @ R
I = np.eye(n_states)
V_exact = np.linalg.solve(I - gamma * P, R)
print("Exact V^π:", V_exact.round(4))

# ---- Temporal Difference (TD) learning ----
V_td = np.zeros(n_states)
alpha = 0.1

def step(state):
    next_state = min(state + 1, n_states - 1)
    reward = 1.0 if state == 3 else 0.0
    return next_state, reward

for episode in range(2000):
    state = 0
    for _ in range(20):
        next_state, reward = step(state)
        td_error = reward + gamma * V_td[next_state] - V_td[state]
        V_td[state] += alpha * td_error
        state = next_state
        if state == n_states - 1:
            break

print("TD-learned V^π:", V_td.round(4))

# ---- Advantage function ----
# Q(s, a=right) = R[s] + gamma * V[next_s]
Q_right = np.array([R[s] + gamma * V_td[min(s+1, 4)] for s in range(n_states)])
A_right = Q_right - V_td   # advantage of going right over average
print("Advantage A(s, right):", A_right.round(4))
# Positive: going right is better than average from that state

# ---- Neural network value function (sketch) ----
# In PyTorch (pseudocode):
# class ValueNet(nn.Module):
#     def __init__(self, state_dim):
#         super().__init__()
#         self.net = nn.Sequential(
#             nn.Linear(state_dim, 256), nn.ReLU(),
#             nn.Linear(256, 256),        nn.ReLU(),
#             nn.Linear(256, 1)
#         )
#     def forward(self, state):
#         return self.net(state).squeeze(-1)   # scalar V(s)
#
# TD update:
# with torch.no_grad():
#     target = reward + gamma * value_net(next_state) * (1 - done)
# loss = F.mse_loss(value_net(state), target)
# optimizer.zero_grad(); loss.backward(); optimizer.step()
```

## See Also
- [[Policy]] — the strategy V^π evaluates; improved using V via policy gradient / actor-critic
- [[Reward]] — the raw signal V^π accumulates over time; solves the credit assignment problem
- [[Agent]] — the learner that trains and uses the value function
- [[State]] — the input to V^π; value is defined over the state space
- [[Markov Property]] — required for V^π(s_t) to be well-defined without conditioning on history
- [[Environment]] — the source of transitions and rewards that V^π summarizes
- [[Gradient Descent]] — used to train neural network value function approximators
- [[Loss Function]] — the TD error squared is the loss used to train V_φ(s)
- [[Convergence]] — TD learning converges to V^π under tabular conditions; approximate methods are less guaranteed
- [[Geometric Series]] — the discounted return Σγ^t·r_t is a geometric series; the same convergence condition |γ|<1 that makes geometric series finite makes the return well-defined
- [[Conservation of Energy]] — value is "stored potential" — the same conceptual role as potential energy in physics; the Bellman equation is a conservation law for value across transitions
