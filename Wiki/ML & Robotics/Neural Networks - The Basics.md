# Neural Networks - The Basics

**One-liner:** A neural network is a parameterized function composed of stacked layers of linear transformations and nonlinearities, trained by gradient descent to approximate any target function from data.

## Why It Exists

Consider the problem of predicting whether a robot's grasp will succeed given a sensor reading. You could hand-craft a rule: "if grip force > threshold AND contact area > threshold, then success." But what threshold? What if there are 50 sensor inputs with complex interactions?

Feature engineering — manually designing the right input transformations — is fragile, domain-specific, and doesn't scale. Neural networks solve this by **learning the feature representations directly from data** rather than requiring humans to design them.

The theoretical justification comes from the **Universal Approximation Theorem** (Cybenko, 1989; Hornik, 1991): a feedforward neural network with a single hidden layer of sufficient width can approximate any continuous function on a compact domain to arbitrary precision. Depth offers further advantages: deep networks can represent certain function classes exponentially more efficiently than shallow networks.

For robotics specifically, neural networks enable:
- **Perception:** Learning to map raw sensor data (images, LIDAR, force sensors) to semantic representations without manual feature extraction
- **Policy learning:** Representing policies $\pi(a|s)$ that map high-dimensional state spaces to continuous action spaces — too complex for tabular methods
- **Value function approximation:** Representing $Q(s,a)$ or $V(s)$ over continuous state spaces in deep RL
- **Inverse kinematics and dynamics:** Learning complex nonlinear mappings from task space to joint space

## The Concept

### What a Single Neuron Computes

The artificial neuron is a scalar-output function of a vector input:

$$z = \mathbf{w}^T \mathbf{x} + b = \sum_{i=1}^{n} w_i x_i + b$$
$$a = \sigma(z)$$

