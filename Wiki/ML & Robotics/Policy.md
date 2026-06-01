# Policy

**One-liner:** The agent's strategy — a mapping from states to actions (or distributions over actions) that completely defines how the agent behaves.

## Core Idea
$$\pi: \mathcal{S} \rightarrow \Delta(\mathcal{A}), \quad a_t \sim \pi(\cdot \mid s_t)$$
A stochastic policy π(a|s) gives a probability distribution over actions given the current state. A deterministic policy μ: S → A gives a single action. The policy IS what the [[Agent]] is learning — everything else (the [[Value Function]], the loss, the gradient) exists only to improve it. In code, a policy is a function (or neural network) with a specific interface: takes state in, returns action (or action distribution) out.

## Why It Exists
The policy formalizes the question "what should I do?" as a mathematical object that can be represented, differentiated, and optimized. Without this abstraction it's unclear what the learning algorithm is modifying. The policy as a named entity also enables transfer: a policy trained in one environment can be fine-tuned or evaluated in another without changing the underlying RL machinery.

## Real-World Applications
- **AlphaGo:** A policy network (13-layer CNN) trained by supervised learning on human games, then improved by self-play RL using REINFORCE and MCTS. π(a|s) outputs a probability over 361 board positions.
- **Locomotion (Isaac Lab):** A neural network policy (2-layer MLP, 256 units) that takes ~48-dim robot state and outputs 12 joint torques. Trained with PPO. This policy runs at 50Hz on the real robot after sim-to-real transfer.
- **ChatGPT (RLHF):** The language model IS the policy. π(token|context) — a probability distribution over 50,000+ tokens given the conversation history. RLHF fine-tunes this policy toward human preferences.
- **Baymax navigation:** A policy mapping (LiDAR + goal direction) → (v, ω) — linear and angular velocity commands. Deterministic at deployment; stochastic during training for exploration.
- **SAC robot manipulation:** A stochastic Gaussian policy for dexterous manipulation, where maintaining action entropy is explicitly incentivized (maximum entropy RL).

## Intuition
Think of the policy as the agent's "rulebook." In chess, a human player's strategy — when to attack, when to defend, how to respond to specific positions — is a policy. A grandmaster has a better policy than a beginner; RL is the process of automatically improving the rulebook through experience.

**CS analogy — [[Interface]]:** The policy is like a Java interface or Python abstract class. It defines a contract: given a state, produce an action (or distribution). The interface is fixed; the implementation can vary. A lookup table policy, a linear policy, and a 100M-parameter neural network all implement the same interface. The RL algorithm doesn't care about the implementation — only that the interface is satisfied. This is why the same PPO algorithm works for Atari, locomotion, and language.

**Deterministic vs stochastic:**
| | Deterministic π: S → A | Stochastic π: S → Δ(A) |
|---|---|---|
| Exploration | External noise required | Built-in via distribution |
| Policy gradient | Requires off-policy tricks (DPG) | Standard REINFORCE works |
| Use case | Exploitation / deployment | Training, max-entropy RL |
| Example | DDPG, TD3 | PPO, SAC, REINFORCE |

**Why stochastic policies during training?** Exploration. If the policy always outputs the same action for a given state, the agent never discovers that other actions might be better. A stochastic policy explores naturally, and the entropy H(π(·|s)) measures how much it's exploring.

## Derivation
The policy gradient theorem gives the gradient of the expected return J(π_θ) = E_π[G_0] with respect to policy parameters θ:

$$\nabla_\theta J(\theta) = \mathbb{E}_\pi\left[\nabla_\theta \log \pi_\theta(a_t \mid s_t) \cdot Q^{\pi}(s_t, a_t)\right]$$

**Derivation sketch:**
$$J(\theta) = \sum_s d^\pi(s) \sum_a \pi_\theta(a|s) Q^\pi(s,a)$$

where d^π(s) is the stationary distribution of states under π. Taking the gradient and using the log-derivative trick ∇π = π∇log π:

$$\nabla_\theta J = \sum_s d^\pi(s) \sum_a \nabla_\theta \pi_\theta(a|s) Q^\pi(s,a)$$
$$= \mathbb{E}_\pi\left[\nabla_\theta \log \pi_\theta(a|s) \cdot Q^\pi(s,a)\right]$$

In practice Q^π(s,a) is replaced by the sample return G_t (REINFORCE) or an estimated advantage A(s,a) = Q(s,a) − V(s) (actor-critic, PPO).

