# Stochastic Gradient Descent

**One-liner:** A variant of gradient descent that estimates the gradient from a random mini-batch of data rather than the full dataset, trading exactness for speed and beneficial noise.

## Core Idea
$$\theta \leftarrow \theta - \alpha \cdot \frac{1}{|\mathcal{B}|} \sum_{i \in \mathcal{B}} \nabla_\theta \mathcal{L}_i(\theta)$$
Instead of summing gradients over all N training examples, SGD samples a mini-batch B (typically 32–512 examples), computes a noisy gradient estimate, and takes a step. One pass through all the data (N/|B| steps) is called an epoch. The estimate is unbiased: E[gradient of batch] = true gradient.

## Why It Exists
Computing the exact gradient over millions of examples is prohibitively slow — one weight update on ImageNet with full-batch GD would take minutes per step. SGD makes each step cheap (milliseconds) and lets the model see many updates per second. Counterintuitively, the noise introduced by using mini-batches is not purely harmful: it acts as implicit regularization and helps the optimizer escape sharp local minima that would trap full-batch GD.

## Real-World Applications
- **Training GPT / LLMs:** All large language models are trained with Adam (an SGD variant) with batch sizes in the thousands — the stochastic noise at scale is carefully managed via gradient clipping.
- **AlphaGo:** Policy and value networks were trained with SGD on self-play game data sampled in mini-batches from a replay buffer.
- **RLHF (ChatGPT):** The reward model and the RL policy update both use Adam (adaptive SGD) — stochasticity comes from both data sampling and environment interactions.
- **Robot locomotion (Isaac Sim):** PPO collects rollouts from thousands of parallel environments and runs multiple SGD epochs over the collected mini-batches to update the policy.
- **Baymax:** On-device fine-tuning of a navigation policy after deployment uses SGD with a small batch of recent sensor readings — full-dataset retraining would be too slow.

## Intuition
Full-batch [[Gradient Descent]] is like averaging the opinion of every person in a stadium before taking one step. SGD is like asking 64 random people and moving now. You'll sometimes go slightly the wrong direction, but you move thousands of times before full-batch takes one step.

The noise matters for two reasons:
1. **Escaping sharp minima:** Flat minima generalize better. SGD noise randomly kicks the optimizer out of sharp narrow valleys and tends to settle in flat basins (Keskar et al., 2017).
2. **Implicit regularization:** The noise variance is proportional to α/|B|. Larger batches → less noise → closer to full-batch GD → risk of sharper minima. This is the "large-batch problem."

**SGD vs Adam:**
- SGD: one global learning rate α, no memory of past gradients.
- Adam: per-parameter adaptive rates, tracks first moment (mean) and second moment (variance) of gradients → faster convergence on ill-conditioned loss landscapes.
- SGD with momentum often generalizes better than Adam in the long run; Adam converges faster early.

## Derivation
Let the true loss be L(θ) = (1/N)Σᵢ Lᵢ(θ). The mini-batch gradient is:

$$\hat{g}_t = \frac{1}{|\mathcal{B}|} \sum_{i \in \mathcal{B}_t} \nabla_\theta \mathcal{L}_i(\theta_t)$$

**Unbiasedness:** E[ĝ_t] = ∇L(θ_t) — the expected mini-batch gradient equals the true gradient.

**Variance:** Var[ĝ_t] = σ²/|B| where σ² is the per-sample gradient variance. Larger batches reduce noise.

**Update rule:** θ_{t+1} = θ_t − α ĝ_t

**Convergence for non-convex L** (with L-smooth loss, fixed α):
$$\frac{1}{T}\sum_{t=0}^{T-1} \mathbb{E}\|\nabla \mathcal{L}(\theta_t)\|^2 \leq \frac{\mathcal{L}(\theta_0) - \mathcal{L}^*}{\alpha T} + \frac{\alpha L \sigma^2}{|\mathcal{B}|}$$

The first term shrinks as T grows (progress). The second term is a noise floor — it never goes to zero unless α → 0. This is why learning rate decay is necessary for exact convergence: diminishing α makes the noise floor shrink over time.

**SGD with momentum** accumulates a velocity vector v:
$$v_{t+1} = \mu v_t - \alpha \hat{g}_t, \quad \theta_{t+1} = \theta_t + v_{t+1}$$
Momentum β ∈ [0.9, 0.99] smooths the noisy gradient signal and accelerates along consistent directions.

## Worked Example
```python
import numpy as np

rng = np.random.default_rng(42)

# Synthetic dataset: y = 2x + 1 + noise
N = 1000
X = rng.standard_normal(N)
y = 2 * X + 1 + 0.5 * rng.standard_normal(N)

# Parameters: f(x) = w*x + b, MSE loss
w, b = 0.0, 0.0
lr = 0.01
batch_size = 32
epochs = 5

for epoch in range(epochs):
    indices = rng.permutation(N)          # shuffle each epoch
    total_loss = 0.0
    for start in range(0, N, batch_size):
        idx = indices[start:start + batch_size]
        xb, yb = X[idx], y[idx]

        pred = w * xb + b                 # forward pass
        err  = pred - yb
        loss = (err ** 2).mean()
        total_loss += loss

        # Gradients of MSE w.r.t. w and b
        dw = (2 * err * xb).mean()
        db = (2 * err).mean()

        w -= lr * dw                      # SGD update
        b -= lr * db

    n_batches = N // batch_size
    print(f"Epoch {epoch+1}: loss={total_loss/n_batches:.4f}, w={w:.3f}, b={b:.3f}")

# After 5 epochs w ≈ 2.0, b ≈ 1.0 (true values)

# --- Compare: full-batch GD (one step per epoch) ---
w2, b2 = 0.0, 0.0
for epoch in range(epochs):
    pred = w2 * X + b2
    err  = pred - y
    dw   = (2 * err * X).mean()
    db   = (2 * err).mean()
    w2 -= lr * dw
    b2 -= lr * db
print(f"Full-batch after {epochs} epochs: w={w2:.3f}, b={b2:.3f}")
# SGD converges faster in wall-clock time despite noisier updates
```

## See Also
- [[Gradient Descent]] — the noiseless version SGD approximates
- [[Learning Rate]] — interacts critically with batch size: halving the batch doubles the noise, requiring lower α
- [[Loss Function]] — the function whose gradient SGD estimates from mini-batches
- [[Gradient]] — what SGD approximates from a subset of data
- [[Convergence]] — SGD has a noise floor unless learning rate is decayed
- [[Policy]] — in RL, policy gradient methods apply SGD to the expected return objective
