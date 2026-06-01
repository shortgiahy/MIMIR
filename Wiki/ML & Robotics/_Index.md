# ML & Robotics — Master Index

> **Goal:** Build Baymax — a home robot capable of navigation, obstacle avoidance, and eventually manipulation — while building the mathematical and algorithmic foundations to apply for MIT/Stanford.

---

## Baymax Learning Roadmap

```
Python → Linear Algebra → NumPy → Optimization → RL → Neural Nets → Isaac Sim
  │            │              │           │          │         │            │
  │       Scalar/Vector   NumPy Array  Loss Fn   MDP/RL   Neurons    Simulation
  │       Matrix/Dot Prod  Vectorize  Gradient  Q-Learn   Backprop   & Transfer
  └───────────────────────────────────────────────────────────────────────────►
                                                             Baymax v0.1 ready
```

**Recommended reading order for Baymax v0.1 (navigation + obstacle avoidance):**

1. [[Scalar]] → [[Vector]] → [[Matrix]] → [[Dot Product]] → [[Matrix Multiplication]]
2. [[NumPy Array]] → [[Vectorization]] → [[Broadcasting]]
3. [[Loss Function]] → [[Gradient]] → [[Gradient Descent]]
4. [[Markov Property]] → [[State]] → [[Action]] → [[Reward]] → [[Policy]]
5. [[Agent]] → [[Environment]] → [[Value Function]]
6. [[Markov Decision Process]] → [[Bellman Equation]] → [[Q-Learning]]
7. [[Artificial Neuron]] → [[Activation Function]] → [[Neural Network]]
8. [[Forward Pass]] → [[Backpropagation]]
9. Deploy in Isaac Sim, transfer to Baymax hardware.

---

## All Entries by Cluster

### Linear Algebra Foundations

| Entry | One-sentence description |
|---|---|
| [[Scalar]] | A single real number — the atomic quantity all other structures are built from. |
| [[Vector]] | An ordered list of scalars representing a direction and magnitude in space. |
| [[Matrix]] | A 2D grid of scalars that represents a linear transformation between vector spaces. |
| [[Dot Product]] | The scalar sum of element-wise products of two vectors, measuring their alignment. |
| [[Matrix Multiplication]] | A composition of linear transformations computed as a grid of dot products. |

### NumPy

| Entry | One-sentence description |
|---|---|
| [[NumPy Array]] | Python's N-dimensional array object — the in-memory representation of vectors and matrices used by every ML library. |
| [[Vectorization]] | Replacing explicit Python loops with array-level operations so that NumPy runs optimised C/BLAS code under the hood. |
| [[Broadcasting]] | NumPy's rule for implicitly expanding smaller arrays to match the shape of larger ones during element-wise operations. |

### Optimization

| Entry | One-sentence description |
|---|---|
| [[Loss Function]] | A scalar that measures how wrong the model's current predictions are, giving gradient descent its target. |
| [[Gradient]] | The vector of all partial derivatives of the loss with respect to every parameter, pointing in the direction of steepest increase. |
| [[Gradient Descent]] | An iterative optimisation algorithm that moves parameters in the negative gradient direction to minimise the loss. |
| [[Learning Rate]] | The scalar step-size hyperparameter that controls how far gradient descent moves on each update. |
| [[Stochastic Gradient Descent]] | A variant of gradient descent that estimates the gradient from a random mini-batch rather than the full dataset, enabling training on large datasets. |

### Reinforcement Learning

