# Forward Pass

**One-liner:** The computation that flows input data through a neural network layer by layer — applying affine transformations and activation functions — to produce a final prediction or output.

## Core Idea
$$\mathbf{h}^{(l)} = f^{(l)}\!\left(W^{(l)}\mathbf{h}^{(l-1)} + \mathbf{b}^{(l)}\right), \quad \mathbf{h}^{(0)} = \mathbf{x}, \quad \hat{\mathbf{y}} = \mathbf{h}^{(L)}$$
For each layer l: (1) multiply the previous layer's output by a weight matrix, (2) add a bias vector, (3) apply the activation function element-wise. Repeat for L layers. The result $\hat{\mathbf{y}}$ is the network's prediction (logits, probabilities, or a continuous value depending on the task).

## Why It Exists
The forward pass is the network making a prediction. Before training can happen (via [[Backpropagation]]), we must first compute the output so we can measure how wrong it is (via the [[Loss Function]]). Additionally, the intermediate values z^(l) and h^(l) computed during the forward pass must be stored — they are reused during backpropagation to compute gradients efficiently.

## Real-World Applications
- **Inference:** the forward pass IS the model at deployment time. When ChatGPT generates a token, it runs a forward pass through a transformer with billions of weights.
- **Image classification:** input = 224×224×3 pixel values; forward pass produces a 1000-dimensional softmax vector; the argmax is the predicted class.
- **Baymax sensor fusion:** input = 360 laser distance values; forward pass produces velocity command [v_x, v_y, ω].
- **DQN (Deep Q-Learning):** state (pixels or sensor vector) → forward pass → Q-values for each action.

## Intuition
Think of the forward pass as water flowing through a pipe system. Each layer is a processing station: it takes in the water from the previous station, filters and transforms it, and passes it forward. The shape of the water changes at each station (the dimensionality of the representation), but the flow is always strictly left-to-right. Nothing goes backwards until [[Backpropagation]].

**The computation graph:** the forward pass implicitly builds a directed acyclic graph (DAG) where nodes are tensors and edges are operations. This graph is precisely what frameworks like PyTorch traverse in reverse during backpropagation. PyTorch calls this autograd; the forward pass builds the tape, the backward pass reads it.

**Shape tracking — the critical skill:**

Layer l has weight matrix W^(l) of shape (n_l × n_{l-1}). The forward computation:

$$\underbrace{W^{(l)}}_{n_l \times n_{l-1}} \underbrace{\mathbf{h}^{(l-1)}}_{n_{l-1} \times 1} + \underbrace{\mathbf{b}^{(l)}}_{n_l \times 1} = \underbrace{\mathbf{z}^{(l)}}_{n_l \times 1}$$

For a batch of m examples: **h**^(l−1) has shape (n_{l−1} × m), and the bias **b**^(l) broadcasts across the batch dimension.

## Derivation

**Full forward pass for an L-layer network:**

Layer 0 (input): $\mathbf{h}^{(0)} = \mathbf{x} \in \mathbb{R}^{n_0}$

For l = 1, 2, ..., L:
$$\mathbf{z}^{(l)} = W^{(l)}\mathbf{h}^{(l-1)} + \mathbf{b}^{(l)}$$
$$\mathbf{h}^{(l)} = f^{(l)}(\mathbf{z}^{(l)})$$

Output: $\hat{\mathbf{y}} = \mathbf{h}^{(L)}$

For binary classification the last layer uses sigmoid; for multi-class, softmax; for regression, identity (f = linear).

**What the forward pass computes — function composition view:**

$$\hat{\mathbf{y}} = f^{(L)}\!\left(W^{(L)} f^{(L-1)}\!\left(\cdots f^{(1)}\!\left(W^{(1)}\mathbf{x} + \mathbf{b}^{(1)}\right) \cdots\right) + \mathbf{b}^{(L)}\right)$$

This is a function $F: \mathbb{R}^{n_0} \to \mathbb{R}^{n_L}$ parametrised by all W^(l), **b**^(l). Training = finding the parameter values that minimise the [[Loss Function]] over the training data.

**Storage requirement for backpropagation:**

At each layer l, we must store z^(l) (needed for computing f'(z^(l)) in the backward pass). Total memory for stored activations: O(n₁ + n₂ + ... + n_L) per training example — the dominant memory cost during training for deep networks.

## Worked Example

```python
import numpy as np

# Network: 3 → 4 → 2 → 1 (manual forward pass)
np.random.seed(0)

def relu(z):    return np.maximum(0, z)
def sigmoid(z): return 1 / (1 + np.exp(-z))

# Initialise weights: W[l] shape = (n_l, n_{l-1})
W = {
    1: np.random.randn(4, 3) * 0.1,   # layer 1: 3 → 4
    2: np.random.randn(2, 4) * 0.1,   # layer 2: 4 → 2
    3: np.random.randn(1, 2) * 0.1,   # layer 3: 2 → 1
}
b = {
    1: np.zeros((4, 1)),
    2: np.zeros((2, 1)),
    3: np.zeros((1, 1)),
}

# Input: batch of 5 examples, each with 3 features
x = np.random.randn(3, 5)  # shape: (3, 5)

# === Forward pass ===
cache = {}  # store (z, h) for backprop later

h = x
for l in [1, 2, 3]:
    z = W[l] @ h + b[l]    # linear transform
    if l < 3:
        h = relu(z)         # hidden layers: ReLU
    else:
        h = sigmoid(z)      # output layer: sigmoid (binary classification)
    cache[l] = (z, h)
    print(f"Layer {l}: z.shape={z.shape}, h.shape={h.shape}")

y_hat = h   # shape: (1, 5) — one prediction per example
print(f"\nPredictions (5 examples): {y_hat.round(3)}")

# Shape check:
# Layer 1: (4,3)@(3,5)+(4,1) = (4,5) ✓
# Layer 2: (2,4)@(4,5)+(2,1) = (2,5) ✓
# Layer 3: (1,2)@(2,5)+(1,1) = (1,5) ✓

# Compute loss (binary cross-entropy)
y_true = np.array([[1, 0, 1, 0, 1]], dtype=float)
eps = 1e-8
loss = -np.mean(y_true * np.log(y_hat + eps) + (1-y_true) * np.log(1-y_hat + eps))
print(f"Binary cross-entropy loss: {loss:.4f}")
```

## See Also
- [[Neural Network]] — the architecture the forward pass runs through
- [[Artificial Neuron]] — each layer applies a neuron's computation across all units
- [[Activation Function]] — f^(l) applied element-wise at each layer
- [[Backpropagation]] — uses the cached z^(l) and h^(l) from the forward pass to compute gradients
- [[Loss Function]] — computed after the forward pass; measures prediction error
- [[Matrix Multiplication]] — W^(l) h^(l-1) is a matrix multiply at each layer
- [[Gradient Descent]] — uses the loss (from forward pass) and gradients (from backprop) to update weights
- [[Composition over Inheritance]] — the forward pass is function composition in action: each layer's output is fed into the next, building complexity by chaining independent transformations rather than inheriting behavior
