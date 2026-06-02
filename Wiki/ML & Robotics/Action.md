# Action

**One-liner:** The output of the agent at each timestep — what it does to the environment in order to influence future states and rewards.

## Core Idea
$$a_t \sim \pi(\cdot \mid s_t), \quad a_t \in \mathcal{A}$$
An action a_t is sampled from the [[Policy]] π conditioned on the current [[State]] s_t, and belongs to the action space A. The action is then sent to the [[Environment]], which uses it to compute the next state s_{t+1} and reward r_t. Actions are the only lever the [[Agent]] has to influence the world.

## Why It Exists
Without an action space, the agent is a passive observer. The action space is the agent's "vocabulary" for affecting the environment. Its design — discrete vs continuous, constrained vs unconstrained — directly determines what the agent can learn to do and how hard the learning problem is.

## Real-World Applications
- **Atari games (DQN):** Discrete action space of 18 joystick positions (up/down/left/right + fire combinations). The agent selects one integer per frame.
- **Go (AlphaGo):** Discrete action space of 361 board positions (19×19) + pass. AlphaZero's policy head outputs a softmax over all positions.
- **Legged robot locomotion (Isaac Sim):** Continuous action space — 12 joint torques (3 per leg) in Newton-meters. The policy outputs a 12-dimensional real-valued vector.
- **Baymax navigation:** Continuous action space — linear velocity v ∈ [0, v_max] and angular velocity ω ∈ [-ω_max, ω_max]. Two floats per timestep.
- **ChatGPT (RLHF):** Discrete action space — the next token from a vocabulary of ~50,000 tokens. The policy outputs a probability distribution over the vocabulary.
- **Robot manipulation (dexterous hand):** High-dimensional continuous action space — 16–24 joint torques for fingertip control.

## Intuition
The action space is the agent's "control panel." Design it too small and the agent can't express the behavior you want; too large and the learning problem becomes exponentially harder.

**Discrete vs continuous:**
- **Discrete:** Finite set of options. Easy to represent (categorical distribution, softmax). Hard to scale — a robot with 12 joints at 100 levels each has 100¹² possible action combinations. Must be factored.
- **Continuous:** A vector in ℝⁿ. Naturally scales to high-dimensional control. Requires outputting a distribution (usually Gaussian: mean + log_std). The policy outputs μ(s) and σ(s), and actions are sampled as a ~ N(μ, σ²).

**Action constraints in real robots:**
1. **Joint limits:** Mechanical stops prevent hyperextension. Actions must be clipped to the safe range.
2. **Torque/velocity limits:** Actuator saturation — too large a command gets clipped by hardware.
3. **Rate limits:** Large action changes between timesteps cause jerky motion and hardware stress. Often penalized in the reward or filtered with a low-pass.
4. **Safety constraints:** Actions that would cause collisions or tip the robot must be blocked. Constraint-aware RL (e.g., CPO, FOCOPS) handles this formally.

**Exploration requires trying actions:**
Without trying diverse actions, the agent cannot discover that some actions lead to high reward. Exploration strategies:
- **ε-greedy:** With probability ε, sample a random action instead of the policy's best guess.
- **Gaussian noise:** Add N(0, σ²) to continuous actions during training.
- **Entropy bonus:** Add H(π(·|s)) to the reward to encourage the policy to remain stochastic.

## Derivation
For a **continuous action space** with Gaussian policy:
$$\pi_\theta(a \mid s) = \mathcal{N}(a; \mu_\theta(s), \sigma_\theta^2(s))$$

The policy network outputs μ_θ(s) and log σ_θ(s). Action sampling:
$$a = \mu_\theta(s) + \sigma_\theta(s) \cdot \epsilon, \quad \epsilon \sim \mathcal{N}(0, I)$$

This "reparameterization trick" makes a differentiable w.r.t. θ (the randomness ε is separate from the parameters), enabling backpropagation through action sampling.