| Entry | One-sentence description |
|---|---|
| [[Agent]] | The decision-making entity that observes the environment, selects actions, and receives rewards. |
| [[Environment]] | Everything outside the agent — it receives actions, transitions state, and emits observations and rewards. |
| [[State]] | A complete description of the environment at one moment, sufficient to predict all future behaviour. |
| [[Action]] | The choice an agent makes at each timestep, drawn from a discrete or continuous action space. |
| [[Reward]] | A scalar signal the environment sends to the agent after each action, encoding what "good behaviour" means. |
| [[Policy]] | A mapping from states to actions (deterministic) or distributions over actions (stochastic) that defines the agent's behaviour. |
| [[Value Function]] | V(s) — the expected cumulative discounted reward starting from state s under a given policy. |
| [[Markov Property]] | The assumption that the future depends only on the present state, not on the full history — the mathematical foundation of all MDP methods. |
| [[Markov Decision Process]] | The formal 5-tuple (S, A, P, R, γ) that encodes every element of a sequential decision problem — the framework underlying all modern RL. |
| [[Bellman Equation]] | The recursive equation V*(s) = max_a[R(s,a) + γ Σ P(s'\|s,a) V*(s')] that collapses infinite-horizon planning into a one-step look-ahead. |
| [[Q-Learning]] | A model-free, off-policy algorithm that learns Q*(s,a) from sampled transitions without ever knowing the transition probabilities. |

### Neural Networks

| Entry | One-sentence description |
|---|---|
| [[Artificial Neuron]] | A single computational unit that computes a weighted sum of its inputs plus a bias, then applies a nonlinear activation function. |
| [[Activation Function]] | The nonlinear function (ReLU, sigmoid, tanh, softmax) applied after the linear part of a neuron — without it, stacked layers collapse to a single linear map. |
| [[Neural Network]] | A layered composition of artificial neurons that can approximate any continuous function, trained end-to-end by gradient descent. |
| [[Forward Pass]] | The left-to-right computation that flows input data through every layer of a neural network to produce a prediction. |
| [[Backpropagation]] | The algorithm that applies the chain rule layer by layer in reverse to compute the gradient of the loss with respect to every weight in one backward pass. |

---

## Cross-Subject Links

| ML & Robotics concept | Links to Math | Links to CS |
|---|---|---|
| [[Gradient]] | Chain Rule, Partial Derivative | — |
| [[Gradient Descent]] | Taylor Series, Convergence | — |
| [[Backpropagation]] | Chain Rule | — |
| [[Matrix Multiplication]] | Dot Product, Vector, Matrix | — |
| [[NumPy Array]] | Vector, Matrix, Scalar | [[Class]], [[Object]], [[Instance]] |
| [[Vectorization]] | Matrix Multiplication | [[Class]] |
| [[Broadcasting]] | Scalar, Vector | — |
| [[Neural Network]] | Matrix, Dot Product | [[Class]], [[Interface]] |
| [[Forward Pass]] | Matrix Multiplication | — |
| [[Markov Decision Process]] | Convergence, Series | — |
| [[Q-Learning]] | Convergence, Geometric Series | — |

---

## Recommended Reading Order — Baymax v0.1

> Target capability: laser-scan-based obstacle avoidance + goal navigation in a known map, running on-robot.

**Phase 1 — Mathematical backbone (2–3 weeks)**
1. [[Scalar]], [[Vector]], [[Matrix]]
2. [[Dot Product]], [[Matrix Multiplication]]
3. [[NumPy Array]], [[Vectorization]], [[Broadcasting]]
4. [[Loss Function]], [[Gradient]], [[Gradient Descent]]

**Phase 2 — Reinforcement Learning (2–3 weeks)**
5. [[Markov Property]], [[State]], [[Action]], [[Reward]], [[Policy]], [[Value Function]]
6. [[Agent]], [[Environment]]
7. [[Markov Decision Process]]
8. [[Bellman Equation]]
9. [[Q-Learning]] (tabular first, then DQN sketch)

**Phase 3 — Neural Networks (2–3 weeks)**
10. [[Artificial Neuron]]
11. [[Activation Function]]
12. [[Neural Network]]
13. [[Forward Pass]]
14. [[Backpropagation]]

**Phase 4 — Isaac Sim integration**
- Implement tabular Q-learning in a grid-world sim.
- Replace Q-table with a 3-layer MLP (see [[Neural Network]] worked example).
- Train with [[Backpropagation]] + [[Gradient Descent]] (Adam optimiser).
- Transfer to Baymax hardware.

---

*Last updated: 2026-06-01*
