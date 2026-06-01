# Markov Property

**One-liner:** The future depends only on the present — given the current state and action, the past provides no additional information about where you'll end up.

## Core Idea
$$P(s_{t+1} \mid s_t, a_t,\; s_{t-1}, a_{t-1}, \ldots, s_0, a_0) = P(s_{t+1} \mid s_t, a_t)$$
The transition probability depends only on the current state s_t and action a_t — not on the entire history of states and actions. This "memoryless" property collapses an infinite-dimensional conditioning problem (the full history) into a finite one (just s_t and a_t), making RL mathematically tractable.

## Why It Exists
Without the Markov Property, the [[Value Function]] V^π(s_t) would not be well-defined — you'd need V^π(s_t, s_{t-1}, s_{t-2}, ..., s_0) instead, which grows with time and is intractable to learn. The Markov Property guarantees that a mapping from the current state alone to the expected future return exists and is unique. It is the foundational assumption that makes MDPs and all of standard RL theory work.

## Real-World Applications
- **Board games (AlphaGo, Chess):** The board position is a perfect Markov state. All information about how the position was reached is irrelevant to future play — only the current position matters. This makes tree search + value functions work exactly.
- **Physical simulation (Isaac Sim):** Robot joint angles + velocities form a Markov state for Newtonian physics. Given the current physics state and torque commands, Newton's laws determine future states deterministically. This is why RL works well in physics simulators.
- **Baymax (LiDAR + velocity):** The current LiDAR scan + robot velocity is approximately Markov for navigation in static environments. Moving objects (humans) break it — the environment is non-stationary relative to the observation.
- **Language models (RLHF):** The full token sequence so far is the state. It is Markov for text generation — P(next token | full history) = P(next token | context window). The finite context window is a practical approximation.
- **Real robots (partial observability):** A robot using only a front-facing camera does NOT have a Markov state. It can't see behind itself. Solutions: frame stacking, RNNs, or maintaining a SLAM map.

## Intuition
The Markov Property says the state is a sufficient statistic for the future. Everything relevant about the past has been "compressed" into the current state. This is not a fact about the world — it is a requirement that the state representation must satisfy.

**When it holds:**
- Full physics state (position + velocity of all objects)
- Complete game state (all pieces, turn, captured pieces)
- Full conversation history (nothing forgotten)

**When it is violated:**
1. **Partial observability:** A robot sees position but not velocity — s_t = position alone is not Markov. The velocity (hidden state) still affects future trajectories.
2. **Non-stationary environments:** Other agents change behavior over time. The state doesn't capture "what round is it?" in an iterated game.
3. **Memory-dependent opponents:** An opponent who adapts to your strategy — their next move depends on the whole history, not just the current board.
4. **Real-world aliasing:** Two physically different situations look identical from available sensors but lead to different futures. Classic example: a maze with identical-looking corridors that branch differently.

**The fix:** Expand the state to include enough history that the Markov Property is restored. Formally, if the raw observation o_t is not Markov, then the history (o_0, a_0, ..., o_t) IS Markov (trivially — it contains everything). In practice:
- **Frame stacking:** Append last k observations as a proxy for velocity / recent history.
- **RNNs:** The hidden state h_t = f(h_{t-1}, o_t) is trained to be a Markov summary of history.
- **Belief states:** Maintain P(s_t | o_{0:t}) — the full posterior over the hidden state.

## Derivation
**Markov Chain:** A sequence of random variables (X_0, X_1, ...) is a Markov chain if:
$$P(X_{t+1} = x \mid X_t, X_{t-1}, \ldots, X_0) = P(X_{t+1} = x \mid X_t)$$

**Markov Decision Process (MDP):** Adds actions and rewards to a Markov chain:
$$P(s_{t+1} \mid s_t, a_t) = \text{transition kernel}$$

The Markov Property enables the **Bellman decomposition**:
$$V^\pi(s) = \mathbb{E}_\pi\left[\sum_{k=0}^\infty \gamma^k r_{t+k} \;\middle|\; s_t = s\right]$$
$$= \mathbb{E}\left[r_t + \gamma \sum_{k=0}^\infty \gamma^k r_{t+1+k} \;\middle|\; s_t = s\right]$$
$$= \mathbb{E}\left[r_t + \gamma V^\pi(s_{t+1}) \;\middle|\; s_t = s\right]$$

