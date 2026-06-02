# Backpropagation

**One-liner:** The algorithm that computes the gradient of the loss with respect to every weight in a neural network by applying the [[Chain Rule]] layer by layer from output to input, reusing intermediate computations to avoid exponential work.

## Core Idea
$$\frac{\partial L}{\partial W^{(l)}} = \frac{\partial L}{\partial \mathbf{z}^{(l)}} \cdot \left(\mathbf{h}^{(l-1)}\right)^\top, \qquad \boldsymbol{\delta}^{(l)} = \frac{\partial L}{\partial \mathbf{z}^{(l)}} = \left(W^{(l+1)}\right)^\top \boldsymbol{\delta}^{(l+1)} \odot f'\!\left(\mathbf{z}^{(l)}\right)$$
δ^(l) is the **error signal** at layer l — computed from δ^(l+1) of the layer above. Once we have δ^(l), the weight gradient ∂L/∂W^(l) is a rank-1 outer product. The algorithm propagates δ from the output back to the input, visiting each layer exactly once.

## Why It Exists
A network with a million weights has a million partial derivatives to compute. Naively applying finite differences (perturbing each weight by ε and measuring loss change) requires a million forward passes — computationally infeasible. Backpropagation computes all gradients in a single backward pass whose cost is the same order as two forward passes, regardless of the number of parameters. This is what made training deep networks feasible.

## Real-World Applications
- **Every gradient-based deep learning system:** PyTorch and TensorFlow implement backpropagation automatically via reverse-mode automatic differentiation (autograd).
- **ChatGPT training:** backpropagation computes gradients through a 96-layer transformer with 175B parameters.
- **Baymax:** backprop trains the sensor-to-action network in Isaac Sim, updating all weights after each rollout.
- **AlphaFold, DALL-E, Stable Diffusion:** all trained using variants of backpropagation.

## Intuition
Backpropagation is the [[Chain Rule]] applied systematically to a computation graph, with memoisation. Imagine tracing how a small change to weight w_{ij}^(l) ripples through the network to affect the final loss L. It first changes z^(l), which changes h^(l), which changes z^(l+1), ..., which changes L. The chain rule multiplies all these local sensitivities together. Backprop does this for every weight simultaneously by working backwards: it computes how sensitive L is to each layer's pre-activation z^(l), stores that as δ^(l), and uses it to compute the gradient for that layer's weights. The key insight: δ^(l) can be reused for all weights in layer l, and computing δ^(l) from δ^(l+1) is cheap (one matrix multiply + element-wise multiply).

## Derivation

**Setup:** L-layer network, loss L = ℓ(ŷ, y) where ŷ = h^(L). Forward pass has already stored z^(l) and h^(l) for all l.

**Step 1 — Output layer gradient (l = L):**

