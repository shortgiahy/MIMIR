# Neural Network

**One-liner:** A layered composition of [[Artificial Neuron]]s that together can approximate arbitrarily complex functions, trained end-to-end by propagating gradients through every layer.

## Core Idea
$$\mathbf{h}^{(l)} = f^{(l)}\!\left(W^{(l)}\mathbf{h}^{(l-1)} + \mathbf{b}^{(l)}\right), \quad l = 1,\ldots,L$$
Layer l takes the previous layer's output **h**^(l−1) (or the raw input **x** when l=1), applies an affine transformation with weight matrix W^(l) and bias **b**^(l), then passes element-wise through activation f^(l). The final layer's output is the network's prediction.

## Why It Exists
A single [[Artificial Neuron]] is a linear classifier (modulo the activation). Adding depth lets the network build a hierarchy of representations: early layers detect simple patterns (edges in images, phonemes in audio), later layers combine those into complex concepts (faces, words). This hierarchy emerges automatically from training — no feature engineering required.

## Real-World Applications
- **Image recognition:** CNNs (specialised neural nets with shared weights) classify photos at superhuman accuracy (ResNet, EfficientNet).
- **Language models (ChatGPT):** transformer architecture — a specific kind of neural net — predicts the next token from context.
- **Robot control (Baymax):** a 3-layer MLP maps laser scan + odometry → velocity command, trainable entirely in Isaac Sim.
- **AlphaFold:** neural net predicts 3D protein structure from amino acid sequence.
- **Deep Q-Networks:** neural net approximates Q(s,a) for RL over large state spaces.

## Intuition
A neural network is a pipeline of learned transformations. Think of it as a sequence of coordinate changes: each layer warps and rotates the input space so that, by the final layer, examples of different classes have been untangled into regions that a simple linear boundary can separate. The network learns these warps by gradient descent — it tries millions of parameter values and keeps the ones that minimise the loss on training data.

**Depth vs. width:**
- A wider network has more neurons per layer — it can represent more features at the same level of abstraction.
- A deeper network has more layers — it can build more levels of abstraction, typically needing far fewer parameters to represent the same function.
- In practice: depth is more important. ResNet-50 (50 layers) outperforms a single massive layer.

## Derivation

**Universal Approximation Theorem (informal):**

*Theorem (Cybenko 1989):* Let f: [0,1]ⁿ → ℝ be continuous and σ be any non-polynomial continuous activation. Then for any ε > 0 there exist N, weights w_i, biases b_i, and output weights v_i such that:

$$\left|F(\mathbf{x}) - f(\mathbf{x})\right| < \varepsilon \quad \forall \mathbf{x} \in [0,1]^n$$

where $F(\mathbf{x}) = \sum_{i=1}^N v_i\, \sigma(\mathbf{w}_i^\top \mathbf{x} + b_i)$ is a one-hidden-layer network.

**What it says:** width alone (with one hidden layer) is sufficient for approximation.

**What it does NOT say:**
- It says nothing about how many neurons N are needed (could be exponential in n).
- It gives no training algorithm — only existence.
- Depth often reduces N by exponential factors (functions needing exp(n) neurons in 1 layer need only O(n²) with depth).

**Parameter count formula:**

For a fully connected network with input size n₀, layer sizes n₁, n₂, ..., n_L:

$$\text{Parameters} = \sum_{l=1}^{L} \underbrace{n_l \cdot n_{l-1}}_{\text{weights}} + \underbrace{n_l}_{\text{biases}} = \sum_{l=1}^{L} n_l(n_{l-1} + 1)$$

Example: 784 → 128 → 64 → 10 network (MNIST):
- Layer 1: 784×128 + 128 = 100,480
- Layer 2: 128×64 + 64 = 8,256
- Layer 3: 64×10 + 10 = 650
- **Total: 109,386 parameters**

**Forward pass equations (matrix form, see also [[Forward Pass]]):**

$$\mathbf{z}^{(l)} = W^{(l)}\mathbf{h}^{(l-1)} + \mathbf{b}^{(l)} \in \mathbb{R}^{n_l}$$
$$\mathbf{h}^{(l)} = f^{(l)}(\mathbf{z}^{(l)}) \in \mathbb{R}^{n_l}$$

**Shape rule:** W^(l) must have shape (n_l × n_{l−1}) so the matrix multiply is valid.

## Worked Example

```python
import numpy as np

# Fully connected neural net from scratch: 2 → 4 → 1
# Task: learn XOR (not linearly separable)

def relu(z):       return np.maximum(0, z)
def relu_grad(z):  return (z > 0).astype(float)
def sigmoid(z):    return 1 / (1 + np.exp(-z))

# Dataset: XOR
X = np.array([[0,0],[0,1],[1,0],[1,1]], dtype=float)  # (4, 2)
y = np.array([[0],  [1],  [1],  [0]],  dtype=float)  # (4, 1)

# Initialise parameters (He init for ReLU)
np.random.seed(42)
W1 = np.random.randn(4, 2) * np.sqrt(2/2)  # (4, 2)
b1 = np.zeros((4, 1))                        # (4, 1)
W2 = np.random.randn(1, 4) * np.sqrt(2/4)  # (1, 4)
b2 = np.zeros((1, 1))                        # (1, 1)

lr = 0.5
for epoch in range(5000):
    # === Forward pass ===
    Z1 = W1 @ X.T + b1         # (4, 4)
    H1 = relu(Z1)              # (4, 4)
    Z2 = W2 @ H1 + b2          # (1, 4)
    H2 = sigmoid(Z2)           # (1, 4)  — predictions

    # MSE loss
    loss = np.mean((H2 - y.T)**2)

    # === Backward pass ===
    dL_dH2 = 2 * (H2 - y.T) / 4           # (1, 4)
    dL_dZ2 = dL_dH2 * H2 * (1 - H2)      # (1, 4)
    dL_dW2 = dL_dZ2 @ H1.T                # (1, 4)
    dL_db2 = dL_dZ2.sum(axis=1, keepdims=True)

    dL_dH1 = W2.T @ dL_dZ2                # (4, 4)
    dL_dZ1 = dL_dH1 * relu_grad(Z1)       # (4, 4)
    dL_dW1 = dL_dZ1 @ X                   # (4, 2)
    dL_db1 = dL_dZ1.sum(axis=1, keepdims=True)

    # Parameter update
    W2 -= lr * dL_dW2;  b2 -= lr * dL_db2
    W1 -= lr * dL_dW1;  b1 -= lr * dL_db1

    if epoch % 1000 == 0:
        print(f"epoch {epoch}: loss = {loss:.4f}")

print("\nPredictions (should be ~[0,1,1,0]):", H2.round(2))
```

## See Also
- [[Artificial Neuron]] — the atomic unit composing every layer
- [[Activation Function]] — the nonlinearity that makes stacking layers useful
- [[Forward Pass]] — detailed walkthrough of data flowing through the network
- [[Backpropagation]] — how gradients flow backward to update all weights
- [[Loss Function]] — the signal that tells the network how wrong it is
- [[Gradient Descent]] — the optimiser that uses gradients to update W and b
- [[Matrix Multiplication]] — the core linear algebra operation in every layer
- [[Q-Learning]] — DQN replaces the Q-table with a neural network