where:
- $\mathbf{x} \in \mathbb{R}^n$ — the input vector
- $\mathbf{w} \in \mathbb{R}^n$ — the weight vector (learned parameters)
- $b \in \mathbb{R}$ — the bias (learned parameter)
- $z$ — the pre-activation (a weighted sum)
- $\sigma$ — the activation function (a fixed nonlinearity)
- $a$ — the activation (the neuron's output)

The weight vector $\mathbf{w}$ determines which input dimensions the neuron "cares about." The bias shifts the threshold. The activation function introduces nonlinearity — without it, the entire network collapses to a single linear transformation regardless of depth.

This is biologically inspired by actual neurons: weighted input synapses, a threshold, an output signal. But take this analogy loosely — artificial neurons are a mathematical abstraction, not a simulation of biology.

### Why Nonlinearity Is Necessary

Without activation functions, any stack of linear layers is just one linear layer:

$$\mathbf{W}_2(\mathbf{W}_1 \mathbf{x} + \mathbf{b}_1) + \mathbf{b}_2 = (\mathbf{W}_2 \mathbf{W}_1)\mathbf{x} + (\mathbf{W}_2 \mathbf{b}_1 + \mathbf{b}_2) = \mathbf{W}'\mathbf{x} + \mathbf{b}'$$

The composition of linear functions is linear. A linear network can only represent linear decision boundaries — it cannot learn XOR, let alone robot grasping.

The nonlinearity $\sigma$ breaks this collapse. With nonlinearities, deeper networks can represent exponentially more complex functions.

### Common Activation Functions

**ReLU** (Rectified Linear Unit): $\sigma(z) = \max(0, z)$

The default choice for hidden layers. Simple, computationally cheap, avoids the vanishing gradient problem better than sigmoids.

**Sigmoid**: $\sigma(z) = \frac{1}{1 + e^{-z}}$, output in $(0, 1)$

Used in binary classification output layers. Suffers from vanishing gradients in deep networks — saturated neurons (extreme $z$ values) have near-zero gradient.

**Tanh**: $\sigma(z) = \tanh(z)$, output in $(-1, 1)$

Zero-centered (unlike sigmoid), sometimes better for hidden layers. Also has vanishing gradient issues at saturation.

**Softmax**: $\sigma(\mathbf{z})_i = \frac{e^{z_i}}{\sum_j e^{z_j}}$

Used in multi-class classification output layers. Converts a vector of logits to a probability distribution (outputs sum to 1).

### Layers and Architecture

A neural network is organized into **layers**:
- **Input layer:** The raw input (not a transformation layer, just the data)
- **Hidden layers:** Intermediate layers that learn representations
- **Output layer:** Final layer whose output is the network's prediction

For a network with $L$ layers, denote the output of layer $l$ as $\mathbf{a}^{[l]}$. The input is $\mathbf{a}^{[0]} = \mathbf{x}$.

Each hidden layer $l$ computes:
$$\mathbf{z}^{[l]} = \mathbf{W}^{[l]} \mathbf{a}^{[l-1]} + \mathbf{b}^{[l]}$$
$$\mathbf{a}^{[l]} = \sigma\left(\mathbf{z}^{[l]}\right)$$

where $\mathbf{W}^{[l]}$ is the weight matrix of layer $l$ (shape: number of neurons in layer $l$ × number of neurons in layer $l-1$), and $\sigma$ is applied elementwise.

### The Forward Pass

The **forward pass** is simply evaluating the network: propagate input $\mathbf{x}$ through each layer in order to produce the output $\hat{\mathbf{y}}$.

```python
import numpy as np

def relu(z):
    return np.maximum(0, z)

def forward_pass(x, weights, biases):
    """
    x: input vector, shape (n_inputs,)
    weights: list of weight matrices [W1, W2, ...]
    biases: list of bias vectors [b1, b2, ...]
    Returns: final activation (network output)
    """
    a = x
    for W, b in zip(weights[:-1], biases[:-1]):
        z = W @ a + b         # linear transformation
        a = relu(z)           # nonlinear activation
    # Output layer — no activation (regression) or softmax (classification)
    z = weights[-1] @ a + biases[-1]
    return z   # raw output (logits)

# Example: 3-layer network, 4 inputs → 8 hidden → 4 hidden → 2 outputs
W1 = np.random.randn(8, 4) * 0.1
W2 = np.random.randn(4, 8) * 0.1
W3 = np.random.randn(2, 4) * 0.1
b1 = np.zeros(8)
b2 = np.zeros(4)
b3 = np.zeros(2)

x = np.array([0.5, -0.3, 0.8, 0.1])
output = forward_pass(x, [W1, W2, W3], [b1, b2, b3])
print(output.shape)  # (2,)
```

### Why Depth Matters

A shallow network can approximate any function in theory, but requires exponentially many neurons. A deep network can represent the same function with polynomially many neurons by composing simpler functions.

Consider recognizing an image of a Baymax. A deep network naturally learns a hierarchy:
- Layer 1: edges and gradients
- Layer 2: corners, curves, texture patches  
- Layer 3: eyes, hands, torso shapes
- Layer 4: combinations that form the full robot

Each layer builds on the previous. This compositional structure maps onto how many real-world signals are generated — and it's why deep networks work better than shallow ones on perceptual tasks.

### Connecting Forward Pass to Gradient Descent

After the forward pass produces $\hat{\mathbf{y}}$, we compute the **loss** $L(\hat{\mathbf{y}}, \mathbf{y})$:
- Mean squared error for regression: $L = \frac{1}{n}\|\hat{\mathbf{y}} - \mathbf{y}\|^2$
- Cross-entropy for classification: $L = -\sum_i y_i \log \hat{y}_i$

To minimize $L$, we need $\frac{\partial L}{\partial \mathbf{W}^{[l]}}$ and $\frac{\partial L}{\partial \mathbf{b}^{[l]}}$ for every layer. Computing these efficiently is **backpropagation** — the chain rule applied recursively from the output layer backward.

The update rule (gradient descent):
$$\mathbf{W}^{[l]} \leftarrow \mathbf{W}^{[l]} - \alpha \frac{\partial L}{\partial \mathbf{W}^{[l]}}$$

This is how training works: forward pass (compute loss) → backpropagation (compute gradients) → gradient descent (update weights) → repeat.

### Batched Forward Pass

In practice, we process batches of examples simultaneously (this is why GPUs matter — they parallelize over the batch dimension).

For a batch of $B$ examples stacked as $\mathbf{X} \in \mathbb{R}^{B \times n}$, the layer computation becomes:

$$\mathbf{Z}^{[l]} = \mathbf{A}^{[l-1]} \mathbf{W}^{[l]T} + \mathbf{b}^{[l]}$$

(shape: $B \times d_l$, where $d_l$ is the number of neurons in layer $l$). This single matrix multiplication processes all $B$ examples in the time it takes to process one — that's the GPU speedup.

```python
# Batched forward pass
B = 32   # batch size
n = 4    # input dimension
X = np.random.randn(B, n)          # shape (32, 4)
W1 = np.random.randn(8, n) * 0.1   # shape (8, 4)
b1 = np.zeros(8)

Z1 = X @ W1.T + b1   # shape (32, 8) — broadcasts b1 across batch
A1 = relu(Z1)        # shape (32, 8)
print(A1.shape)  # (32, 8)
```

## Intuition

Think of each layer as a coordinate system transformation. The input $\mathbf{x}$ lives in some space where the data is tangled — a spiral, say, where no straight line can separate classes. Each layer warps and folds this space. After enough layers, the data becomes linearly separable — the final linear layer can draw a straight decision boundary.

The weights control how the space is warped. Training adjusts the weights so the final representation (the last hidden layer's activations) is linearly separable with respect to the task.

Each neuron is a feature detector. After training, neurons in early layers of image networks respond to edges in specific orientations; neurons in middle layers respond to shapes; neurons in late layers respond to high-level concepts. This is not programmed — it emerges from training.

The depth adds expressiveness: you can represent more complex warps by composing simpler ones. A single layer can translate and rotate; stacked layers can fold, stretch, and reshape in ways a single layer cannot.

## Key Formula / Rule

**Single layer forward pass:**
$$\mathbf{a}^{[l]} = \sigma\left(\mathbf{W}^{[l]} \mathbf{a}^{[l-1]} + \mathbf{b}^{[l]}\right)$$

**MSE Loss for regression:**
$$L = \frac{1}{B} \sum_{i=1}^{B} \|\hat{\mathbf{y}}_i - \mathbf{y}_i\|^2$$

**Gradient for output layer weights** (MSE loss):
$$\frac{\partial L}{\partial \mathbf{W}^{[L]}} = \frac{2}{B} (\hat{\mathbf{Y}} - \mathbf{Y})^T \mathbf{A}^{[L-1]}$$

```python
import numpy as np

# Minimal: train a 1-hidden-layer network on XOR
X = np.array([[0,0],[0,1],[1,0],[1,1]], dtype=float)
y = np.array([[0],[1],[1],[0]], dtype=float)  # XOR labels

np.random.seed(0)
W1 = np.random.randn(4, 2) * 0.5
b1 = np.zeros((1, 4))
W2 = np.random.randn(1, 4) * 0.5
b2 = np.zeros((1, 1))
alpha = 0.5

for _ in range(10000):
    # Forward pass
    Z1 = X @ W1.T + b1     # (4, 4)
    A1 = np.maximum(0, Z1) # ReLU
    Z2 = A1 @ W2.T + b2    # (4, 1)
    A2 = 1 / (1 + np.exp(-Z2))  # sigmoid output

    # Loss
    loss = -np.mean(y * np.log(A2 + 1e-8) + (1-y) * np.log(1-A2 + 1e-8))

    # Backward pass (chain rule)
    dA2 = (A2 - y) / 4
    dW2 = dA2.T @ A1
    db2 = dA2.sum(axis=0, keepdims=True)
    dA1 = dA2 @ W2
    dZ1 = dA1 * (Z1 > 0)  # ReLU derivative
    dW1 = dZ1.T @ X
    db1 = dZ1.sum(axis=0, keepdims=True)

    W1 -= alpha * dW1
    b1 -= alpha * db1
    W2 -= alpha * dW2
    b2 -= alpha * db2

predictions = (A2 > 0.5).astype(int)
print("XOR predictions:", predictions.flatten())  # Should be [0, 1, 1, 0]
```

## Worked Example

**Problem:** Understand exactly what happens at each step of a 2-layer network doing a forward pass and computing one gradient update.

```python
import numpy as np

# Tiny network: 2 inputs → 3 hidden (ReLU) → 1 output
# Single training example
x = np.array([0.5, -0.3])          # input
y_true = 1.0                        # target

# Initialize weights (small random)
W1 = np.array([[0.4, -0.2],
               [0.1,  0.8],
               [-0.5, 0.3]])       # shape (3, 2)
b1 = np.array([0.0, 0.0, 0.0])    # shape (3,)
W2 = np.array([[0.6, -0.4, 0.2]]) # shape (1, 3)
b2 = np.array([0.0])               # shape (1,)
alpha = 0.1

# === FORWARD PASS ===
# Layer 1
z1 = W1 @ x + b1        # [0.4*0.5 + (-0.2)*(-0.3), ...] = [0.26, 0.29, -0.34]
a1 = np.maximum(0, z1)  # ReLU: [0.26, 0.29, 0.0]   ← negative clamped to 0

# Layer 2 (output)
z2 = W2 @ a1 + b2       # 0.6*0.26 + (-0.4)*0.29 + 0.2*0.0 = 0.156 - 0.116 = 0.040
y_hat = z2[0]           # 0.040

print(f"z1 = {z1.round(3)}")
print(f"a1 = {a1.round(3)}")
print(f"y_hat = {y_hat:.4f}")

# === LOSS ===
loss = 0.5 * (y_hat - y_true)**2   # MSE (factor of 0.5 simplifies gradient)
print(f"Loss = {loss:.4f}")        # 0.5 * (0.040 - 1.0)^2 = 0.461

# === BACKWARD PASS ===
# dL/dy_hat = y_hat - y_true
dL_dyhat = y_hat - y_true          # -0.960

# Gradient through W2: dL/dW2 = dL/dz2 * dz2/dW2 = dL/dz2 * a1^T
dL_dz2 = dL_dyhat                  # linear layer, no activation
dL_dW2 = dL_dz2 * a1              # outer product (scalar * vector)

# Gradient through Layer 1: chain through W2 weights, then through ReLU
dL_da1 = W2[0] * dL_dz2            # shape (3,)
dL_dz1 = dL_da1 * (z1 > 0)        # ReLU derivative: 1 where z1>0, 0 elsewhere
dL_dW1 = np.outer(dL_dz1, x)       # shape (3, 2)

print(f"\ndL/dW2 = {dL_dW2.round(4)}")
print(f"dL/dW1 =\n{dL_dW1.round(4)}")

# === UPDATE ===
W2 -= alpha * dL_dW2
W1 -= alpha * dL_dW1

print(f"\nAfter 1 update, y_hat would be closer to 1.0")
# Run forward again to verify
z1_new = W1 @ x + b1
a1_new = np.maximum(0, z1_new)
y_hat_new = (W2 @ a1_new + b2)[0]
print(f"New y_hat = {y_hat_new:.4f}")  # Should be closer to 1.0
```

This traces every number through the forward and backward pass. The third neuron in layer 1 had $z_1 = -0.34$, so ReLU zeroed it out — its weight receives zero gradient (it's "dead" in this example). This is the **dying ReLU problem** at the single-step level.

## Gotchas

**Gotcha 1 — `*` is elementwise, `@` is matrix multiplication.** In NumPy, `W * a` is elementwise multiplication — almost certainly wrong in a neural network. `W @ a` is matrix multiplication — what you want for a linear layer. This confusion causes bugs that produce wrong-shaped outputs or wrong numerical results.

**Gotcha 2 — Dying ReLU neurons.** If a neuron's pre-activation $z$ is negative for every training example (can happen with large negative biases or large weight updates), the gradient is always zero (ReLU derivative is 0 for negative inputs). The neuron stops learning. Fix: use Leaky ReLU ($\max(0.01z, z)$) which has a small gradient for negative inputs, or careful weight initialization.

**Gotcha 3 — Vanishing gradients in deep sigmoid networks.** Sigmoid's gradient $\sigma'(z) = \sigma(z)(1-\sigma(z))$ peaks at 0.25 for $z=0$ and approaches 0 for large $|z|$. In a 10-layer sigmoid network, the gradient of the first layer is the product of 10 such terms — easily $< 10^{-6}$. The first layers effectively don't train. Use ReLU in deep networks, and consider batch normalization or residual connections.

**Gotcha 4 — Feature scaling matters a lot.** If input features have very different scales (e.g., one feature is 0-1 and another is 0-10,000), the weights for the large-scale feature will be much smaller, and gradient descent will navigate an elongated, poorly-conditioned loss landscape. Always normalize input features (subtract mean, divide by std) before training.

**Gotcha 5 — Weight initialization.** If all weights are initialized to zero, all neurons compute the same function, receive the same gradient, and learn identically — the hidden layer might as well be a single neuron. Initialize weights randomly (He initialization for ReLU: $\mathcal{N}(0, \sqrt{2/n_{in}})$; Xavier for tanh: $\mathcal{N}(0, \sqrt{1/n_{in}})$).

**Gotcha 6 — Overfitting: memorizing, not learning.** A large enough network can memorize the training set without learning any generalizable patterns. The loss on training data goes to zero while validation loss increases. Fixes: dropout (randomly zero neurons during training), weight decay (L2 regularization), more training data, early stopping.

**Gotcha 7 — Batch size affects training dynamics, not just speed.** Small batches add gradient noise, which acts as regularization and helps escape sharp minima. Large batches compute accurate gradients but may converge to sharp minima that generalize poorly. The typical recommendation for RL is small batches (32-256); for supervised learning, experiment.

## See Also

- [[Gradient Descent]] — how neural networks are trained; backpropagation computes the gradients, gradient descent applies them
- [[NumPy Arrays]] — the entire forward and backward pass is NumPy matrix and array operations; proficiency here directly enables neural net coding
- [[Matrix Multiplication]] — every linear layer is a matrix multiplication; the shapes of W, x, and their product determine the layer architecture
- [[Chain Rule]] — backpropagation IS the chain rule; you must understand it to derive gradients manually or debug training
- [[What is Reinforcement Learning]] — in deep RL, the policy and value function are neural networks; this entry defines the function approximator that deep RL uses
- [[Markov Decision Processes]] — the objective that deep RL neural networks optimize; the network structure and the MDP structure determine the full RL system