$$\boldsymbol{\delta}^{(L)} = \frac{\partial L}{\partial \mathbf{z}^{(L)}} = \frac{\partial L}{\partial \mathbf{h}^{(L)}} \odot f^{(L)'}\!\!\left(\mathbf{z}^{(L)}\right)$$

For cross-entropy + softmax: δ^(L) = ŷ − y (a beautiful simplification).

**Step 2 — Backpropagate the error signal (l = L-1, ..., 1):**

From δ^(l+1), compute δ^(l) via the chain rule:

$$\frac{\partial L}{\partial \mathbf{h}^{(l)}} = \left(W^{(l+1)}\right)^\top \boldsymbol{\delta}^{(l+1)}$$

$$\boldsymbol{\delta}^{(l)} = \frac{\partial L}{\partial \mathbf{z}^{(l)}} = \frac{\partial L}{\partial \mathbf{h}^{(l)}} \odot f^{(l)'}\!\!\left(\mathbf{z}^{(l)}\right)$$

Expanded:
$$\boldsymbol{\delta}^{(l)} = \left[\left(W^{(l+1)}\right)^\top \boldsymbol{\delta}^{(l+1)}\right] \odot f^{(l)'}\!\!\left(\mathbf{z}^{(l)}\right)$$

**Step 3 — Compute weight and bias gradients:**

$$\frac{\partial L}{\partial W^{(l)}} = \boldsymbol{\delta}^{(l)} \left(\mathbf{h}^{(l-1)}\right)^\top \qquad \text{shape: } (n_l \times n_{l-1})$$

$$\frac{\partial L}{\partial \mathbf{b}^{(l)}} = \boldsymbol{\delta}^{(l)} \qquad \text{shape: } (n_l \times 1)$$

**Why this works — tracing the chain rule:**

$$\frac{\partial L}{\partial w_{ij}^{(l)}} = \underbrace{\frac{\partial L}{\partial z_i^{(l)}}}_{\delta_i^{(l)}} \cdot \underbrace{\frac{\partial z_i^{(l)}}{\partial w_{ij}^{(l)}}}_{h_j^{(l-1)}}$$

The second factor is simply h_j^{(l-1)} because $z_i^{(l)} = \sum_j w_{ij}^{(l)} h_j^{(l-1)} + b_i^{(l)}$.

**Time complexity:** O(P) where P = total number of parameters. Same order as the forward pass — this is the key efficiency gain.

**Connection to calculus:** This is exactly reverse-mode automatic differentiation (reverse accumulation). Forward-mode AD propagates derivatives from input to output (efficient when n_inputs < n_outputs); reverse-mode (backprop) propagates from output to input (efficient when n_outputs ≪ n_inputs, which is always true in ML since L is scalar).

## Worked Example

```python
import numpy as np

# Manual backprop through a 2-layer net
# Architecture: 2 → 3 → 1, MSE loss
# Dataset: 4 samples

np.random.seed(1)

def relu(z):       return np.maximum(0, z)
def relu_d(z):     return (z > 0).astype(float)   # ReLU derivative

# Data
X = np.array([[0.1, 0.2], [0.4, 0.5], [0.7, 0.8], [0.9, 0.1]]).T  # (2, 4)
y = np.array([[0.3, 0.7, 0.9, 0.4]])                                # (1, 4)

# Parameters
W1 = np.random.randn(3, 2) * 0.5   # (3, 2)
b1 = np.zeros((3, 1))
W2 = np.random.randn(1, 3) * 0.5   # (1, 3)
b2 = np.zeros((1, 1))

lr = 0.1

for epoch in range(500):
    m = X.shape[1]

    # === FORWARD PASS ===
    Z1 = W1 @ X + b1         # (3, 4)
    H1 = relu(Z1)             # (3, 4)
    Z2 = W2 @ H1 + b2         # (1, 4)
    H2 = Z2                   # linear output for regression

    # MSE Loss
    loss = np.mean((H2 - y)**2)

    # === BACKWARD PASS ===
    # Output layer delta: dL/dZ2
    dL_dH2 = 2 * (H2 - y) / m     # (1, 4)
    delta2  = dL_dH2               # linear output → f'=1

    # Gradients for W2, b2
    dW2 = delta2 @ H1.T            # (1, 3)
    db2 = delta2.sum(axis=1, keepdims=True)  # (1, 1)

    # Backpropagate to hidden layer
    dL_dH1 = W2.T @ delta2         # (3, 4)
    delta1  = dL_dH1 * relu_d(Z1)  # (3, 4) — element-wise * ReLU'

    # Gradients for W1, b1
    dW1 = delta1 @ X.T             # (3, 2)
    db1 = delta1.sum(axis=1, keepdims=True)  # (3, 1)

    # === PARAMETER UPDATE ===
    W2 -= lr * dW2;  b2 -= lr * db2
    W1 -= lr * dW1;  b1 -= lr * db1

    if epoch % 100 == 0:
        print(f"Epoch {epoch:4d}: Loss = {loss:.6f}")

print(f"\nFinal predictions: {H2.round(3)}")
print(f"True values:       {y}")

# Verify gradients numerically (finite differences)
eps = 1e-5
def forward_loss(W1, b1, W2, b2):
    Z1 = W1 @ X + b1; H1 = relu(Z1)
    Z2 = W2 @ H1 + b2
    return np.mean((Z2 - y)**2)

# Check one weight numerically
i, j = 0, 0
W2_plus  = W2.copy(); W2_plus[i,j]  += eps
W2_minus = W2.copy(); W2_minus[i,j] -= eps
numerical_grad = (forward_loss(W1,b1,W2_plus,b2) - forward_loss(W1,b1,W2_minus,b2)) / (2*eps)
print(f"\nNumerical dW2[0,0]: {numerical_grad:.6f}")
print(f"Analytic  dW2[0,0]: {dW2[i,j]:.6f}")
```

## See Also
- [[Chain Rule]] — the calculus identity that backpropagation mechanically applies at each layer
- [[Forward Pass]] — must run first; stores intermediate values (z^(l), h^(l)) that backprop reuses
- [[Gradient]] — backpropagation's output; the vector ∇_w L used by optimisers
- [[Gradient Descent]] — consumes the gradients from backprop to update all weights
- [[Neural Network]] — the architecture backpropagation is defined over
- [[Artificial Neuron]] — one neuron's backward pass is a single application of the chain rule
- [[Activation Function]] — f'(z^(l)) appears in every δ^(l) computation
- [[Loss Function]] — the scalar L that backprop differentiates with respect to all weights
- [[Integration by Parts]] — IBP and backprop both use the product/chain rule to push derivatives backward through a composition; the structural analogy is exact
- [[Taylor Series]] — the chain rule that backprop applies is the machinery of multivariable Taylor expansion; each layer's Jacobian is a local linear (first-order Taylor) map
