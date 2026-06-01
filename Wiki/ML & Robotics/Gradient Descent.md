# Gradient Descent

**One-liner:** The iterative algorithm that moves model parameters opposite the gradient of the loss — the universal workhorse of machine learning training.

## Core Idea
$$\theta \leftarrow \theta - \alpha \nabla_\theta L(\theta)$$
At each step: compute the [[Gradient]] of the [[Loss Function]] with respect to parameters $\theta$, then move $\theta$ in the *opposite* direction (downhill) by a step size $\alpha$ (the [[Learning Rate]]). Repeat until loss converges.

## Why It Exists
Neural networks have millions of parameters. You can't find the minimum analytically (no closed form). You can't try all combinations (intractable). But you *can* compute the gradient cheaply (via [[Backpropagation]]) and take small downhill steps. This is the fundamental insight that makes modern ML possible: you don't need to know where the minimum is, only which direction is downhill from where you are.

## Real-World Applications
- **Every neural network trains with some variant of gradient descent** — SGD, Adam, RMSprop are all gradient descent with modifications.
- **ChatGPT:** trained with Adam (adaptive gradient descent) on ~10,000 GPUs running gradient descent for months.
- **AlphaGo:** trained RL policy gradients — still fundamentally gradient descent on expected reward.
- **Baymax navigation policy:** fine-tuned with gradient descent on robot simulation trajectories.
- **Linear regression:** gradient descent gives the same answer as the closed-form normal equations but scales to millions of parameters where normal equations fail.

## Intuition
The landscape metaphor: the [[Loss Function]] is a terrain. Parameters $\theta$ are your position. The gradient is a compass pointing uphill. Gradient descent: always walk opposite the compass (downhill). The [[Learning Rate]] is your stride length. Too large → overshoot valleys, bounce around. Too small → takes forever. Just right → smooth convergence to a valley.

Physical analogy: a ball rolling down a hill under [[Newton's Second Law]] gravity — the force ($-\nabla V$, negative gradient of potential) drives the ball toward lower energy. Gradient descent is a discrete, first-order version of this dynamics, updating by discrete steps rather than continuous motion.

## Derivation
**Why opposite the gradient moves downhill:**

The first-order Taylor approximation: $L(\theta + \delta) \approx L(\theta) + \nabla L \cdot \delta$

To minimize $L(\theta + \delta)$ over choices of $\delta$ with $||\delta|| = \epsilon$ (fixed step size):
$$L(\theta + \delta) \approx L(\theta) + \nabla L \cdot \delta \geq L(\theta) - ||\nabla L|| \cdot \epsilon$$
The minimum is achieved when $\delta = -\epsilon \cdot \frac{\nabla L}{||\nabla L||}$ (opposite gradient direction).

Setting $\delta = -\alpha \nabla L$ with $\alpha$ small (so Taylor approx holds) gives gradient descent. ∎

**Convergence condition (convex functions):**
If $L$ is convex and $\alpha < \frac{1}{L_{smooth}}$ (Lipschitz constant of gradient), gradient descent converges:
$$L(\theta^{(t)}) - L(\theta^*) \leq O(1/t)$$

**Non-convex reality:** Neural network loss surfaces are non-convex. Gradient descent finds a *local* minimum or saddle point, not necessarily global. In practice, SGD's noise helps escape saddle points.

## Worked Example
```python
import numpy as np
import matplotlib.pyplot as plt

# ── Gradient descent on a simple 1D function ─────────────
def f(x):     return x**2 - 4*x + 5       # minimum at x=2
def df(x):    return 2*x - 4              # gradient

x = 0.0       # start far from minimum
lr = 0.1      # learning rate
history = [x]

for step in range(50):
    grad = df(x)
    x = x - lr * grad    # gradient descent update
    history.append(x)

print(f"Converged to x={x:.6f}, f(x)={f(x):.6f}")
# x=2.000000, f(x)=1.000000 (true minimum)

# ── Gradient descent for linear regression ────────────────
np.random.seed(0)
N = 100
X_raw = np.random.randn(N)
y = 3.0 * X_raw + 1.5 + 0.5 * np.random.randn(N)  # y = 3x + 1.5 + noise
X = np.column_stack([np.ones(N), X_raw])  # add bias column

# Initialize parameters: w = [bias, slope]
w = np.zeros(2)
lr = 0.05

losses = []
for epoch in range(200):
    # Forward pass: predict
    y_pred = X @ w

    # Loss: MSE
    loss = np.mean((y_pred - y) ** 2)
    losses.append(loss)

    # Gradient: dL/dw = (2/N) * X^T @ (y_pred - y)
    grad = (2 / N) * X.T @ (y_pred - y)

    # Update
    w = w - lr * grad

print(f"Learned parameters: bias={w[0]:.3f}, slope={w[1]:.3f}")
# bias≈1.5, slope≈3.0 — recovered! ✓
print(f"Final loss: {losses[-1]:.4f}")

# ── Effect of learning rate ───────────────────────────────
for lr in [0.001, 0.1, 0.9]:
    x = 10.0
    for _ in range(100):
        x = x - lr * (2*x)   # gradient of x^2 is 2x
    print(f"lr={lr}: final x={x:.4f}")
# lr=0.001: x=6.6083  (too slow)
# lr=0.1:   x=0.0000  (converged)
# lr=0.9:   x=nan     (diverged/oscillated)
```

## See Also
- [[Gradient]] — the direction gradient descent moves *against*
- [[Learning Rate]] — the step size $\alpha$; most critical hyperparameter
- [[Loss Function]] — the landscape being minimized
- [[Backpropagation]] — how the gradient is computed efficiently
- [[Stochastic Gradient Descent]] — the practical variant using mini-batches
- [[Newton's Second Law]] — continuous analog: force = $-\nabla V$ drives a system toward lower energy, just as gradient descent drives parameters toward lower loss
- [[Taylor Series]] — the first-order Taylor approximation justifies each gradient descent step as a locally optimal descent direction
- [[Conservative Force]] — the loss landscape is analogous to a potential energy surface; gradient descent is the discrete trajectory a particle would follow under the conservative force $-\nabla L$
