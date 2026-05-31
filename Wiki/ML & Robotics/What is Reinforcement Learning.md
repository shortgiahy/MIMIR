# What is Reinforcement Learning

**One-liner:** Reinforcement learning is a framework for training an agent to make sequential decisions by rewarding it for good outcomes — learning entirely from experience, not labeled examples.

## Why It Exists

Supervised learning requires a dataset of (input, correct output) pairs. Somebody must have already solved the problem and labeled every example. For a robot arm learning to pick up objects, who provides those labels? The correct joint torques at every millisecond? In most robotics problems, the correct action is unknown in advance — that's the entire point of the robot.

Reinforcement learning handles a different class of problem: the agent must discover what to do through trial and error. Nobody hands it the answer. It acts, observes what happens, gets a signal telling it whether things got better or worse (the reward), and slowly learns to associate states and actions with long-term outcomes.

This is also closer to how biological intelligence develops. A child learning to walk doesn't receive labeled examples of correct muscle activations — it falls, recovers, falls less, and gradually builds a policy through pure experience. RL formalizes this kind of learning mathematically.

RL enables things no other ML paradigm can handle cleanly:
- **Game-playing:** AlphaGo, AlphaZero, OpenAI Five — RL agents that reached superhuman performance by playing against themselves with no human demonstration
- **Robotic control:** Learning locomotion gaits, manipulation policies, navigation in simulation before hardware deployment
- **Continuous control:** Managing systems where the action space is continuous (joint torques, motor currents) and the correct action depends on the entire history of the system

## The Concept

### The Agent-Environment Loop

RL is built on one core abstraction: the **agent-environment loop**.

At each discrete time step $t$:
1. The environment is in some **state** $s_t$
2. The agent observes the state (or a partial observation of it) and selects an **action** $a_t$
3. The agent receives a **reward** $r_t$ from the environment
4. The environment transitions to a new state $s_{t+1}$
5. Repeat

```
        ┌─────────┐  action a_t   ┌─────────────┐
        │  Agent  │ ─────────────▶│ Environment │
        │         │               │             │
        │         │◀─────────────│             │
        └─────────┘  state s_t+1  └─────────────┘
                     reward r_t
```

The agent's goal is to select actions that maximize cumulative reward over time — not just the immediate reward, but the long-term sum of rewards. This distinction is crucial. Many decisions look bad in the short term but are correct in the long term (sacrifice a chess piece to win the game, take a long detour to avoid an obstacle, spend time learning a skill).

### What Is a State?

The state $s_t$ is everything the agent needs to know about the environment to make a decision. In a robot arm reaching for an object, the state might include joint positions, joint velocities, object location, and gripper status. In an Atari video game, the state might be the raw pixel screen plus the last few frames (to encode velocity).

In practice, the agent rarely observes the true full state — it observes a **partial observation** $o_t$. Dealing with partial observability (POMDPs) is harder and requires the agent to maintain some internal memory or belief state. For this entry, assume we have full state access.

### What Is a Reward?

The reward $r_t \in \mathbb{R}$ is a scalar signal quantifying how good the agent's situation is at time $t$. It is the only feedback signal the agent receives.

This is profoundly important: **the reward signal encodes everything you want the agent to learn.** If you define reward incorrectly, the agent will learn the wrong thing — sometimes in ways that look correct on the reward but completely miss the intent. This is called **reward hacking** and it is a core challenge in RL.

Example: If a robot arm's reward is "minimize distance from end-effector to target," and you don't penalize collisions, the arm may learn to smash through obstacles. The reward signal was incomplete.

### What Is a Policy?

The **policy** $\pi$ is the agent's decision rule — a mapping from states to actions:

$$\pi: \mathcal{S} \rightarrow \mathcal{A}$$

A **deterministic policy** maps each state to a single action: $a = \pi(s)$.

A **stochastic policy** maps each state to a probability distribution over actions: $a \sim \pi(\cdot | s)$. This is often better in practice because randomness enables exploration — the agent occasionally tries suboptimal actions that might turn out to be better than expected.

The goal of RL is to find the optimal policy $\pi^*$ that maximizes expected cumulative reward.

### The Return: Cumulative Reward Over Time

The agent doesn't just maximize $r_t$ at the current step — it maximizes the **return** $G_t$, the total reward from step $t$ onward:

$$G_t = r_t + r_{t+1} + r_{t+2} + \cdots$$

