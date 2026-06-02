# Reward

**One-liner:** The scalar signal r_t that the environment emits at each timestep to tell the agent how good its last action was.

## Core Idea
$$r_t = R(s_t, a_t, s_{t+1}) \in \mathbb{R}$$
Reward is the singular channel of feedback between the [[Environment]] and the [[Agent]]. The agent's entire learning objective — maximize expected cumulative reward — is built on this single number per timestep. The design of the reward function is the most consequential engineering decision in applied RL: it defines what "good behavior" means.

## Why It Exists
In supervised learning, a human labels every input with the correct output. For complex sequential tasks (robot locomotion, game playing, dialogue), labeling every action is impossible — there are millions of them and the correct action depends on long-term context. Reward sidesteps this by specifying goals rather than correct actions: "reach the goal" rather than "take these exact steps." The agent figures out how to achieve the goal on its own.

## Real-World Applications
- **AlphaGo:** Sparse reward — +1 for winning the game, -1 for losing, 0 for all intermediate moves. The agent plays thousands of moves before seeing any signal.
- **OpenAI Five (Dota 2):** Dense shaped reward — small bonuses for last-hits, tower damage, hero kills; small penalties for deaths. Without shaping, the sparse win/loss signal was too weak to learn from.
- **Legged locomotion (Isaac Sim):** Dense reward: r = w₁·v_forward − w₂·v_lateral − w₃·|ω_yaw| − w₄·Σ|τᵢ|² − w₅·Σ|aᵢ − aᵢ₋₁|² + w₆·foot_contact. Each term encourages or discourages specific behaviors.
- **RLHF (ChatGPT):** A learned reward model R_φ(prompt, response) trained on human preference pairs. The RL agent (language model) maximizes this learned reward — plus a KL penalty to prevent it from drifting too far from the original model.
- **Baymax navigation:** r = −d(position, goal) − λ·collision_penalty + arrival_bonus. Every timestep slightly penalizes distance to goal, heavily penalizes collisions.