**PPO's clipped objective** — the dominant policy optimization method:
$$\mathcal{L}^{\text{PPO}}(\theta) = \mathbb{E}_t\left[\min\left(r_t(\theta) \hat{A}_t,\; \text{clip}(r_t(\theta), 1-\epsilon, 1+\epsilon)\hat{A}_t\right)\right]$$
where r_t(θ) = π_θ(a_t|s_t) / π_{\text{old}}(a_t|s_t) is the probability ratio. The clip prevents large policy updates that would destabilize training — a crucial practical improvement over vanilla policy gradient.

## Worked Example
```python
import numpy as np

# --- Policy as an interface: three implementations ---
# Interface contract: given state -> return action

# 1. Lookup table policy (tabular, discrete)
class TabularPolicy:
    def __init__(self, n_states, n_actions):
        # Uniform random initially
        self.table = np.ones((n_states, n_actions)) / n_actions

    def __call__(self, state):
        """Sample action from probability distribution."""
        return np.random.choice(len(self.table[state]), p=self.table[state])

    def update(self, state, action, advantage):
        """Crude policy gradient update (REINFORCE step)."""
        lr = 0.01
        self.table[state, action] += lr * advantage
        self.table[state]  = np.clip(self.table[state], 1e-6, None)
        self.table[state] /= self.table[state].sum()   # renormalize

# 2. Linear softmax policy (discrete actions, continuous state)
class LinearSoftmaxPolicy:
    def __init__(self, state_dim, n_actions):
        self.W = np.zeros((n_actions, state_dim))

    def logits(self, state):
        return self.W @ state

    def probs(self, state):
        l = self.logits(state)
        e = np.exp(l - l.max())
        return e / e.sum()

    def __call__(self, state):
        return np.random.choice(len(self.W), p=self.probs(state))

    def log_prob(self, state, action):
        p = self.probs(state)
        return np.log(p[action] + 1e-8)

# 3. Gaussian policy (continuous actions)
class GaussianPolicy:
    def __init__(self, state_dim, action_dim):
        self.W_mu   = np.zeros((action_dim, state_dim))
        self.log_std = np.full(action_dim, -1.0)

    def __call__(self, state):
        mu  = self.W_mu @ state
        std = np.exp(self.log_std)
        return mu + std * np.random.randn(*mu.shape)   # reparameterization

    def log_prob(self, state, action):
        mu  = self.W_mu @ state
        std = np.exp(self.log_std)
        return (-0.5 * ((action - mu) / std)**2 - np.log(std)
                - 0.5 * np.log(2*np.pi)).sum()

# --- All three implement the same interface: policy(state) -> action ---
n_states, n_actions = 5, 3
state_dim, action_dim = 4, 2

tab_policy  = TabularPolicy(n_states, n_actions)
lin_policy  = LinearSoftmaxPolicy(state_dim, n_actions)
gauss_policy = GaussianPolicy(state_dim, action_dim)

discrete_state   = 2
continuous_state = np.array([0.5, -0.3, 1.2, 0.0])

print("Tabular (discrete):", tab_policy(discrete_state))
print("Linear softmax:    ", lin_policy(continuous_state))
print("Gaussian (continuous):", gauss_policy(continuous_state).round(3))

# All implement the same contract: state in, action out
# The RL algorithm (PPO, REINFORCE) doesn't care which is used
policies = [lin_policy, gauss_policy]   # heterogeneous list — they share the interface
```

## See Also
- [[Agent]] — the policy IS the agent's decision-making component
- [[Value Function]] — the critic that evaluates policy quality; guides policy improvement
- [[State]] — the input to the policy at every timestep
- [[Action]] — the output of the policy at every timestep
- [[Reward]] — the signal used (via policy gradient) to update the policy
- [[Environment]] — the world the policy acts in and learns from
- [[Markov Property]] — the assumption that makes a policy mapping from s_t (not history) sufficient
- [[Interface]] — the policy-as-interface analogy; same algorithm, many implementations
- [[Class]] — policy implementations are classes with a shared interface
- [[Gradient]] — policy gradient methods use gradients of J(π) w.r.t. policy parameters
- [[Stochastic Gradient Descent]] — the optimizer used to update policy parameters
- [[Newton's First Law]] — a deterministic policy is like inertia: it keeps producing the same action from the same state unless an external update (gradient step) changes it