But summing rewards infinitely causes problems if episodes never end (the sum diverges). The standard fix is **discounting**: future rewards are worth less than immediate rewards by a discount factor $\gamma \in [0, 1)$:

$$G_t = r_t + \gamma r_{t+1} + \gamma^2 r_{t+2} + \cdots = \sum_{k=0}^{\infty} \gamma^k r_{t+k}$$

When $\gamma = 0$: the agent is fully myopic — it only cares about immediate reward. When $\gamma \approx 1$: the agent cares a lot about the future. In robotics, $\gamma = 0.99$ is common — future rewards decay slowly, so the agent genuinely plans ahead.

### What Is a Value Function?

The **value function** $V^\pi(s)$ estimates the expected return from state $s$ when following policy $\pi$:

$$V^\pi(s) = \mathbb{E}_\pi\left[G_t \mid s_t = s\right] = \mathbb{E}_\pi\left[\sum_{k=0}^{\infty} \gamma^k r_{t+k} \mid s_t = s\right]$$

Intuitively: the value function asks "if I'm in state $s$ and follow policy $\pi$ from here, how much cumulative reward will I get on average?" States near the goal have high value; states far from the goal have low value.

The **action-value function** $Q^\pi(s, a)$ also conditions on the initial action:

$$Q^\pi(s, a) = \mathbb{E}_\pi\left[G_t \mid s_t = s, a_t = a\right]$$

This tells you: "how good is it to take action $a$ in state $s$ and then follow policy $\pi$ afterward?" This is what Q-learning algorithms (like DQN) learn.

### How These Pieces Connect

The full RL pipeline:
1. Initialize the policy (randomly, or with some prior knowledge)
2. Run the agent in the environment, collecting experience (state, action, reward, next state) tuples
3. Use those tuples to estimate how good each state-action pair is (estimate value functions)
4. Update the policy to take higher-value actions more often
5. Repeat — better policy generates better experience, which refines the value estimates, which improves the policy

This is the fundamental RL loop. Different algorithms (Q-learning, policy gradients, actor-critic) differ in exactly *how* steps 3 and 4 are implemented, but this structure is universal.

### Exploration vs. Exploitation

One of RL's core challenges: the agent must **exploit** what it has learned (take the actions it currently believes are best) but also **explore** (try new actions that might be better). If it never explores, it might get stuck in a suboptimal policy. If it never exploits, it never uses what it has learned.

The simplest tradeoff: **epsilon-greedy** — take the best known action with probability $1-\epsilon$, and a random action with probability $\epsilon$. Anneal $\epsilon$ from 1.0 (pure exploration at the start) to 0.01 (mostly exploitation after learning) over training.

## Intuition

Think of training a dog. You don't explain to the dog in advance what "sit" means — you give treats (positive reward) when the behavior approximates what you want, and gradually the dog learns to associate the action with the outcome.

The key difference from supervised learning: the dog isn't given the correct behavior as a label. It discovers it through trial, error, and the reward signal. It also learns *sequences* of behaviors — sit, then stay, then come — where each step's value depends on the whole chain.

