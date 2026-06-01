# Markov Property

**One-liner:** The future depends only on the present state, not on any history of how you got there.

## Core Idea
$$P(s_{t+1} \mid s_t, a_t, s_{t-1}, a_{t-1}, \ldots, s_0, a_0) = P(s_{t+1} \mid s_t, a_t)$$
The next state is fully determined by the current state and action alone. The entire history collapses into a single number (the state). This is what makes reinforcement learning mathematically tractable.

## Why It Exists
Without the Markov property, an agent would need to remember its entire history to make optimal decisions — the state space becomes infinite. The property is a modeling choice: if you encode the right information in the state, the past is redundant. The challenge in real robotics is designing states that actually satisfy this.

## Real-World Applications
- **Chess engines**: the board position is the full state — past moves don't matter once you have the current board
- **Robot navigation**: if state = (x, y, heading, velocity), Markov holds; if state = (x, y) only, it doesn't — velocity matters for where you'll be next
- **Baymax obstacle avoidance**: the state vector must include velocity, not just position
- **All of modern RL**: Q-Learning, PPO, DQN — every standard algorithm assumes the Markov property holds

## Intuition
Think of a GPS. It doesn't need your full driving history to give you the optimal route — your current location is enough. The Markov property says the state is exactly that: a sufficient statistic for the future. If you know where you are, you don't need to know how you got there.

The violation: imagine a maze where a door only opens if you previously visited room A, then B. The current room alone isn't sufficient — history matters. You'd need to encode "visited A then B" into the state to restore the Markov property.

## Derivation
A stochastic process $(S_t)$ is Markov if and only if:
$$P(S_{t+1} = s' \mid S_0, \ldots, S_t) = P(S_{t+1} = s' \mid S_t)$$

**Why this enables dynamic programming:**
The value function $V^\pi(s) = \mathbb{E}[G_t \mid S_t = s]$ only depends on $s$, not the trajectory to reach $s$. Without Markov, $V$ would need to be defined over histories — infinite state space, intractable.

**Restoring a violated Markov property:**
Stack the last $k$ observations. A single camera frame doesn't encode velocity, but 4 consecutive frames do. This is exactly how Atari DQN works.

## Worked Example
```python
# Non-Markov state: position only
state = (robot.x, robot.y)
# Next position depends on current velocity — not captured
# Markov property violated: agent will fail to learn dynamics

# Markov state: position + velocity
state = (robot.x, robot.y, robot.vx, robot.vy)
# Next state fully determined by state + action
# Markov property holds

# Frame stacking to restore Markov in visual RL
from collections import deque
import numpy as np

class FrameStack:
    def __init__(self, k=4):
        self.k = k
        self.frames = deque(maxlen=k)

    def push(self, frame):
        self.frames.append(frame)

    def get_state(self):
        return np.stack(self.frames)  # shape: (k, H, W)
```

## See Also
- [[Markov Decision Process]] — the full RL framework built on this property
- [[State]] — designing states that satisfy the Markov property
- [[Value Function]] — only well-defined because of the Markov property
- [[Bellman Equation]] — recursive structure is only valid under Markov
- [[Environment]] — the transition function $P(s'|s,a)$ assumes Markov holds