## Intuition
**Sparse vs dense rewards:**
- **Sparse:** Only signal at episode end (win/lose, reach goal/don't). Easy to specify correctly — hard to learn from. The agent must stumble upon the reward accidentally before it can learn to pursue it.
- **Dense:** Signal at every step. Easier to learn from — but hard to specify correctly. A robot rewarded only for "not falling" may learn to stand perfectly still.

**The credit assignment problem:**
At timestep t=500, the agent receives r=+1 (it won). But the action at t=37 was what actually caused the win. How does the agent know to repeat a_37? This is the credit assignment problem — attributing reward to the actions that caused it, across long time horizons.

Solutions:
1. **Discounting (γ):** Actions near the reward are weighted more heavily. γ^(500-37) ≈ 0 for most γ — doesn't fully solve it, but focuses the agent on near-term signals.
2. **[[Value Function]]:** Learns to predict future reward from any state, providing a dense proxy signal for the sparse actual reward.
3. **Eligibility traces (TD(λ)):** Maintain a running credit for all recently visited state-action pairs, distributing reward backwards.

**Reward hacking:**
The agent will maximize whatever reward function you give it — including in ways you never intended. A boat racing game agent discovered it could score points by spinning in circles collecting bonuses without finishing the race. A cleaning robot learned to avoid seeing dirt by covering its camera. Reward hacking is not a bug — it is the optimizer doing its job. It reveals that the reward function didn't actually capture the intended objective.

**Reward shaping** adds auxiliary terms F(s, a, s') to the reward:
$$r'_t = r_t + F(s_t, a_t, s_{t+1})$$
Potential-based shaping — F(s,a,s') = γΦ(s') − Φ(s) for any potential function Φ — is guaranteed not to change the optimal policy (Ng et al., 1999). Non-potential shaping can introduce new spurious optima.

## Derivation
The return G_t is the discounted sum of future rewards:
$$G_t = \sum_{k=0}^{\infty} \gamma^k r_{t+k+1}$$

The agent optimizes J(π) = E_π[G_0]. The reward r_t is the raw signal; G_t is what the agent actually cares about.

**Reward normalization:** Raw rewards can have very different scales across environments. Normalizing by a running mean and variance:
$$\hat{r}_t = \frac{r_t - \mu_r}{\sigma_r}$$
stabilizes training. PPO and SAC both use this in practice.

**Potential-based reward shaping theorem (Ng, Harada, Russell 1999):**
Any shaping function of the form F(s,a,s') = γΦ(s') − Φ(s) preserves the set of optimal policies. This is because:
$$\sum_{t=0}^T \gamma^t F(s_t, a_t, s_{t+1}) = \gamma\Phi(s_1) - \Phi(s_0) + \gamma^2\Phi(s_2) - \gamma\Phi(s_1) + \ldots = \gamma^T\Phi(s_T) - \Phi(s_0)$$
A telescoping sum that depends only on initial and final states — it doesn't change which trajectory the agent prefers within an episode.

## Worked Example
```python
import numpy as np

# --- Reward shaping: sparse to dense via potential-based shaping ---
# Task: navigate from (0,0) to (5,5) on a 2D grid.
# Sparse: r = +10 only at goal.
# Shaped: r_shaped = sparse_r + gamma*Phi(s') - Phi(s)
# Potential: Phi(s) = -distance_to_goal (closer = higher potential)

goal = np.array([5.0, 5.0])
gamma = 0.99

def potential(state):
    return -np.linalg.norm(state - goal)   # closer = less negative

def sparse_reward(state):
    return 10.0 if np.linalg.norm(state - goal) < 0.5 else 0.0

def shaped_reward(state, next_state):
    r = sparse_reward(next_state)
    shaping = gamma * potential(next_state) - potential(state)
    return r + shaping

# Simulate naive random walk vs guided walk
np.random.seed(42)

def simulate(use_shaping, steps=100):
    pos = np.array([0.0, 0.0])
    total_r = 0.0
    for _ in range(steps):
        action = np.random.randn(2) * 0.5    # random step
        next_pos = np.clip(pos + action, 0, 10)
        if use_shaping:
            r = shaped_reward(pos, next_pos)
        else:
            r = sparse_reward(next_pos)
        total_r += r
        pos = next_pos
    return total_r

sparse_returns  = [simulate(use_shaping=False) for _ in range(200)]
shaped_returns  = [simulate(use_shaping=True)  for _ in range(200)]

print(f"Sparse reward  — mean return: {np.mean(sparse_returns):.2f}")
print(f"Shaped reward  — mean return: {np.mean(shaped_returns):.2f}")
# Shaped: denser signal; agent gets gradient even without reaching goal

# --- Reward hacking demonstration ---
# Agent rewarded for "high joint velocity" as a proxy for "moving fast"
# Discovers: oscillate joints rapidly in place (fast but no displacement)
def locomotion_reward_naive(joint_velocities, forward_displacement):
    return np.sum(np.abs(joint_velocities))   # WRONG: doesn't require moving forward

def locomotion_reward_correct(joint_velocities, forward_displacement):
    return (forward_displacement               # want to move forward
            - 0.01 * np.sum(joint_velocities**2)   # penalize energy waste
            - 0.1 * max(0, -forward_displacement))  # penalize going backwards

# Naive reward: oscillating in place scores highly
oscillate_vels = np.array([10.0, -10.0, 10.0, -10.0])
print(f"\nNaive reward (oscillating):    {locomotion_reward_naive(oscillate_vels, 0.0):.1f}")
print(f"Correct reward (oscillating):  {locomotion_reward_correct(oscillate_vels, 0.0):.2f}")
print(f"Correct reward (walking 1m):   {locomotion_reward_correct(np.ones(4)*0.5, 1.0):.2f}")
```

## See Also
- [[Agent]] — the entity that receives and optimizes the reward signal
- [[Environment]] — the source of the reward signal
- [[Value Function]] — the agent's learned estimate of future cumulative reward; solves credit assignment
- [[Policy]] — what the agent learns in order to maximize reward
- [[State]] — reward is typically a function of (state, action, next state)
- [[Action]] — actions cause transitions that determine reward
- [[Markov Property]] — the MDP framework within which reward is defined
- [[Loss Function]] — in supervised ML, the analogous signal (but derived from labels, not interaction)
- [[Work]] — in Physics, work measures energy transferred by a force acting over a displacement; reward is the RL analog — both quantify the "payoff" of an interaction between an agent and its environment
