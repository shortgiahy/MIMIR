# ML & Robotics Wiki — Index

> **Your goal:** Build Baymax, a home companion robot, as a portfolio anchor for MIT/Stanford transfer.
> **Your path:** Python → NumPy → Reinforcement Learning → Isaac Sim
> **Current position:** Early. Solid Python fundamentals, learning NumPy now.

---

## Baymax Learning Roadmap

```
Stage 1: Python Foundations
└── You know the basics. Deepen your NumPy.

Stage 2: Linear Algebra (Math wiki)
└── Vectors, matrices, dot products, eigenvalues
└── This is the language ML is written in.

Stage 3: Machine Learning Basics  ← You are here
└── NumPy Arrays (done)
└── Gradient Descent
└── Neural Networks - The Basics

Stage 4: Reinforcement Learning
└── What is Reinforcement Learning
└── Markov Decision Processes
└── (Next: Policy Gradients, Actor-Critic, PPO)

Stage 5: Isaac Sim + Deployment
└── Sim-to-real transfer
└── ROS 2 integration  
└── Physics simulation and domain randomization
```

**Milestone check:** When you can implement a simple policy gradient algorithm on a custom gymnasium environment, you're ready to start Isaac Sim. That's your gate.

---

## Cluster 1 — Python Foundations

These exist in the CS wiki but connect deeply to everything here.

| Entry | What It Is |
|---|---|
| [[Python Data Structures]] | Lists, dicts, sets — understand these before NumPy can make sense |
| [[Classes and Objects]] | NumPy arrays and PyTorch tensors are objects; you need OOP to understand ML libraries |

---

## Cluster 2 — Linear Algebra

These live in the Math wiki. Do not skip them — ML is applied linear algebra.

| Entry | What It Is |
|---|---|
| [[Vectors and Dot Products]] | The math behind `np.dot`, inner products, projections — foundational |
| [[Matrix Multiplication]] | What `A @ B` actually computes and why; the core operation in every neural network |
| [[Eigenvalues and Eigenvectors]] | How covariance matrices decompose; the math behind PCA and many optimization methods |

---

## Cluster 3 — Calculus for ML

| Entry | What It Is |
|---|---|
| [[Partial Derivatives and the Gradient]] | The gradient $\nabla L$ is what gradient descent follows — you must understand this |
| [[Chain Rule]] | Backpropagation is the chain rule; without this, neural net training is a black box |
| [[Integration by Parts]] | Relevant for probability distributions and some RL derivations; less urgent than derivatives |

---

## Cluster 4 — Machine Learning Basics

The core ML concepts before RL. Start here after linear algebra.

| Entry | What It Is |
|---|---|
| [[NumPy Arrays]] | The `ndarray`, vectorization, broadcasting, indexing — the substrate for all numerical ML |
| [[Gradient Descent]] | How models are trained; the update rule $\theta \leftarrow \theta - \alpha \nabla L$; why it works |
| [[Neural Networks - The Basics]] | What a neuron computes, layers, forward pass, why depth matters, connection to gradient descent |

---

## Cluster 5 — Reinforcement Learning

Build on ML Basics. RL is the core of Baymax's decision-making.

| Entry | What It Is |
|---|---|
| [[What is Reinforcement Learning]] | The agent-environment loop, reward, policy, value function — the full framework, conceptually |
| [[Markov Decision Processes]] | The formal math behind RL: states, actions, transitions, Bellman equations, Q-values |
| [[Policy Gradients]] *(stub)* | Gradient-based policy optimization; the foundation of PPO, the algorithm you'll use in Isaac Sim |
| [[Actor-Critic Methods]] *(stub)* | Combines value functions (critic) with direct policy learning (actor); most modern RL uses this |
| [[Proximal Policy Optimization]] *(stub)* | The practical state-of-the-art on-policy RL algorithm; what Isaac Sim tutorials use by default |

---

## Cluster 6 — Robotics Simulation

Where everything comes together. You're building toward this.

| Entry | What It Is |
|---|---|
| [[Isaac Sim Overview]] *(stub)* | NVIDIA's GPU-accelerated robotics simulator; runs on your RTX 3060 Super |
| [[Sim-to-Real Transfer]] *(stub)* | How to train in simulation and deploy on real hardware without catastrophic failure |
| [[Kinematics and Inverse Kinematics]] *(stub)* | How joint angles map to end-effector position; the math for arm control |
| [[Robot Operating System (ROS 2)]] *(stub)* | The middleware standard for robotics communication; connects simulation to hardware |

---

## Reading Order for Baymax v0.1 (Software-Only Phase)

If you're starting from zero with just Python, here is the sequence:

1. [[Python Data Structures]] + [[Classes and Objects]] — CS fundamentals
2. [[Vectors and Dot Products]] + [[Matrix Multiplication]] — Math foundation
3. [[Partial Derivatives and the Gradient]] + [[Chain Rule]] — Calculus for ML
4. [[NumPy Arrays]] — The numerical computing layer
5. [[Gradient Descent]] — How everything gets trained
6. [[Neural Networks - The Basics]] — The function approximator
7. [[What is Reinforcement Learning]] — The decision-making framework
8. [[Markov Decision Processes]] — The formal math
9. Policy Gradients → Actor-Critic → PPO (write these next)
10. Isaac Sim setup + first training run

Each entry has a **See Also** section with cross-links. Follow those when something is unclear — they point to the exact prerequisite concept that will resolve the confusion.

---

## Stub Convention

Entries marked *(stub)* don't exist yet. They're placeholders for the next phase of the wiki. When you're ready to learn them, ask MIMIR to fill them in. Start writing requests like: "MIMIR, expand the Policy Gradients stub — I just finished MDPs and need to understand REINFORCE before PPO."

---

*Index maintained by MIMIR. Last updated: 2026-05-31.*
