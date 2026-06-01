# Gradient

**One-liner:** A vector of all partial derivatives of a scalar function — pointing in the direction of steepest ascent in parameter space.

## Core Idea
$$\nabla f(\mathbf{x}) = \begin{bmatrix} \frac{\partial f}{\partial x_1} \\ \frac{\partial f}{\partial x_2} \\ \vdots \\ \frac{\partial f}{\partial x_n} \end{bmatrix}$$
The gradient of a scalar-valued function $f: \mathbb{R}^n \to \mathbb{R}$ is an $n$-dimensional [[Vector]] where each component is the partial derivative with respect to one variable. It tells you: if you nudge $x_i$ slightly, how much does $f$ change? Stack all those answers and you get the gradient.

## Why It Exists
In 1D, the derivative tells you which direction on a number line increases a function. In $n$ dimensions, you need a vector for "which direction" — that's the gradient. Without it, [[Gradient Descent]] has no direction to move. Every weight update in every neural network in the world is driven by $\nabla L$.

## Real-World Applications
- **ML training:** $\nabla_\theta L$ is the gradient of the loss with respect to all model parameters $\theta$. A ResNet-50 has 25M parameters — the gradient is a 25M-dimensional vector computed every training step.
- **Optimization in robotics:** trajectory optimization for Baymax's motion planning computes gradients of collision cost w.r.t. joint angles.
- **Computer graphics:** gradient of a signed distance function points toward the surface normal — used in ray marching.
- **Physics simulation:** gradient of potential energy is the force: $\mathbf{F} = -\nabla V$. [[Newton's Second Law]] in disguise.

## Intuition
Stand on a mountain (the loss landscape). You're blindfolded. The gradient is a compass that always points uphill — toward the steepest slope. To descend, walk opposite the gradient. The *magnitude* of the gradient tells you steepness; the *direction* tells you which way.

In 2D ($f(x, y)$): the gradient is a 2D vector. At any point, it points perpendicular to the contour lines (lines of equal height). The steeper the nearby slope, the longer the gradient vector.

**Critical distinction:** The gradient $\nabla f$ is a property of a *point* — it changes as you move. The gradient at the minimum is exactly $\mathbf{0}$ (every partial is zero = flat in all directions).

## Derivation
**Directional derivative** (how fast $f$ changes in direction $\hat{u}$):
$$D_{\hat{u}} f = \nabla f \cdot \hat{u} = ||\nabla f|| \cos\theta$$
This is maximized when $\hat{u} = \nabla f / ||\nabla f||$ (unit gradient direction, $\cos 0° = 1$). Proof that gradient points in direction of steepest ascent. ∎

**Partial derivative intuition:**
$$\frac{\partial f}{\partial x_i} = \lim_{h \to 0} \frac{f(x_1, \ldots, x_i + h, \ldots, x_n) - f(\mathbf{x})}{h}$$
"Wiggle only $x_i$ by an infinitesimal amount, measure the change in $f$, divide."

**Example:** $f(x, y) = x^2 + 3xy + y^2$
$$\nabla f = \begin{bmatrix} 2x + 3y \\ 3x + 2y \end{bmatrix}$$
At point $(1, 2)$: $\nabla f = \begin{bmatrix} 8 \\ 7 \end{bmatrix}$ — moving in direction $(8, 7)$ increases $f$ fastest.

## Worked Example
```python
import numpy as np

# ── Numerical gradient (finite differences) ──────────────
def f(x):
    """Scalar function of a vector."""
    return x[0]**2 + 3*x[0]*x[1] + x[1]**2

def numerical_gradient(f, x, h=1e-5):
    """Compute gradient by finite differences."""
    grad = np.zeros_like(x, dtype=float)
    for i in range(len(x)):
        x_plus  = x.copy(); x_plus[i]  += h
        x_minus = x.copy(); x_minus[i] -= h
        grad[i] = (f(x_plus) - f(x_minus)) / (2 * h)
    return grad

x = np.array([1.0, 2.0])
grad = numerical_gradient(f, x)
print(f"Numerical gradient at (1,2): {grad}")
# [8. 7.] — matches analytical 2x+3y=8, 3x+2y=7 ✓

# ── Gradient of a loss function (practical ML) ────────────
# Simple linear regression: predict y from x*w
def compute_loss_and_grad(X, y_true, w):
    """MSE loss and its gradient w.r.t. w."""
    y_pred = X @ w               # predictions
    errors = y_pred - y_true     # residuals
    loss   = np.mean(errors**2)  # MSE scalar
    # dL/dw = (2/N) * X^T @ errors
    grad_w = (2 / len(y_true)) * X.T @ errors
    return loss, grad_w

# Toy data: y ≈ 2x
np.random.seed(42)
X = np.column_stack([np.ones(50), np.random.randn(50)])  # (50, 2)
w_true = np.array([0.5, 2.0])  # intercept=0.5, slope=2.0
y = X @ w_true + 0.1 * np.random.randn(50)

w = np.array([0.0, 0.0])   # start at zero
loss, grad = compute_loss_and_grad(X, y, w)
print(f"Initial loss: {loss:.4f}")
print(f"Gradient:     {grad}")   # [neg, neg] → w needs to increase

# One step of gradient descent
lr = 0.1
w_new = w - lr * grad
loss_new, _ = compute_loss_and_grad(X, y, w_new)
print(f"Loss after one step: {loss_new:.4f}")  # lower ✓

# ── Gradient is zero at a minimum ────────────────────────
# For f(x) = x^2, minimum at x=0, gradient = 2x = 0 there
x_values = np.linspace(-3, 3, 100)
grad_values = 2 * x_values   # gradient of x^2
print(f"Gradient at x=0: {2*0}")  # 0.0 — flat at minimum
```

## See Also
- [[Derivative]] — the 1D analog; gradient is its n-dimensional extension
- [[Gradient Descent]] — uses the gradient to iteratively minimize a loss
- [[Loss Function]] — the scalar function whose gradient drives training
- [[Backpropagation]] — the algorithm that computes $\nabla_\theta L$ efficiently
- [[Vector]] — the gradient is a vector; understanding vectors is prerequisite
- [[Partial Derivative]] — each component of the gradient is a partial derivative
- [[Newton's Second Law]] — $\mathbf{F} = -\nabla V$; force is negative gradient of potential
- [[Conservative Force]] — in Physics, F = −∇U means force is the negative gradient of potential energy; gradient descent mimics this descent exactly
- [[Taylor Series]] — the gradient is the first-order coefficient in the Taylor expansion of a scalar function; the whole step is a local linear approximation
- [[Velocity]] — gradient of loss w.r.t. time is analogous to velocity; both measure the rate of change of a scalar quantity along a trajectory