This decomposition — **V^π(s) = E[r + γV^π(s')]** — is only valid if s_{t+1} depends only on s_t and a_t. If the [[Markov Property]] fails, then V^π(s_{t+1}) is not determined by s_{t+1} alone, and the recursion breaks. The entire Bellman equation requires Markov states.

**Why this is necessary for convergence:** Value iteration updates:
$$V_{k+1}(s) = \max_a \sum_{s'} P(s'|s,a)[R(s,a,s') + \gamma V_k(s')]$$

are contractions in the sup-norm (by factor γ) when P(s'|s,a) is a valid Markov transition. This guarantees [[Convergence]] to V*. Without the Markov Property, P is ill-defined and the contraction argument fails.

**Sufficient conditions for a valid Markov state:**
A state representation φ(h_t) of history h_t is Markov if and only if:
1. P(s_{t+1} | φ(h_t), a_t) = P(s_{t+1} | h_t, a_t)  — transition is determined
2. P(r_t | φ(h_t), a_t) = P(r_t | h_t, a_t)  — reward is determined

## Worked Example
```python
import numpy as np
from collections import deque

# Demonstrate: non-Markov observation → Markov via frame stacking

class PartialGridEnv:
    """
    1D grid with position AND velocity (hidden).
    Observation: position only. True state: (position, velocity).
    P(next_pos | pos, action) is NOT Markov — velocity matters!
    """
    def __init__(self):
        self.pos = 0.0
        self.vel = 0.0

    def reset(self, vel_init=1.0):
        self.pos = 0.0
        self.vel = vel_init   # hidden variable — agent can't see this
        return self.pos

    def step(self, force):
        self.vel  = 0.8 * self.vel + force   # velocity persists (momentum)
        self.pos += self.vel * 0.1
        return self.pos, 0.0, False

env = PartialGridEnv()

# Two scenarios: same starting position, same action, DIFFERENT outcomes
# because velocity is different (not in observation)
for vel_init in [1.0, -1.0]:
    obs = env.reset(vel_init=vel_init)
    print(f"\nInitial obs (pos)={obs:.2f}, hidden vel={vel_init:.1f}")
    for t in range(5):
        obs, _, _ = env.step(force=0.5)   # same action
        print(f"  step {t+1}: pos={obs:.3f}")

# Same observation (pos=0), same action (force=0.5), different outcomes → NOT Markov!

print("\n--- Frame stacking restores Markov property ---")

class FrameStackWrapper:
    def __init__(self, env, k=3):
        self.env = env
        self.k = k
        self.frames = deque(maxlen=k)

    def reset(self, vel_init=1.0):
        obs = self.env.reset(vel_init=vel_init)
        for _ in range(self.k):
            self.frames.append(obs)
        return np.array(self.frames)   # stacked state

    def step(self, force):
        obs, r, done = self.env.step(force)
        self.frames.append(obs)
        return np.array(self.frames), r, done

wrapped = FrameStackWrapper(PartialGridEnv(), k=3)

for vel_init in [1.0, -1.0]:
    stacked_obs = wrapped.reset(vel_init=vel_init)
    print(f"\nInitial vel={vel_init:.1f}, initial stack: {stacked_obs.round(3)}")
    stacked_obs, _, _ = wrapped.step(0.5)
    print(f"After 1 step: stack={stacked_obs.round(3)}")
    inferred_vel = (stacked_obs[-1] - stacked_obs[-2]) / 0.1
    print(f"Inferred velocity from stack: {inferred_vel:.3f} (true: {wrapped.env.vel:.3f})")

# With frame stacking, the stack encodes velocity information
# Two different hidden velocities → distinguishable stacks → Markov is (approximately) restored
```

## See Also
- [[State]] — the representation that must satisfy the Markov Property for RL to work
- [[Environment]] — defines the transition function P(s'|s,a) that the Markov Property constrains
- [[Value Function]] — the Bellman equation that defines it requires the Markov Property
- [[Agent]] — must decide whether its observations are Markov or whether history is needed
- [[Policy]] — maps current state to action; valid only when state is Markov
- [[Reward]] — part of the MDP definition alongside the Markov transition
- [[Action]] — the other input to the Markov transition function alongside state
- [[Convergence]] — TD and value iteration convergence proofs rely on the Markov Property
