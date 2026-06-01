# Loss Function

**One-liner:** A scalar measure of how wrong the model's predictions are — the single number that gradient descent minimizes during training.

## Core Idea
$$L(\theta) = \frac{1}{N}\sum_{i=1}^{N} \ell(f_\theta(x_i),\ y_i)$$
A loss function takes (predictions, ground-truth labels) and returns a non-negative [[Scalar]]. Smaller is better; zero is perfect. The model's parameters $\theta$ are the only things we can change, so we compute $\nabla_\theta L$ and update $\theta$ to reduce $L$. Everything in supervised ML is optimization of a loss function.

## Why It Exists
You need one number to optimize. A model might output a 1000-class probability distribution — you can't do [[Gradient Descent]] on a vector. The loss function collapses all the error information into a differentiable scalar that tells you: "this is how wrong you are, and here's a gradient pointing toward less wrong."

## Real-World Applications
- **Regression (predicting robot joint torques):** Mean Squared Error: $L = \frac{1}{N}\sum(y_i - \hat{y}_i)^2$
- **Classification (recognizing objects for Baymax):** Cross-entropy: $L = -\sum_i y_i \log \hat{y}_i$
- **RL policy gradient:** the "loss" is negative expected reward — we minimize $-\mathbb{E}[R]$
- **Diffusion models:** MSE between predicted and actual noise at each denoising step
- **ChatGPT pretraining:** cross-entropy loss over next-token prediction on trillions of tokens

## Intuition
The loss function is the *terrain* that gradient descent explores. The model lives at a point in parameter space (every weight is one coordinate). The loss function assigns an elevation to every point. Training is finding the lowest valley. The specific choice of loss function shapes the terrain — cross-entropy creates gentler gradients than squared error for probabilities, making classification training more stable.

A crucial property: **differentiability**. The loss must be differentiable with respect to model parameters so we can compute $\nabla_\theta L$. This rules out "0/1 accuracy" as a loss (not differentiable at errors) — that's why we use cross-entropy as a surrogate.

## Derivation
**Mean Squared Error (MSE):**
$$L_{MSE} = \frac{1}{N}\sum_{i=1}^N (y_i - \hat{y}_i)^2$$
Gradient w.r.t. prediction $\hat{y}_i$:
$$\frac{\partial L}{\partial \hat{y}_i} = -\frac{2}{N}(y_i - \hat{y}_i)$$
Points toward $y_i$ (increase prediction if $\hat{y}_i < y_i$, decrease if $\hat{y}_i > y_i$). ✓

**Binary Cross-Entropy:**
$$L_{BCE} = -\frac{1}{N}\sum_{i=1}^N \left[y_i \log(\hat{y}_i) + (1 - y_i)\log(1 - \hat{y}_i)\right]$$
When $y_i = 1$ and $\hat{y}_i \approx 0$: $-\log(0.001) \approx 6.9$ — very high penalty for confident wrong predictions. When $\hat{y}_i \approx 1$: $-\log(1) = 0$ — no penalty. This logarithmic shape gives steeper gradients far from the correct answer, which helps convergence.

**Why cross-entropy for classification:** maximizing likelihood of correct class = minimizing negative log-likelihood = minimizing cross-entropy. It's statistically principled.

## Worked Example
```python
import numpy as np

# ── MSE Loss ─────────────────────────────────────────────
def mse_loss(y_true, y_pred):
    return np.mean((y_true - y_pred) ** 2)

# Predicting robot joint angle (radians)
y_true = np.array([1.57, 0.78, 2.36, 0.0])  # target angles
y_pred = np.array([1.50, 0.90, 2.30, 0.05]) # predicted angles
print(f"MSE: {mse_loss(y_true, y_pred):.4f}")  # 0.0030

# ── Cross-Entropy Loss ────────────────────────────────────
def cross_entropy_loss(y_true_onehot, y_pred_probs):
    # y_pred_probs should be softmax output (sums to 1)
    eps = 1e-10  # prevent log(0)
    return -np.mean(np.sum(y_true_onehot * np.log(y_pred_probs + eps), axis=1))

# 3-class classification (e.g., object type for Baymax)
y_true = np.array([[1, 0, 0],    # class 0: door
                   [0, 1, 0],    # class 1: person
                   [0, 0, 1]])   # class 2: obstacle

# Good predictions (confident and correct)
y_pred_good = np.array([[0.9, 0.05, 0.05],
                        [0.1, 0.8,  0.1 ],
                        [0.05, 0.05, 0.9]])
# Bad predictions (confident and wrong)
y_pred_bad  = np.array([[0.05, 0.9, 0.05],
                        [0.8,  0.1, 0.1 ],
                        [0.9,  0.05, 0.05]])

print(f"Good predictions loss: {cross_entropy_loss(y_true, y_pred_good):.4f}")  # ~0.15
print(f"Bad predictions loss:  {cross_entropy_loss(y_true, y_pred_bad):.4f}")   # ~2.3

# ── Why the loss must be scalar ───────────────────────────
# Gradient descent needs one number to minimize.
# A per-example loss vector would require vector optimization (undefined).
per_example = (y_pred_good - [[0.9,0,0],[0,0.8,0],[0,0,0.9]]) ** 2
print(f"Per-example losses shape: {per_example.shape}")  # (3, 3) — can't optimize this
scalar_loss = np.mean(per_example)
print(f"Scalar loss: {scalar_loss:.6f}")  # one number ✓
```

## See Also
- [[Gradient]] — $\nabla_\theta L$ is what we compute from the loss
- [[Gradient Descent]] — uses the gradient of the loss to update parameters
- [[Backpropagation]] — algorithm that computes $\nabla_\theta L$ through the network
- [[Neural Network]] — the model $f_\theta$ whose output the loss evaluates
- [[Scalar]] — the loss function must return a scalar
- [[Reward]] — in RL, reward is the signal we maximize (negative loss perspective)
