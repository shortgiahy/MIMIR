# Artificial Neuron

**One-liner:** The atomic unit of a neural network — it computes a weighted sum of its inputs, adds a bias, then passes the result through an activation function to produce a single scalar output.

## Core Idea
$$\text{output} = f\!\left(\sum_{i=1}^{n} w_i x_i + b\right) = f(\mathbf{w}^\top \mathbf{x} + b)$$
**x** = input vector, **w** = weight vector (one scalar per input), b = bias scalar, f = [[Activation Function]]. The weights control how much each input matters; the bias shifts the threshold. Without f the neuron is a pure linear function.

## Why It Exists
We need a parametric, differentiable building block that can be composed into arbitrarily complex functions. The weighted sum is cheap to compute and easy to differentiate. The activation function introduces the nonlinearity required for the composite system to represent anything beyond linear maps. A single neuron is the simplest such unit.

## Real-World Applications
- **Single neuron = logistic regression:** sigmoid applied to **w**ᵀ**x** + b gives a binary classifier; this is how spam filters work.
- **[[Neural Network]]:** stacking thousands of neurons in layers produces image classifiers, language models, and robot controllers.
- **Baymax sensors:** each laser scan reading x_i is an input; after one layer of neurons, the network extracts features like "obstacle on the left."
- **ChatGPT:** has billions of neurons; each token position is processed through repeated weighted sums + activations.

## Intuition
The neuron is a voting machine. Each input x_i casts a vote, weighted by w_i — a large positive weight means "this input strongly activates me"; a large negative weight means "this input suppresses me." The bias b is a default opinion that shifts the decision threshold regardless of inputs. The activation function then decides whether the accumulated vote is strong enough to "fire." This is a loose analogy to a biological neuron firing when it receives enough excitatory input — but the analogy breaks down quickly: biological neurons are spike-timing devices, not smooth differentiable functions.

**Why a single neuron is a linear classifier (without activation):**

$$z = \mathbf{w}^\top \mathbf{x} + b = 0$$

is the equation of a hyperplane in ℝⁿ. The neuron classifies inputs by which side of this hyperplane they fall on. This can separate linearly separable data but fails on XOR — the original motivation for adding nonlinearity and depth.

## Derivation

**Forward pass of one neuron:**

Given input **x** ∈ ℝⁿ, weight vector **w** ∈ ℝⁿ, bias b ∈ ℝ:

1. Pre-activation (linear combination): $z = \mathbf{w}^\top \mathbf{x} + b$
2. Activation: $a = f(z)$

**Gradient computation (for [[Backpropagation]]):**

Let L be the loss. Suppose we already have $\delta = \frac{\partial L}{\partial a}$ from the layer above.

$$\frac{\partial L}{\partial z} = \delta \cdot f'(z)$$

$$\frac{\partial L}{\partial w_i} = \frac{\partial L}{\partial z} \cdot \frac{\partial z}{\partial w_i} = \delta \cdot f'(z) \cdot x_i$$

$$\frac{\partial L}{\partial b} = \frac{\partial L}{\partial z} \cdot 1 = \delta \cdot f'(z)$$

$$\frac{\partial L}{\partial x_i} = \frac{\partial L}{\partial z} \cdot w_i \quad \text{(passed to the layer below)}$$

This is exactly the [[Chain Rule]] applied once. Stacking these gradients over many neurons is [[Backpropagation]].

**Parameter count:** a single neuron with n inputs has n + 1 parameters (n weights + 1 bias).

## Worked Example

```python
import numpy as np

# Single neuron: 3 inputs
x = np.array([0.5, -1.0, 2.0])  # input
w = np.array([0.3,  0.8, -0.5]) # weights
b = 0.1                          # bias

# Forward pass
z = np.dot(w, x) + b             # pre-activation
print(f"z = {z:.4f}")            # z = -0.4 + 0.1 = -0.75 (roughly)

# ReLU activation
a = max(0, z)
print(f"a (ReLU) = {a:.4f}")     # 0.0 if z < 0

# Sigmoid activation
def sigmoid(z):
    return 1 / (1 + np.exp(-z))

a_sig = sigmoid(z)
print(f"a (sigmoid) = {a_sig:.4f}")  # ~0.32

# Backward pass: assume dL/da = 1.0 and we use sigmoid
delta = 1.0
dL_dz = delta * a_sig * (1 - a_sig)   # sigmoid derivative
dL_dw = dL_dz * x                     # gradient w.r.t. weights
dL_db = dL_dz                         # gradient w.r.t. bias
print(f"dL/dw = {dL_dw}")
print(f"dL/db = {dL_db:.6f}")

# Weight update (gradient descent, lr=0.01)
lr = 0.01
w -= lr * dL_dw
b -= lr * dL_db
print(f"Updated w = {w}")
```

## See Also
- [[Activation Function]] — f(z); without it the neuron is purely linear
- [[Neural Network]] — a layered graph of artificial neurons
- [[Forward Pass]] — how input flows through a neuron (and the whole network)
- [[Backpropagation]] — how gradients flow backward through the neuron
- [[Loss Function]] — the signal that drives weight updates
- [[Gradient Descent]] — the optimiser that uses the gradients to update w and b
- [[Dot Product]] — the core operation **w**ᵀ**x**
- [[Vector]] — **x** and **w** are vectors; the neuron lives in that space
- [[Ohm's Law]] — V = IR is a linear weighted relationship between voltage and current; a neuron's pre-activation z = w·x + b is the same linear combination structure, just in a higher-dimensional space
- [[Electric Current]] — the biological neuron metaphor: current flows when the threshold is exceeded; the activation function models whether a neuron "fires" based on accumulated input
