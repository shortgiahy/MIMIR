# Markov Decision Processes

**One-liner:** A Markov Decision Process (MDP) is the formal mathematical framework that defines what an RL problem actually is — states, actions, transition probabilities, and rewards, with the Markov property as the foundational assumption.

## Why It Exists

The agent-environment loop described in [[What is Reinforcement Learning]] needs a rigorous mathematical foundation before you can prove anything, design algorithms, or analyze convergence. Without a formal model, RL is just a vague intuition.

MDPs provide that foundation. They give precise definitions to every concept that RL relies on: what a state is, what an action does, how the environment responds, and what we're optimizing. This matters because:

1. **Algorithm correctness:** Most RL proofs (Q-learning converges, policy iteration finds the optimal policy) require the environment to be an MDP. If it's not, the guarantees break.
2. **Reward design:** The MDP formalism makes clear that the reward function must be a function of $(s, a, s')$ — not of history. This constrains how you design rewards.
3. **Generalization:** Every RL problem — robotics, games, resource allocation, financial trading — is either an MDP or an approximation of one. If you understand MDPs, you understand RL.
4. **Computational tractability:** The Markov property is what makes dynamic programming (and hence value iteration, policy iteration) work efficiently. Without it, optimal decision-making requires tracking the entire history, which is exponentially expensive.

## The Concept

### Formal Definition

A finite MDP is a 5-tuple $(\mathcal{S}, \mathcal{A}, P, R, \gamma)$:

- $\mathcal{S}$ — the **state space**: all possible states the environment can be in
- $\mathcal{A}$ — the **action space**: all possible actions the agent can take
- $P: \mathcal{S} \times \mathcal{A} \times \mathcal{S} \rightarrow [0, 1]$ — the **transition function**: $P(s' | s, a)$ is the probability of transitioning to state $s'$ after taking action $a$ in state $s$
- $R: \mathcal{S} \times \mathcal{A} \times \mathcal{S} \rightarrow \mathbb{R}$ — the **reward function**: $R(s, a, s')$ is the reward received after transitioning from $s$ via $a$ to $s'$
- $\gamma \in [0, 1)$ — the **discount factor**: how much to discount future rewards

Some formulations write $R(s, a)$ instead of $R(s, a, s')$ — the expected reward for taking action $a$ in state $s$, averaged over next states. Both are equivalent; the $(s, a, s')$ form is more general.

### The Markov Property

The Markov property is the single most important assumption in the MDP framework:

$$P(s_{t+1} | s_t, a_t, s_{t-1}, a_{t-1}, \ldots, s_0, a_0) = P(s_{t+1} | s_t, a_t)$$

In plain English: **the future depends only on the current state and action, not on how you got here.**

Given $s_t$, the entire history $(s_0, a_0, s_1, a_1, \ldots, s_{t-1}, a_{t-1})$ contains no additional information about where you'll end up. The current state is a sufficient statistic for the future.

This is why it's called "Markov" — it's Andrey Markov's property for stochastic processes. A Markov chain is a sequence of random variables where each depends only on the previous one.

**Why this property is essential:** Without it, the optimal action in state $s$ might depend on how you arrived at $s$. This would mean the policy $\pi: \mathcal{S} \rightarrow \mathcal{A}$ is insufficient — you'd need a policy that also conditions on history: $\pi: \mathcal{S}^* \rightarrow \mathcal{A}$. The state space grows exponentially with history length. Dynamic programming — and all the clean algorithms built on it — breaks.

**What counts as a valid state:** For the Markov property to hold, the state must capture everything relevant to future decisions. For a robot arm, joint positions alone are not Markovian — you also need joint velocities, because the same position with different velocity leads to different futures. Adding velocity to the state vector restores the Markov property. This is why state representations in robotics often include both positions and velocities.

### The Transition Function

$P(s' | s, a)$ is a conditional probability distribution. For each $(s, a)$ pair, it defines a probability distribution over next states. Key properties:

$$\sum_{s' \in \mathcal{S}} P(s' | s, a) = 1 \quad \forall s \in \mathcal{S}, a \in \mathcal{A}$$

The transition function encodes the **dynamics of the environment** — how the world responds to actions. For a robot arm, this is the physics: given current joint angles and velocities (state) and applied torques (action), what are the next joint angles and velocities? This is governed by the equations of motion.

In model-free RL (Q-learning, PPO), we never explicitly learn or use $P$ — we just sample from it by running in the environment. In model-based RL, we explicitly learn $\hat{P}$ from experience and use it for planning. Isaac Sim provides a differentiable physics engine that can act as a near-perfect model.

### The Reward Function

$R(s, a, s')$ maps every transition to a scalar reward. In practice, rewards are often:
- **Sparse:** $R = +1$ at goal, $0$ everywhere else. Easy to define correctly, hard to learn from (credit assignment problem).
- **Dense/shaped:** Reward at every step based on progress (e.g., distance to goal decreased by $\delta$ → reward $= \delta$). Easier to learn from, but requires careful design to avoid unintended behavior.
- **Mixed:** Sparse reward for task completion plus small dense penalties for energy use, joint limits violation, time elapsed.

The reward function embeds everything the designer wants. The fundamental theorem of MDP optimization (Bellman optimality) only cares about maximizing expected cumulative reward — it will find loopholes if they exist.

### Policies in the MDP Framework

A **deterministic policy** is $\pi: \mathcal{S} \rightarrow \mathcal{A}$.
A **stochastic policy** is $\pi: \mathcal{S} \rightarrow \Delta(\mathcal{A})$ (a probability distribution over actions for each state).

For a stochastic policy: $\pi(a | s)$ is the probability of taking action $a$ in state $s$.

An important result: for any finite MDP with a finite horizon or discount factor $\gamma < 1$, **there exists a deterministic optimal policy.** The randomness in stochastic policies is not needed to achieve optimality (though it helps with exploration during training).

### The Bellman Equations

The Bellman equations express value functions recursively. They are the core structural result of MDP theory.

**Bellman expectation equation** (for policy $\pi$):
$$V^\pi(s) = \sum_{a} \pi(a|s) \sum_{s'} P(s'|s,a) \left[R(s,a,s') + \gamma V^\pi(s')\right]$$

This says: the value of state $s$ under policy $\pi$ equals the expected immediate reward plus the discounted value of the next state, where both expectations are taken over the policy's action distribution and the environment's transition distribution.

**Bellman optimality equation** (for the optimal value function $V^*$):
$$V^*(s) = \max_{a} \sum_{s'} P(s'|s,a) \left[R(s,a,s') + \gamma V^*(s')\right]$$

This is a fixed-point equation — $V^*$ is the unique function satisfying it (under the discount factor ensuring contraction). The optimal policy is then:
$$\pi^*(s) = \arg\max_{a} \sum_{s'} P(s'|s,a) \left[R(s,a,s') + \gamma V^*(s')\right]$$

**Why these matter:** The Bellman equations decompose the intractable problem of maximizing infinite-horizon return into a tractable recursive relationship. Dynamic programming algorithms (value iteration, policy iteration) repeatedly apply these equations until convergence.

### Value Iteration

Value iteration directly applies the Bellman optimality equation as an update rule:

$$V_{k+1}(s) \leftarrow \max_{a} \sum_{s'} P(s'|s,a) \left[R(s,a,s') + \gamma V_k(s')\right]$$

Starting from $V_0 = 0$ (or any initialization), repeated application of this update converges to $V^*$. This convergence is guaranteed because the Bellman optimality operator is a **contraction mapping** (under the $\gamma < 1$ condition) — each application brings the estimates closer to the true $V^*$ by a factor of $\gamma$.

### Q-Values: The Action-Value Function

The **Q-function** $Q^\pi(s, a)$ is the expected return from taking action $a$ in state $s$ and then following policy $\pi$:

$$Q^\pi(s, a) = \sum_{s'} P(s'|s,a) \left[R(s,a,s') + \gamma V^\pi(s')\right]$$

The relationship $V^\pi(s) = \sum_a \pi(a|s) Q^\pi(s, a)$ ties Q-values and V-values together.

The optimal Q-function:
$$Q^*(s, a) = \sum_{s'} P(s'|s,a) \left[R(s,a,s') + \gamma \max_{a'} Q^*(s', a')\right]$$

Q-values are central to Q-learning algorithms (including deep Q-networks / DQN). If you know $Q^*$, the optimal policy is trivially $\pi^*(s) = \arg\max_a Q^*(s, a)$ — just take the action with the highest Q-value.

### Episodic vs. Continuing Tasks

**Episodic tasks** have a natural endpoint (terminal state). A robot arm task "pick up the block" ends when the block is picked up or when a time limit is reached. Episodes end, reset, and start fresh.

**Continuing tasks** run indefinitely (no terminal state). A robot maintaining balance on one leg, or a control system regulating temperature, never ends. For these, the discount factor is critical — without $\gamma < 1$, cumulative reward diverges.

## Intuition

Think of the MDP as a complete specification of the "game." The environment is the board; the state is the current board position; the action is your move; the transition function is the rules (how the board changes after your move); the reward is the score change. The discount factor is how much you prefer winning now over winning later.

The Markov property means: everything important about the game's history is already encoded in the current board position. You don't need to remember that you moved your queen 10 moves ago — the current position tells you everything. This is what makes chess MDPs tractable. (A game where the same position can have different futures depending on history — like repetition rules — requires augmenting the state to capture that history and restore the Markov property.)

The Bellman equations say: **a good state is one adjacent to other good states.** Your value is the reward you get now plus the value of where you'll end up. This recursive definition is what makes dynamic programming work — you propagate value backward from the goal state through the state space.

## Key Formula / Rule

**Bellman optimality equation:**

$$Q^*(s, a) = \mathbb{E}_{s'}\left[R(s,a,s') + \gamma \max_{a'} Q^*(s', a')\right]$$

**Q-learning update rule** (the core RL algorithm derived from this):

$$Q(s_t, a_t) \leftarrow Q(s_t, a_t) + \alpha \left[r_t + \gamma \max_{a'} Q(s_{t+1}, a') - Q(s_t, a_t)\right]$$

The term in brackets is the **TD error** (temporal difference error): how wrong the current Q-estimate is compared to what we just observed. This is the signal that drives learning.

```python
import numpy as np

def q_learning_update(Q, s, a, r, s_next, alpha=0.1, gamma=0.99):
    """Single Q-learning update. Q is a 2D array [state, action]."""
    td_target = r + gamma * np.max(Q[s_next])   # Bellman target
    td_error  = td_target - Q[s, a]              # how wrong we were
    Q[s, a]  += alpha * td_error                 # nudge Q toward target
    return Q
```

## Worked Example

**Problem:** Solve a 5-state chain MDP with Q-learning. States 0-4, goal is state 4 (reward +10). Action 0 = left, action 1 = right. Deterministic transitions. $\gamma = 0.9$.

```python
import numpy as np

# MDP parameters
n_states = 5
n_actions = 2  # 0=left, 1=right
goal = 4

def step(s, a):
    """Deterministic transition."""
    if a == 0:      # left
        s_next = max(0, s - 1)
    else:           # right
        s_next = min(n_states - 1, s + 1)
    reward = 10.0 if s_next == goal else 0.0
    done = (s_next == goal)
    return s_next, reward, done

# Q-learning
Q = np.zeros((n_states, n_actions))
alpha = 0.5
gamma = 0.9
epsilon = 0.3  # exploration rate

for episode in range(500):
    s = np.random.randint(0, goal)  # start anywhere except goal

    for step_num in range(50):
        # Epsilon-greedy action selection
        if np.random.rand() < epsilon:
            a = np.random.randint(n_actions)
        else:
            a = np.argmax(Q[s])

        s_next, r, done = step(s, a)

        # Q-learning update (Bellman equation as update rule)
        td_target = r + gamma * np.max(Q[s_next]) * (1 - done)
        Q[s, a] += alpha * (td_target - Q[s, a])

        s = s_next
        if done:
            break

print("Learned Q-table:")
print(Q.round(2))
print("\nLearned policy (0=left, 1=right):")
policy = np.argmax(Q[:goal], axis=1)  # don't print for goal state
print(policy)  # Should be [1, 1, 1, 1] — always go right!

# Verify values are decreasing with distance from goal:
# State 3 → right → goal (r=10) → V*(3) = 10
# State 2 → right → S3 → right → goal → V*(2) = γ*10 = 9
# State 1 → ... → V*(1) = γ²*10 = 8.1
# State 0 → ... → V*(0) = γ³*10 = 7.29
print("\nMax Q-values (approximate V*):", Q.max(axis=1).round(2))
```

The optimal policy emerges without being told "always go right" — Q-learning discovers it from scratch by repeatedly applying the Bellman update.

## Gotchas

**Gotcha 1 — The Markov property can fail silently.** If your state representation doesn't capture everything relevant to the future (e.g., joint positions but not velocities, game score but not player inventory), the Markov property is violated. The algorithm will still run, but it will learn a suboptimal policy or fail to converge. Always audit your state representation.

**Gotcha 2 — Q-learning does not require knowing $P$.** Q-learning is model-free: it learns Q-values directly from experience without explicitly estimating the transition function. This is useful but means you can only learn from states you've visited. Rare states require lots of exploration.

**Gotcha 3 — Tabular Q-learning doesn't scale.** If the state space has $10^6$ states and 50 actions, the Q-table has $5 \times 10^7$ entries. This is why deep RL replaces the table with a neural network — the network generalizes across similar states.

**Gotcha 4 — The discount factor affects learned behavior fundamentally.** $\gamma = 0$ makes the agent fully greedy (maximizes only immediate reward). $\gamma = 0.99$ makes it plan far ahead. Robotics tasks with long horizons need $\gamma$ close to 1. Setting it wrong means the agent won't plan far enough ahead to solve the task.

**Gotcha 5 — Off-policy vs. on-policy algorithms.** Q-learning is off-policy: it learns $Q^*$ regardless of what policy it uses to collect data. SARSA is on-policy: it learns the Q-values of the actual policy being followed. In continuing tasks with safety constraints, on-policy methods are safer because the policy you're evaluating is the one generating data.

**Gotcha 6 — Convergence requires visiting all state-action pairs.** Tabular Q-learning converges to $Q^*$ only if every $(s, a)$ pair is visited infinitely often. In practice, exploration must be sustained — epsilon should not decay to zero too early.

## See Also

- [[What is Reinforcement Learning]] — the conceptual framework this entry formalizes; MDPs are the math behind that intuition
- [[Probability and Random Variables]] — the transition function $P(s'|s,a)$ is a probability distribution; expected value in the Bellman equations requires probability
- [[Gradient Descent]] — policy gradient and actor-critic RL methods optimize MDP policies using gradient descent; understanding gradient descent is prerequisite
- [[Neural Networks - The Basics]] — deep RL (DQN, PPO, SAC) approximates Q-functions or policies with neural networks; the MDP provides the objective, the neural network provides the function approximator
- [[Linear Algebra]] — value iteration and policy evaluation involve solving large linear systems; the Bellman expectation equation is a linear equation in $V^\pi$
