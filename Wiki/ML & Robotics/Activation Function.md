# Activation Function

**One-liner:** A nonlinear function applied element-wise after the linear part of a neuron, giving neural networks the expressive power to represent functions that no stack of linear layers ever could.

## Core Idea
$$a = f(z), \qquad z = \mathbf{w}^\top \mathbf{x} + b$$
Without f (or with f = identity), a [[Neural Network]] of any depth collapses to a single linear map — adding layers provides no benefit. The activation function breaks linearity, enabling the network to partition input space in curved, non-hyperplanar ways.

## Why It Exists
**Linear collapse proof:** Let L₁, L₂ be linear layers with weight matrices W₁, W₂.

$$\mathbf{h}_1 = W_1 \mathbf{x}, \qquad \mathbf{h}_2 = W_2 \mathbf{h}_1 = (W_2 W_1)\mathbf{x} = W_\text{eff}\mathbf{x}$$

No matter how many linear layers you stack, the composition is still a linear map. A linear map cannot represent XOR, a circle decision boundary, or any nonlinear function — which rules out most interesting real-world tasks. The activation function is the sole source of nonlinearity in a standard network.

## Real-World Applications
- **ReLU:** used in almost all modern deep networks — CNNs for image recognition, transformers, robot perception stacks.
- **Sigmoid:** binary classification output layers; historical use in recurrent networks; logistic regression.
- **Softmax:** the output layer of any multi-class classifier (e.g., the token probability distribution in ChatGPT).
- **Tanh:** older RNNs (GRU, LSTM gates); preferred over sigmoid when zero-centered output matters.
- **Baymax:** ReLU activations in the sensor-fusion network; softmax on the final action-selection head.

## Intuition
A neuron with no activation is a measuring tape — it can only say "how much of this direction is in the input?" A neuron with a nonlinear activation is a detector — it can say "is this particular pattern present, yes or no?" Stack enough detectors in layers and you can detect patterns of patterns of patterns — the essence of deep learning.

## Derivation

**ReLU — Rectified Linear Unit:**
$$f(z) = \max(0, z), \qquad f'(z) = \begin{cases} 1 & z > 0 \\ 0 & z \leq 0 \end{cases}$$

Dead/dying ReLU problem: if z ≤ 0 for a neuron on every training example, its gradient is permanently zero — the weight never updates. Fix: Leaky ReLU $f(z) = \max(0.01z,\, z)$, or ELU.

**Sigmoid:**
$$\sigma(z) = \frac{1}{1 + e^{-z}}, \qquad \sigma'(z) = \sigma(z)(1-\sigma(z))$$

Derivation of derivative:
$$\sigma'(z) = \frac{e^{-z}}{(1+e^{-z})^2} = \frac{1}{1+e^{-z}} \cdot \frac{e^{-z}}{1+e^{-z}} = \sigma(z)(1-\sigma(z))$$

Problem: when |z| is large, σ'(z) ≈ 0 — the **vanishing gradient** problem. Gradients shrink exponentially with depth, making deep sigmoid networks hard to train (one reason ReLU replaced it).

**Tanh:**
$$\tanh(z) = \frac{e^z - e^{-z}}{e^z + e^{-z}}, \qquad \tanh'(z) = 1 - \tanh^2(z)$$

Zero-centered (range (−1,1) vs sigmoid's (0,1)), which helps [[Gradient Descent]] converge faster. Still suffers vanishing gradients.

**Softmax (multi-class output layer):**
$$\text{softmax}(\mathbf{z})_k = \frac{e^{z_k}}{\sum_{j=1}^K e^{z_j}}$$

Outputs a valid probability distribution: all values ∈ (0,1), sum = 1. The Jacobian:
$$\frac{\partial \text{softmax}_k}{\partial z_j} = \text{softmax}_k(\delta_{kj} - \text{softmax}_j)$$

Combined with cross-entropy loss, the gradient simplifies to $\hat{y}_k - y_k$, which is why this combination is ubiquitous.

**Why nonlinearity enables universal approximation:**

The universal approximation theorem (Cybenko 1989, Hornik 1991) states: a single hidden layer with a nonlinear activation and sufficiently many neurons can approximate any continuous function on a compact set to arbitrary precision. The proof constructs sigmoid "bump" functions that localise in input space — impossible with linear activations.

## Worked Example

```python
import numpy as np
import matplotlib
matplotlib.use('Agg')
import matplotlib.pyplot as plt

z = np.linspace(-4, 4, 200)

relu     = np.maximum(0, z)
sigmoid  = 1 / (1 + np.exp(-z))
tanh_z   = np.tanh(z)
leaky    = np.where(z > 0, z, 0.01 * z)

# Derivatives
relu_d    = (z > 0).astype(float)
sigmoid_d = sigmoid * (1 - sigmoid)
tanh_d    = 1 - tanh_z**2

print("At z=0:")
print(f"  ReLU     = {np.maximum(0,0):.2f},  ReLU'     = {float(0>0):.2f}")
print(f"  Sigmoid  = {1/(1+np.exp(0)):.4f},  Sigmoid'  = {0.25:.4f}")
print(f"  Tanh     = {np.tanh(0):.4f},  Tanh'     = {1-np.tanh(0)**2:.4f}")

# Softmax example
logits = np.array([2.0, 1.0, 0.1])
exp_z  = np.exp(logits - logits.max())   # numerically stable (subtract max)
softmax_out = exp_z / exp_z.sum()
print(f"\nSoftmax([2.0, 1.0, 0.1]) = {softmax_out.round(4)}")
print(f"Sum = {softmax_out.sum():.6f}")  # must be 1.0

# Dying ReLU demo
w = -5.0  # large negative weight — neuron is dead
x = np.random.randn(1000)
z_vals = w * x           # all negative if w<<0 and x ~ N(0,1)
print(f"\nDead ReLU: {(z_vals <= 0).mean()*100:.1f}% of inputs produce zero gradient")
```

## See Also
- [[Artificial Neuron]] — activation function is the f( ) wrapping the neuron's linear part
- [[Neural Network]] — the architecture that stacks neurons with activations
- [[Forward Pass]] — activation functions are applied at each layer during the forward pass
- [[Backpropagation]] — f'(z) appears in every gradient computation via the [[Chain Rule]]
- [[Loss Function]] — softmax is typically paired with cross-entropy loss at the output
- [[Gradient Descent]] — vanishing gradients (sigmoid/tanh) directly hurt gradient descent's ability to train deep networks
- [[Taylor Series]] — sigmoid and tanh have closed-form Taylor series; the universal approximation proof constructs sigmoid "bump" functions that are analogous to Taylor basis functions approximating arbitrary smooth functions
