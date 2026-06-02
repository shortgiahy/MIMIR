# State

**One-liner:** The agent's snapshot of the world at time t — the information from which it must choose its next action.

## Core Idea
$$s_t \in \mathcal{S}, \quad a_t \sim \pi(\cdot \mid s_t)$$
A state s_t is an element of the state space S. It is the input to the [[Policy]] and [[Value Function]]. In a fully observable environment s_t captures all information about the world that is relevant to future decisions. The [[Markov Property]] demands that s_t alone — without the history s_{t-1}, s_{t-2}, ... — is sufficient to predict future outcomes optimally.

## Why It Exists
The [[Agent]] cannot act on the raw universe. State is the abstraction that compresses everything relevant into a tractable representation. Good state design is one of the most impactful engineering decisions in applied RL: too little information and the agent cannot learn; too much and the input space is wasteful or noise-dominated.

## Real-World Applications
- **Atari DQN:** State = last 4 grayscale frames (84×84 pixels each), stacked to give the agent velocity information. Raw pixels — 28,000 numbers — are the state.
- **Go (AlphaGo):** State = current board position (19×19 binary planes encoding stone colors and legal moves). 48 feature planes total.
- **Robot locomotion (Isaac Sim / Unitree Go2):** State = joint angles, joint velocities, base linear velocity, base angular velocity, foot contact flags, gravity vector in body frame — typically ~48 floats.
- **Baymax navigation:** State = LiDAR point cloud + camera image + robot pose (position + heading) + goal direction. A perception stack processes raw sensors into a compact state vector.
- **ChatGPT (RLHF):** State = the full conversation history (token sequence so far). The model's internal hidden state is the compressed representation of this.

## Intuition
**Full observability:** The agent sees the true state of the environment — like looking at the full Go board. The state is Markov: everything you need to know is right there.

**Partial observability:** The agent only sees an observation o_t that is a noisy or incomplete function of the true state s_t. A robot using only front-facing cameras doesn't know what's behind it. The observation o_t is NOT Markov — knowing it alone isn't enough. Solutions:

1. **Frame stacking:** Concatenate the last k observations. Works for games where velocity ≈ position difference.
2. **Recurrent networks (LSTM/GRU):** Maintain a hidden state h_t that summarizes history. The hidden state becomes the effective Markov state.
3. **Belief states:** Maintain P(s_t | o_{0:t}, a_{0:t-1}) — a distribution over true states. Mathematically correct but expensive.

**State representation design choices:**
- **Low-level (joints + velocities):** Fast, interpretable, but requires careful feature engineering for contact-rich tasks.
- **Privileged information:** During training in simulation, the agent may receive the true physics state (including hidden variables). At test time only observations are available. This is asymmetric actor-critic.
- **Learned embeddings:** A CNN/ViT encodes raw pixels into a latent state vector. The RL agent then operates in latent space.

## Derivation
In a fully observable MDP, the state satisfies the [[Markov Property]]:
$$P(s_{t+1} \mid s_t, a_t, s_{t-1}, a_{t-1}, \ldots) = P(s_{t+1} \mid s_t, a_t)$$

This is what makes the state a *sufficient statistic* for the future. Given s_t, no additional history provides extra predictive power.

In a POMDP, the observation o_t = f(s_t) + noise is not Markov. The belief state b_t is Markov:
$$b_{t+1}(s') = \eta \cdot P(o_{t+1} \mid s') \sum_s P(s' \mid s, a_t) b_t(s)$$
where η is a normalizing constant. Computing this exactly requires summing over all states — exponential in continuous/large state spaces, which is why approximate methods (particle filters, RNNs) dominate in practice.

**State abstraction:** A function φ: S → Z maps states to an abstract representation. φ is a valid abstraction if the optimal policy in Z is the same as in S — i.e., the abstraction is lossless with respect to the [[Value Function]].

## Worked Example
```python
import numpy as np

# Demonstrate partial observability and frame stacking in a 1D position task
# True state: (position, velocity). Observation: position only (velocity hidden).

class PartiallyObservableEnv:
    def __init__(self):
        self.pos = 0.0
        self.vel = 0.0
        self.dt  = 0.1

    def reset(self):
        self.pos = np.random.uniform(-1, 1)
        self.vel = np.random.uniform(-0.5, 0.5)
        return self.observe()

    def observe(self):
        return self.pos + np.random.normal(0, 0.05)   # noisy position

    def step(self, force):
        """force in [-1, 1]"""
        self.vel = 0.9 * self.vel + force * self.dt   # damped velocity
        self.pos = self.pos + self.vel * self.dt
        reward   = -abs(self.pos)                      # reward for staying near 0
        done     = abs(self.pos) > 2.0
        return self.observe(), reward, done

env = PartiallyObservableEnv()
k   = 3   # frame stack size

# Single observation — cannot infer velocity
obs = env.reset()
print(f"Single obs (pos only): {obs:.3f}  — velocity is HIDDEN")

# Frame-stacked state — velocity ≈ (obs[t] - obs[t-1]) / dt
stack = [obs] * k
obs2, _, _ = env.step(0.5)
obs3, _, _ = env.step(0.5)
stack = [obs, obs2, obs3]
inferred_vel = (stack[-1] - stack[-2]) / env.dt
print(f"Stacked obs: {[f'{x:.3f}' for x in stack]}")
print(f"Inferred velocity: {inferred_vel:.3f}  (true: {env.vel:.3f})")

# State vector for a legged robot (example structure)
def make_robot_state(joint_angles, joint_velocities, base_lin_vel,
                     base_ang_vel, gravity_vector, foot_contacts):
    """
    Concatenate all sensor readings into a flat state vector.
    For Unitree Go2: 12 joints, so ~48-dim state.
    """
    return np.concatenate([
        joint_angles,        # 12 floats: hip/thigh/calf for each leg
        joint_velocities,    # 12 floats
        base_lin_vel,        # 3  floats: vx, vy, vz
        base_ang_vel,        # 3  floats: roll/pitch/yaw rate
        gravity_vector,      # 3  floats: gravity direction in body frame
        foot_contacts,       # 4  binary: which feet are on ground
    ])

example_state = make_robot_state(
    joint_angles     = np.zeros(12),
    joint_velocities = np.zeros(12),
    base_lin_vel     = np.array([0.5, 0.0, 0.0]),
    base_ang_vel     = np.zeros(3),
    gravity_vector   = np.array([0.0, 0.0, -9.81]),
    foot_contacts    = np.array([1., 1., 1., 1.]),
)
print(f"Robot state dim: {len(example_state)}")  # 37
```

## See Also
- [[Markov Property]] — the mathematical condition a state must satisfy to be a valid MDP state
- [[Environment]] — the source of state observations
- [[Agent]] — the entity that receives and acts on the state
- [[Policy]] — maps states to actions; its quality depends entirely on state quality
- [[Value Function]] — maps states to expected return; defined over the state space
- [[Action]] — the agent's output given the current state
- [[Reward]] — often a function of (s_t, a_t, s_{t+1})
- [[Kinematic Equations]] — for robot locomotion tasks, the state vector typically contains position and velocity variables that are related by kinematic equations
- [[Velocity]] — velocity is commonly a component of the RL state vector for any physical system; without it, the state violates the Markov property (position alone is insufficient to predict next position)
