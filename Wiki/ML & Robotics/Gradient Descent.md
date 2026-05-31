# Gradient Descent

**One-liner:** Gradient descent is an iterative optimization algorithm that finds the minimum of a function by repeatedly stepping in the direction opposite to the gradient.

## Why It Exists

Almost every machine learning problem reduces to a single question: find the parameters that minimize some cost. For a neural network predicting robot joint angles, the "parameters" are millions of weights, and the "cost" is how far off the predictions are from the correct outputs.

Minimizing this cost analytically — solving for the exact minimum with algebra — is impossible for most real functions. The function is nonlinear, high-dimensional, and has no closed form. Even if it did, inverting a system with millions of variables is computationally intractable.

Gradient descent sidesteps this entirely. Instead of solving for the minimum algebraically, it *walks toward* the minimum iteratively. At each step, it asks: which direction does the function increase most steeply? Then it takes a small step in the *opposite* direction. Repeat. Eventually, it converges to a minimum.

This is not a new idea — it dates back to Cauchy in 1847 — but it became foundational to ML because:
1. It scales to any number of dimensions.
2. It only requires computing the gradient, which can be done efficiently via backpropagation.
3. Stochastic variants (SGD) work on minibatches, making it tractable for datasets of millions of examples.

## The Concept

### What Are We Optimizing?

Start with a **loss function** $L(\theta)$ where $\theta$ is the vector of all parameters. For concreteness, think of the mean squared error (MSE) loss for a simple linear predictor:

$$L(\theta) = \frac{1}{n} \sum_{i=1}^{n} (y_i - \hat{y}_i)^2 = \frac{1}{n} \sum_{i=1}^{n} (y_i - \theta^T x_i)^2$$

This is a scalar function of many variables. The surface traced by $L(\theta)$ as $\theta$ varies is called the **loss landscape**. We want to find the point on this landscape with the lowest elevation.

### Why Derivatives Point Uphill

The derivative $\frac{dL}{d\theta}$ (or for multiple parameters, the gradient $\nabla_\theta L$) tells you how $L$ changes when you nudge each parameter. Specifically, the gradient vector at any point points in the direction of **steepest ascent** — the direction in which the function increases fastest.

This follows from the definition of the directional derivative. For any unit direction vector $\hat{u}$:
$$D_{\hat{u}} L = \nabla L \cdot \hat{u} = \|\nabla L\| \cos\theta$$

This is maximized when $\hat{u}$ is parallel to $\nabla L$ (i.e., $\cos\theta = 1$). The gradient *is* the uphill direction. Moving opposite to the gradient is therefore steepest descent.

### The Algorithm

Starting from some initial parameters $\theta_0$ (often random), repeat:

$$\theta_{t+1} = \theta_t - \alpha \nabla_\theta L(\theta_t)$$

where:
- $\alpha$ (alpha) is the **learning rate** — a small positive scalar controlling step size
- $\nabla_\theta L(\theta_t)$ is the gradient of the loss with respect to all parameters, evaluated at the current parameters

That's it. This single equation, applied thousands or millions of times, trains most modern ML models.

### Why This Works At All

For a **convex** function (bowl-shaped), gradient descent is guaranteed to converge to the global minimum. The loss landscape is a bowl, the gradient always points toward the rim, and taking steps away from the rim moves you toward the bottom.

For **non-convex** functions (which all deep neural networks have), there is no such guarantee. The loss landscape has many local minima, saddle points, and flat regions (plateaus). Yet gradient descent still works remarkably well in practice for two reasons:

1. **Local minima are often good enough.** Empirically, most local minima in large neural networks have similar loss values — the landscape has many equivalently-good solutions.
2. **Saddle points, not local minima, are the main obstacle.** In high dimensions, true local minima (where every direction is uphill) are exponentially rare. Most flat regions are saddle points, and gradient noise (from stochastic methods) helps escape them.

### Learning Rate Intuition

The learning rate $\alpha$ is the single most important hyperparameter. Too large: the steps overshoot the minimum and the loss *diverges* (often to infinity). Too small: the algorithm converges, but it takes too long.