Log probability (needed for policy gradient updates):
$$\log \pi_\theta(a \mid s) = -\frac{(a - \mu_\theta(s))^2}{2\sigma_\theta^2(s)} - \log \sigma_\theta(s) - \frac{1}{2}\log(2\pi)$$

For a **discrete action space** with softmax policy:
$$\pi_\theta(a \mid s) = \text{softmax}(f_\theta(s))_a = \frac{e^{f_\theta(s)_a}}{\sum_{a'} e^{f_\theta(s)_{a'}}}$$

Policy gradient for either case (REINFORCE):
$$\nabla_\theta J(\theta) = \mathbb{E}_\pi\left[\nabla_\theta \log \pi_\theta(a_t \mid s_t) \cdot G_t\right]$$

Actions that lead to high return G_t have their log probability increased; low-return actions are suppressed.

## Worked Example
```python
import numpy as np

# --- Discrete action space: softmax policy ---
class DiscretePolicy:
    def __init__(self, n_states, n_actions):
        self.logits = np.zeros((n_states, n_actions))   # unnormalized scores

    def softmax(self, x):
        e = np.exp(x - x.max())
        return e / e.sum()

    def action_probs(self, state):
        return self.softmax(self.logits[state])

    def select_action(self, state):
        probs = self.action_probs(state)
        return np.random.choice(len(probs), p=probs)

disc = DiscretePolicy(n_states=5, n_actions=3)
for s in range(5):
    probs = disc.action_probs(s)
    action = disc.select_action(s)
    print(f"State {s}: probs={probs.round(3)}, sampled action={action}")


# --- Continuous action space: Gaussian policy ---
class GaussianPolicy:
    def __init__(self, state_dim, action_dim):
        # Simple linear policy: mu = W_mu @ s + b_mu
        self.W_mu   = np.zeros((action_dim, state_dim))
        self.log_std = np.full(action_dim, -1.0)    # std = exp(-1) ≈ 0.37

    def forward(self, state):
        mu  = self.W_mu @ state
        std = np.exp(self.log_std)
        return mu, std

    def sample(self, state):
        mu, std = self.forward(state)
        eps     = np.random.randn(*mu.shape)
        action  = mu + std * eps           # reparameterization trick
        return action, mu, std

    def log_prob(self, action, mu, std):
        return (-0.5 * ((action - mu) / std) ** 2
                - np.log(std)
                - 0.5 * np.log(2 * np.pi)).sum()

# Robot arm: 3-joint continuous control
gauss = GaussianPolicy(state_dim=6, action_dim=3)
state = np.array([0.1, -0.2, 0.5, 0.0, 0.1, -0.3])   # joint positions + velocities
action, mu, std = gauss.sample(state)
lp = gauss.log_prob(action, mu, std)

print(f"\nGaussian policy:")
print(f"  Mean (mu):    {mu.round(3)}")
print(f"  Std:          {std.round(3)}")
print(f"  Sampled action: {action.round(3)}")
print(f"  Log prob: {lp:.3f}")

# Action clipping for robot safety
joint_limits = np.array([-1.5, -1.5, -1.5]), np.array([1.5, 1.5, 1.5])
safe_action = np.clip(action, joint_limits[0], joint_limits[1])
print(f"  Safe (clipped) action: {safe_action.round(3)}")
```

## See Also
- [[Policy]] — the mapping from state to action (or action distribution) that the agent learns
- [[State]] — the input to the policy; determines which action is chosen
- [[Environment]] — receives the action and produces the next state and reward
- [[Reward]] — the feedback signal generated after an action is taken
- [[Agent]] — the entity that selects and executes actions
- [[Value Function]] — evaluates how good it is to take action a in state s (Q-function)
- [[Markov Property]] — actions, along with states, define the MDP transition structure
- [[Force]] — in robotics, an action often is a force or torque command; the mapping from policy output to physical effect goes through Newton's laws
- [[Method Overriding]] — in a class hierarchy of agents, the act() method is overridden by each subclass to implement a different action-selection strategy while keeping the same interface
