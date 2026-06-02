# Learning Rate

**One-liner:** The scalar α that controls how large a step the optimizer takes along the negative gradient at each update.

## Core Idea
$$\theta \leftarrow \theta - \alpha \nabla_\theta \mathcal{L}(\theta)$$
The learning rate α multiplies the gradient before subtracting it from the current parameters. A large α moves parameters far per step; a small α moves them barely at all. It is arguably the single most important hyperparameter to tune in any gradient-based training run.

## Why It Exists
Without a scaling factor, the raw gradient magnitude depends on the loss scale and dataset size — it could be 0.0001 or 1000 depending on the problem. A fixed step of "gradient" would either do nothing or explode. The learning rate decouples step size from gradient magnitude, giving the practitioner explicit control over how aggressively parameters move.

## Real-World Applications
- **Neural network training:** Every modern model (GPT, ResNet, AlphaGo's policy network) is trained with a learning rate schedule — typically starting at 1e-3 and decaying.
- **RLHF (ChatGPT):** The PPO policy update uses a carefully tuned learning rate; too large destroys the pre-trained weights instantly.
- **Robot locomotion (Isaac Sim):** Legged locomotion policies trained via PPO use α ≈ 3e-4 with cosine annealing — too high and the gait never stabilizes.
- **Baymax navigation:** When fine-tuning a navigation policy with real sensor data, a warm-up schedule prevents catastrophic forgetting of the pre-trained behavior.

## Intuition
Picture the [[Loss Function]] as a hilly landscape. The [[Gradient]] tells you the slope direction. The learning rate tells you how far to walk downhill. If α is too large you overshoot the valley and land on the other side (or fly off entirely — loss diverges). If α is too small you creep forward and may take millions of steps to reach the bottom. The sweet spot depends on the local curvature, which changes as training progresses — hence schedules.

**Schedules** solve the fact that a good α early in training is often too large late in training:
- **Step decay:** multiply α by 0.1 every N epochs.
- **Cosine annealing:** α(t) = α_min + ½(α_max − α_min)(1 + cos(πt/T)).
- **Warm-up:** ramp from 0 to α_max over the first few thousand steps (critical for Transformers).
- **Cyclical LR:** oscillate between bounds — helps escape sharp minima.

## Derivation
Start from the goal: minimize L(θ) by following steepest descent. A first-order Taylor expansion (see [[Taylor Series]]) around θ gives:

$$\mathcal{L}(\theta - \alpha g) \approx \mathcal{L}(\theta) - \alpha \|g\|^2 \quad \text{where } g = \nabla_\theta \mathcal{L}$$

This is guaranteed to decrease L as long as α is small enough and g ≠ 0. The precise condition from the Lipschitz smoothness assumption (gradient changes at most L per unit step):

$$\mathcal{L}(\theta_{t+1}) \leq \mathcal{L}(\theta_t) - \alpha\left(1 - \frac{\alpha L}{2}\right)\|g_t\|^2$$

This is negative (i.e., loss decreases) when α < 2/L. So the maximum stable learning rate is determined by the curvature (largest eigenvalue of the Hessian). In practice L is unknown, so α is tuned empirically or bounded via adaptive methods.

**Convergence rate** for convex L with α = 1/L:
$$\mathcal{L}(\theta_T) - \mathcal{L}(\theta^*) \leq \frac{\|\theta_0 - \theta^*\|^2}{2\alpha T}$$
Convergence is O(1/T) — halving the error takes four times the steps. See [[Convergence]].

## Worked Example
```python
import numpy as np

# Simple 1D quadratic: L(theta) = (theta - 3)^2, minimum at theta=3
def loss(theta):
    return (theta - 3) ** 2

def grad(theta):
    return 2 * (theta - 3)

theta = 0.0  # start far from optimum

for lr, label in [(0.9, "too large"), (0.1, "good"), (0.001, "too small")]:
    t = theta
    for step in range(20):
        t = t - lr * grad(t)
    print(f"lr={lr:5.3f} ({label:10s}): final theta={t:.4f}, L={loss(t):.6f}")

# Output (approximate):
# lr=0.900 (too large  ): final theta=3.0000, L=0.000000  # lucky — converged but oscillated
# lr=0.100 (good       ): final theta=3.0000, L=0.000000
# lr=0.001 (too small  ): final theta=0.0558, L=8.688200  # barely moved

# Cosine annealing schedule
T = 100
alpha_max, alpha_min = 0.1, 1e-4
schedule = [alpha_min + 0.5*(alpha_max - alpha_min)*(1 + np.cos(np.pi*t/T))
            for t in range(T)]
print(f"LR at step 0: {schedule[0]:.4f}, step 50: {schedule[50]:.4f}, step 99: {schedule[99]:.6f}")
```

## See Also
- [[Gradient Descent]] — the algorithm that uses the learning rate at every step
- [[Gradient]] — the direction α scales before updating parameters
- [[Loss Function]] — what we are minimizing with each α-scaled step
- [[Stochastic Gradient Descent]] — learning rate interacts differently with noisy mini-batch gradients
- [[Taylor Series]] — the first-order approximation that justifies the gradient step; the valid step size α is bounded by the Taylor remainder
- [[Convergence]] — how learning rate choice determines convergence rate and stability
- [[Kinetic Energy]] — the analogy runs deep: too large a learning rate gives the optimizer too much "kinetic energy" to settle into a valley, causing it to overshoot — exactly as a ball rolling too fast overshoots the minimum