Formally, for convergence on a convex function with Lipschitz-continuous gradients (gradient doesn't change too fast), we need $\alpha < \frac{2}{L}$ where $L$ is the Lipschitz constant of the gradient. In practice, you don't compute this — you tune $\alpha$ empirically or use an adaptive optimizer (Adam, RMSProp) that adjusts step sizes automatically.

### Variants of Gradient Descent

**Batch gradient descent:** Use all $n$ training examples to compute the gradient at each step. Accurate but expensive — one gradient step requires processing the entire dataset.

**Stochastic Gradient Descent (SGD):** Use a single random example to estimate the gradient at each step. Fast but noisy — each step is based on a rough estimate of the true gradient, so the path zigzags.

**Mini-batch gradient descent:** Use a small random batch of $B$ examples (typically $B = 32$ to $256$). The gradient estimate is good enough, the updates are fast, and the hardware (GPU) can process the batch in parallel. This is what "SGD" almost always means in practice.

### Adaptive Optimizers

Modern practice often replaces vanilla gradient descent with **Adam** (Adaptive Moment Estimation). Adam maintains a running estimate of the first moment (mean) and second moment (variance) of past gradients, and uses them to scale each parameter's learning rate individually. Parameters with consistently large gradients get smaller effective steps; parameters with small or noisy gradients get larger relative steps.

This makes Adam far less sensitive to the initial learning rate and faster in practice. But it has its own failure modes (can converge to worse solutions than SGD in some cases). For robotics RL, you'll see both Adam and SGD with momentum, depending on the algorithm.

## Intuition

Imagine you're blindfolded on a hilly landscape and you want to reach the lowest point. You can't see the whole landscape — you can only feel the slope under your feet right now. The strategy: at each step, feel which direction is steepest downhill, and take a small step that way. Repeat until flat.

The learning rate is your step size. Giant steps risk stepping over the valley into the next hill. Tiny steps are safe but take forever.

The gradient is exactly the "slope under your feet" — the vector that tells you which direction is uphill and how steep. The negative gradient is downhill.

For neural networks, this landscape is in millions of dimensions, but the principle is identical. You can never visualize it, but the math is the same.

The key practical insight: you don't need to reach the global minimum. You just need a parameter setting where the loss is low enough that your model makes good predictions. In the million-dimensional landscape, there are many such points.

## Key Formula / Rule

**Gradient descent update rule:**

$$\theta \leftarrow \theta - \alpha \nabla_\theta L(\theta)$$

**For a single parameter $w$ with scalar loss $L$:**

$$w \leftarrow w - \alpha \frac{\partial L}{\partial w}$$

```python
import numpy as np

def gradient_descent(grad_fn, theta_init, alpha=0.01, n_steps=1000):
    """Generic gradient descent. grad_fn computes gradient of loss."""
    theta = theta_init.copy()
    for _ in range(n_steps):
        grad = grad_fn(theta)
        theta -= alpha * grad   # in-place update
    return theta
```

## Worked Example

**Problem:** Fit a line $\hat{y} = w \cdot x + b$ to noisy data using gradient descent on MSE loss. No libraries — derive and implement from scratch.

```python
import numpy as np

# Generate synthetic data: y = 2x + 1 + noise
np.random.seed(42)
n = 100
x = np.random.randn(n)
y = 2.0 * x + 1.0 + 0.3 * np.random.randn(n)

# Initialize parameters
w = 0.0
b = 0.0
alpha = 0.05  # learning rate

# Gradient descent
for step in range(200):
    # Forward pass: predictions
    y_hat = w * x + b                # shape (n,)

    # Loss: Mean Squared Error
    loss = np.mean((y - y_hat) ** 2)

    # Gradients (via calculus):
    # dL/dw = -2/n * sum((y - y_hat) * x)
    # dL/db = -2/n * sum(y - y_hat)
    residual = y - y_hat             # shape (n,)
    dw = -2 * np.mean(residual * x)
    db = -2 * np.mean(residual)

    # Update parameters
    w -= alpha * dw
    b -= alpha * db

    if step % 40 == 0:
        print(f"Step {step:3d}: loss={loss:.4f}, w={w:.3f}, b={b:.3f}")

# True values: w=2.0, b=1.0
# After 200 steps, should be very close.
```

**Tracing the math for one step:** At initialization, $w=0, b=0$, so $\hat{y} = 0$ everywhere. The residuals $(y - \hat{y}) = y \approx 2x + 1$. The gradient $\frac{\partial L}{\partial w} = -2 \text{mean}(y \cdot x) < 0$ for positive $x$ values (since $y$ is correlated with $x$). So the update $w \leftarrow w - \alpha \cdot \text{(negative number)}$ increases $w$ toward 2. The gradient correctly points toward the true parameters.

## Gotchas

**Gotcha 1 — Forgetting to divide by batch size.** If you compute the loss as a sum instead of a mean, your gradient magnitude scales with dataset size. This makes the effective learning rate dataset-size-dependent, breaking portability and making hyperparameters meaningless across different dataset sizes.

**Gotcha 2 — Learning rate too high causes divergence, not just slow convergence.** If you see the loss jumping to `nan` or growing to infinity, your learning rate is too large. The fix: reduce $\alpha$ by 10× and try again.

**Gotcha 3 — Vanishing / exploding gradients in deep networks.** For deep neural networks, gradients can shrink to nearly zero (vanishing) or grow to very large numbers (exploding) as they propagate backward through many layers. This is a major reason naive gradient descent fails on deep networks — why batch normalization, residual connections, and gradient clipping exist.

**Gotcha 4 — The gradient tells you about the current point, not the global landscape.** The update $\theta \leftarrow \theta - \alpha \nabla L$ is a local operation. It tells you nothing about whether you're near a global minimum, a local minimum, or a saddle point. Non-convex landscapes are why multiple random restarts are sometimes used.

**Gotcha 5 — Weight initialization matters.** Starting at $\theta = 0$ for neural network weights is a problem — all neurons are symmetric and learn identical features. Random initialization breaks symmetry. The scale of initialization (e.g., Xavier/He initialization) also affects how well gradients propagate.

**Gotcha 6 — Gradient descent is not the same as gradient flow.** Taking discrete steps with learning rate $\alpha$ is an approximation of the continuous-time gradient flow ODE $\dot{\theta} = -\nabla L(\theta)$. Large $\alpha$ values make the approximation inaccurate and can cause oscillation.

## See Also

- [[NumPy Arrays]] — gradient descent in practice is entirely NumPy array operations; every parameter update is a vectorized subtraction
- [[Neural Networks - The Basics]] — gradient descent is the training algorithm for neural networks; backpropagation computes the gradients that gradient descent consumes
- [[Partial Derivatives and the Gradient]] — the mathematical foundation; the gradient $\nabla L$ must be understood before gradient descent makes sense
- [[Markov Decision Processes]] — in RL, policy gradient methods apply gradient descent to optimize the expected return; same math, different objective
- [[Chain Rule]] — backpropagation is the chain rule applied to compute $\nabla_\theta L$; you need this to understand where the gradients come from
