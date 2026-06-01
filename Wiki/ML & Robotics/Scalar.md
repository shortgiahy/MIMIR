# Scalar

**One-liner:** A single real number — the zero-dimensional building block of all mathematical objects in ML.

## Core Idea
$$s \in \mathbb{R}$$
A scalar is just a number: `5`, `-3.14`, `0.001`. It has magnitude but no direction. Everything more complex — vectors, matrices, tensors — is built by organizing scalars into structures.

## Why It Exists
Before you can do any math, you need atoms. Scalars are those atoms. Every element of a vector, every entry of a matrix, every weight in a neural network, every reward signal in RL — ultimately resolves to a scalar. The concept matters because it clarifies *dimensionality*: when a loss function returns a scalar, that's intentional — it collapses all error information into one number you can differentiate against.

## Real-World Applications
- The **loss** at any training step is a scalar (e.g., `loss = 0.034`). The whole goal of training is to minimize that one number.
- The **reward** signal in RL is a scalar — Baymax's controller receives `r = +1` for reaching a goal, `r = -0.01` per step for efficiency.
- **Temperature** in softmax is a scalar that controls "sharpness" of probability distributions.
- A **learning rate** like `α = 0.001` is a scalar multiplier.

## Intuition
Think of a scalar as a point on the number line — no up/down, no left/right. When you multiply a [[Vector]] by a scalar, you stretch or shrink it (and flip it if negative) but you don't rotate it. That's the defining mechanical property: scalars scale without orienting.

## Derivation
There's no derivation — a scalar is a primitive. What matters is understanding how scalars interact with higher-dimensional objects:

**Scalar × Vector:**
$$c \cdot \mathbf{v} = c \cdot \begin{bmatrix} v_1 \\ v_2 \\ v_3 \end{bmatrix} = \begin{bmatrix} c \cdot v_1 \\ c \cdot v_2 \\ c \cdot v_3 \end{bmatrix}$$

**Scalar in a loss function:** the [[Gradient]] ∇L is a vector, but L itself must be a scalar — because you can only ask "is this number bigger or smaller?" not "is this vector bigger or smaller?"

## Worked Example
```python
import numpy as np

# A scalar in NumPy — 0-dimensional array
s = np.float64(3.14)
print(s.shape)   # ()    <-- zero dimensions
print(s.ndim)    # 0

# Scalar multiplication of a vector
v = np.array([1.0, 2.0, 3.0])
alpha = 0.5   # scalar learning rate
print(alpha * v)  # [0.5, 1.0, 1.5]

# Loss functions return scalars
y_true = np.array([1.0, 0.0, 1.0])
y_pred = np.array([0.9, 0.1, 0.8])
mse = np.mean((y_true - y_pred) ** 2)  # one number
print(f"MSE loss: {mse:.4f}")  # MSE loss: 0.0067
```

## See Also
- [[Vector]] — a 1D ordered collection of scalars; the next dimension up
- [[Matrix]] — a 2D array; scalars are 0D, vectors 1D, matrices 2D
- [[Loss Function]] — always returns a scalar; the thing we minimize
- [[Reward]] — RL's scalar feedback signal at each timestep
- [[Learning Rate]] — the scalar α in gradient descent