RL captures this precisely. The agent (dog), environment (world), policy (current behavioral rules), reward (treat/no treat), value function (dog's expectation of treats given current situation), all map cleanly to the formal framework.

The tricky part RL handles that the dog analogy obscures: the **credit assignment problem**. When a robot succeeds at a task after 50 steps, which of those 50 actions deserve credit for the success? RL algorithms have to solve this, assigning estimated credit backward through time — and doing it correctly is most of the hard work.

## Key Formula / Rule

**Discounted return:**
$$G_t = \sum_{k=0}^{\infty} \gamma^k r_{t+k}$$

**Bellman equation** — the fundamental recursive relationship for value functions:
$$V^\pi(s) = \sum_a \pi(a|s) \sum_{s'} P(s'|s,a) \left[r(s,a,s') + \gamma V^\pi(s')\right]$$

This says: the value of state $s$ equals the expected immediate reward plus the discounted value of the next state, averaged over actions (weighted by policy) and transitions (weighted by environment dynamics).

```python
import numpy as np

def compute_return(rewards, gamma=0.99):
    """Compute discounted return from a list of rewards."""
    G = 0.0
    returns = []
    for r in reversed(rewards):      # work backwards in time
        G = r + gamma * G
        returns.insert(0, G)
    return returns

# Example: robot gets sparse reward of 1.0 only at step 5
rewards = [0.0, 0.0, 0.0, 0.0, 1.0]
G = compute_return(rewards, gamma=0.99)
print([f"{g:.4f}" for g in G])
# ['0.9606', '0.9702', '0.9801', '0.9900', '1.0000']
# The earlier steps receive discounted credit for the final reward.
```

## Worked Example

**Toy RL scenario — gridworld:**

Imagine a 4-state linear grid: `[S0] [S1] [S2] [S3=GOAL]`. The agent starts at S0, can move left or right, and gets reward +1 only when reaching S3.

```python
import numpy as np

# Simple value iteration: compute V*(s) without a policy
# Transition: move right with p=0.9, left with p=0.1 (slippery floor)
# Reward: +1 when entering S3

n_states = 4
goal = 3
gamma = 0.9

V = np.zeros(n_states)  # start with zero value estimates

def transition(s, action):
    """Returns [(prob, next_state, reward)]."""
    # action 0 = left, action 1 = right
    intended = s + 1 if action == 1 else s - 1
    slipped   = s - 1 if action == 1 else s + 1

    # clamp to grid boundaries
    intended = np.clip(intended, 0, n_states - 1)
    slipped  = np.clip(slipped,  0, n_states - 1)

    reward_intended = 1.0 if intended == goal else 0.0
    reward_slipped  = 1.0 if slipped  == goal else 0.0

    return [(0.9, intended, reward_intended), (0.1, slipped, reward_slipped)]

# Value iteration: repeatedly apply Bellman equation
for iteration in range(50):
    V_new = np.zeros(n_states)
    for s in range(n_states - 1):  # goal is absorbing
        # Take best action (greedy)
        action_values = []
        for a in range(2):
            av = sum(p * (r + gamma * V[s_next])
                     for p, s_next, r in transition(s, a))
            action_values.append(av)
        V_new[s] = max(action_values)
    V_new[goal] = 0.0  # absorbing state
    V = V_new

print("Optimal values:", V.round(3))
# [0.729, 0.810, 0.900, 0.000]
# States closer to the goal have higher value — intuitively correct.
```

States closer to the goal have higher value because they're more likely to reach it. This is value iteration — one of the foundational RL algorithms — solving the simplest possible RL problem.

## Gotchas

**Gotcha 1 — RL is not supervised learning with a reward.** The core distinction: in supervised learning, you always know the correct output. In RL, you only know whether the outcome was good or bad — and often only after a long sequence of actions. Do not conflate them.

**Gotcha 2 — Reward hacking is real and dangerous.** An RL agent will find any loophole in your reward function. A robot told to maximize speed may learn to vibrate in place (technically fast on some metric). Define rewards carefully, and test them with adversarial thinking: "how could an agent maximize this while doing nothing useful?"

**Gotcha 3 — Sample efficiency: RL needs a lot of data.** Training an RL agent from scratch can require millions of environment interactions. This is fine in simulation (Isaac Sim can run thousands of parallel environments), but impossible on real hardware. This is why sim-to-real transfer matters so much in robotics.

**Gotcha 4 — The credit assignment problem is hard.** Rewarding a robot for completing a task after 100 steps means the algorithm has to figure out which of those 100 actions was responsible for the success. Sparse rewards (only at the very end) make this hard. Dense rewards (at every step) help but require careful design.

**Gotcha 5 — Partial observability changes everything.** Most real robots cannot observe the full state. Sensors are noisy, cameras have limited field of view, joint encoders have resolution limits. When the state is only partially observable, a single observation is insufficient to make optimal decisions — you need to track history. This requires recurrent networks (LSTMs) or explicit state estimation (Kalman filters).

**Gotcha 6 — Exploration in continuous spaces is hard.** Epsilon-greedy (randomly pick a random discrete action) doesn't work well when actions are continuous (joint torques). You need continuous exploration noise — Ornstein-Uhlenbeck noise, Gaussian noise, or entropy regularization in the policy.

## See Also

- [[Markov Decision Processes]] — the formal mathematical framework underlying everything in this entry; RL is RL because of MDPs
- [[Gradient Descent]] — policy gradient methods literally apply gradient descent to optimize policy parameters; gradient descent trains the policy
- [[Neural Networks - The Basics]] — in deep RL, both the policy and value function are neural networks
- [[NumPy Arrays]] — all RL computations (value tables, transition matrices, reward buffers) are NumPy operations
- [[Probability and Random Variables]] — the stochastic transitions, stochastic policies, and expected values in RL all require probability theory
