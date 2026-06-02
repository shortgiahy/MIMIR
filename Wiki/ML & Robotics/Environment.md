# Environment

**One-liner:** Everything outside the agent — the world that receives actions, transitions to new states, and emits observations and rewards.

## Core Idea
$$P(s_{t+1} \mid s_t, a_t) \quad \text{and} \quad R(s_t, a_t, s_{t+1})$$
The environment is fully characterized by two functions: the **transition function** P giving the probability of landing in state s_{t+1} after taking action a_t in state s_t, and the **reward function** R giving the scalar feedback for that transition. Together they define a Markov Decision Process (MDP). The [[Agent]] never sees these functions directly — it only experiences their outputs.

## Why It Exists
The environment concept separates concerns cleanly: the agent is responsible for deciding what to do; the environment is responsible for the consequences. This boundary (analogous to an [[Interface]] in CS) means the same agent algorithm can be applied to any environment that obeys the MDP contract — from Atari games to robot simulators to financial markets.

## Real-World Applications
- **OpenAI Gym / Gymnasium:** A standardized Python environment interface (reset/step/render) used for benchmarking RL algorithms. Environments include CartPole, MuJoCo locomotion, and Atari.
- **Isaac Sim (NVIDIA):** A physics-accurate GPU-accelerated simulator that runs thousands of parallel robot environments simultaneously, providing Baymax-style agents with realistic sensor data and contact physics.
- **Isaac Lab:** Built on Isaac Sim, provides pre-built robot environments (Unitree Go2, ANYmal) and reward functions for locomotion, manipulation, and navigation.
- **Real robot environments:** The physical world — latency, sensor noise, unmodeled dynamics. Policies trained in simulation must bridge the "sim-to-real gap" caused by environment mismatch.
- **AlphaGo:** The Go game engine was the environment — deterministic transitions (board positions), sparse reward (win/loss at end).

## Intuition
Think of the environment as a black box with two slots: you push an action in, and two things come out — a new observation and a reward. You never see the gears inside (the transition probabilities). All the agent can do is learn the box's behavior through repeated interaction.

**Episodic vs continuing tasks:**
- **Episodic:** The interaction resets after a terminal condition (robot falls, game ends, task completed). Each episode is independent. Most robotics training is episodic.
- **Continuing:** No natural endpoint. The agent must balance immediate and long-term reward indefinitely. Requires discounting (γ < 1) to keep returns finite.

**Simulated vs real environments:**
| Property | Simulated (Isaac Sim) | Real world |
|---|---|---|
| Speed | 10,000× real time | Real time |
| Parallelism | Thousands of instances | One (usually) |
| Accuracy | Approximate physics | Ground truth |
| Reset | Instant | Manual/impossible |
| Sensor noise | Programmable | Uncontrolled |

Domain randomization — randomizing mass, friction, sensor noise in simulation — is the main technique to make policies trained in simulated environments transfer to real ones.

## Derivation
A fully observable environment is a **Markov Decision Process** (MDP), defined by the tuple (S, A, P, R, γ):

- S: state space
- A: action space
- P: S × A → Δ(S), transition kernel (Δ = probability simplex)
- R: S × A × S → ℝ, reward function
- γ ∈ [0,1): discount factor

The [[Markov Property]] requires that P(s_{t+1}|s_t, a_t) does not depend on s_{t-1}, s_{t-2}, ... — the full history.

A **Partially Observable MDP** (POMDP) extends this with:
- O: observation space (agent sees o_t ≠ s_t)
- Ω: O × S × A → [0,1], observation function P(o_t|s_t, a_t)

In a POMDP, the [[Markov Property]] holds at the state level but the agent only observes o_t, breaking the property at the observation level. The agent must maintain a **belief state** b_t = P(s_t | o_0:t, a_0:t-1) — a distribution over states — which is Markov even when individual observations aren't.

**Environment step in code terms:**
```
s_{t+1} ~ P(· | s_t, a_t)
r_t      = R(s_t, a_t, s_{t+1})
o_{t+1}  = observation_fn(s_{t+1})   # may be noisy / partial
```

## Worked Example
```python
import numpy as np

class SimpleGridEnv:
    """
    A minimal 5-cell 1D grid environment implementing the Gym interface.
    State: integer 0..4. Goal: reach cell 4.
    Transition: deterministic left/right with walls.
    Reward: +1 at goal, 0 otherwise.
    """

    def __init__(self, noise_prob=0.0):
        self.n_states  = 5
        self.goal      = 4
        self.noise_prob = noise_prob   # stochastic transition if > 0

    def reset(self):
        self.state = 0
        return self.state

    def step(self, action):
        """action: 0=left, 1=right"""
        rng = np.random.default_rng()
        # Optional: with probability noise_prob, action is reversed
        if rng.random() < self.noise_prob:
            action = 1 - action

        delta = 1 if action == 1 else -1
        self.state = max(0, min(self.n_states - 1, self.state + delta))

        done   = self.state == self.goal
        reward = 1.0 if done else 0.0
        info   = {"true_state": self.state}   # extra diagnostic info
        return self.state, reward, done, info

# Usage: the agent-environment interaction loop
env = SimpleGridEnv(noise_prob=0.1)
obs = env.reset()
total_reward = 0

for t in range(20):
    action = 1                              # always go right (naive policy)
    obs, reward, done, info = env.step(action)
    total_reward += reward
    print(f"t={t}: obs={obs}, reward={reward}, done={done}")
    if done:
        break

print(f"Total reward: {total_reward}")

# Stochastic env: sometimes agent goes left even when choosing right
env_stoch = SimpleGridEnv(noise_prob=0.3)
rewards = []
for _ in range(100):
    obs = env_stoch.reset()
    ep_reward = 0
    for _ in range(20):
        obs, r, done, _ = env_stoch.step(1)
        ep_reward += r
        if done: break
    rewards.append(ep_reward)
print(f"Success rate with noise=0.3: {np.mean(rewards)*100:.1f}%")
```

## See Also
- [[Agent]] — the entity that interacts with the environment
- [[State]] — what the environment exposes to the agent at each timestep
- [[Action]] — what the agent sends into the environment
- [[Reward]] — the scalar signal the environment emits
- [[Markov Property]] — the mathematical assumption the environment must satisfy for standard RL
- [[Policy]] — the agent's strategy for responding to environment observations
- [[Value Function]] — the agent's estimate of how good each state in the environment is
- [[Interface]] — the environment's step/reset API is a classic software interface
- [[Newton's Second Law]] — physics-based environments (Isaac Sim, MuJoCo) implement the environment's transition function by integrating F = ma; the robot's dynamics are Newton's laws
- [[Abstract Class]] — the Gym environment abstract class defines reset/step/render; every concrete environment (CartPole, Atari, MuJoCo) overrides these methods
