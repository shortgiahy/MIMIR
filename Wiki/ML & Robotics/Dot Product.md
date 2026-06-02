# Dot Product

**One-liner:** A single-number summary of how aligned two vectors are — the cosine of their angle, scaled by their magnitudes.

## Core Idea
$$\mathbf{a} \cdot \mathbf{b} = \sum_{i=1}^{n} a_i b_i = ||\mathbf{a}|| \cdot ||\mathbf{b}|| \cos\theta$$
Two equivalent definitions: (1) multiply corresponding elements and sum; (2) product of magnitudes times cosine of angle between them. Both give the same number. The result is a [[Scalar]] — a single number measuring alignment.

## Why It Exists
You constantly need to ask "how similar are these two things?" in ML:
- Is this input similar to this weight vector? → dot product
- How strongly does this direction contribute to that gradient? → dot product
- What's the attention score between this query and key? → dot product

The dot product is the mathematical operation that answers "similarity" in a geometrically meaningful, computationally cheap way. Without it, [[Matrix Multiplication]], [[Neural Network]] attention, and cosine similarity don't exist.

## Real-World Applications
- **Neural network computation:** every neuron computes $\mathbf{w} \cdot \mathbf{x} + b$ — a dot product of weights and inputs.
- **Cosine similarity (NLP):** how similar is "king" to "queen" in embedding space? `cos_sim = a·b / (||a||·||b||)`.
- **Attention mechanism (transformers):** `score = query · key` — scaled dot-product attention is literally this.
- **Baymax obstacle avoidance:** dot product between velocity vector and surface normal tells you if you're moving toward an obstacle.
- **Projection:** `proj_b(a) = (a·b / b·b) * b` — how far does **a** extend in the direction of **b**?

## Intuition
Three cases to internalize:
1. **Same direction (θ=0°):** $\cos 0 = 1$, dot product = $||\mathbf{a}||||\mathbf{b}||$ (max positive)
2. **Perpendicular (θ=90°):** $\cos 90° = 0$, dot product = 0 (no alignment)
3. **Opposite direction (θ=180°):** $\cos 180° = -1$, dot product = $-||\mathbf{a}||||\mathbf{b}||$ (max negative)

A neuron's weights form a "preferred direction vector." When the input aligns with that direction, the dot product is large → strong activation. The neuron is a *directional detector*.

## Derivation
**Algebraic → geometric equivalence:**

Start with the Law of Cosines applied to the triangle formed by **a**, **b**, and **a**-**b**:
$$||\mathbf{a} - \mathbf{b}||^2 = ||\mathbf{a}||^2 + ||\mathbf{b}||^2 - 2||\mathbf{a}||||\mathbf{b}||\cos\theta$$

Expand the left side using element-wise definition:
$$||\mathbf{a} - \mathbf{b}||^2 = \sum_i (a_i - b_i)^2 = \sum_i a_i^2 - 2\sum_i a_i b_i + \sum_i b_i^2 = ||\mathbf{a}||^2 - 2(\mathbf{a} \cdot \mathbf{b}) + ||\mathbf{b}||^2$$

Setting equal:
$$||\mathbf{a}||^2 - 2(\mathbf{a} \cdot \mathbf{b}) + ||\mathbf{b}||^2 = ||\mathbf{a}||^2 + ||\mathbf{b}||^2 - 2||\mathbf{a}||||\mathbf{b}||\cos\theta$$
$$\Rightarrow \mathbf{a} \cdot \mathbf{b} = ||\mathbf{a}||||\mathbf{b}||\cos\theta \quad \blacksquare$$

## Worked Example
```python
import numpy as np

a = np.array([1.0, 2.0, 3.0])
b = np.array([4.0, 5.0, 6.0])

# Algebraic definition
dot_algebraic = np.sum(a * b)
print(f"Algebraic: {dot_algebraic}")  # 32.0

# NumPy shorthand
dot_numpy = np.dot(a, b)   # or a @ b for 1D vectors
print(f"NumPy: {dot_numpy}")          # 32.0

# Geometric: via angle
cos_theta = dot_numpy / (np.linalg.norm(a) * np.linalg.norm(b))
theta_deg = np.degrees(np.arccos(cos_theta))
print(f"Angle between a and b: {theta_deg:.2f}°")  # 12.93°

# Cosine similarity (normalized dot product)
def cosine_similarity(u, v):
    return np.dot(u, v) / (np.linalg.norm(u) * np.linalg.norm(v))

king  = np.array([0.9, 0.2, 0.8])
queen = np.array([0.8, 0.9, 0.7])
cat   = np.array([0.1, 0.1, 0.2])
print(f"king·queen sim: {cosine_similarity(king, queen):.3f}")  # ~0.95
print(f"king·cat sim:   {cosine_similarity(king, cat):.3f}")    # ~0.60

# A single neuron: weighted sum = dot product
weights = np.array([0.5, -0.3, 0.8])
bias    = 0.1
x_input = np.array([1.0, 0.5, 2.0])
output = np.dot(weights, x_input) + bias
print(f"Neuron pre-activation: {output:.3f}")  # 1.85
```

## See Also
- [[Vector]] — dot product is defined on two vectors of the same length
- [[Scalar]] — the result of a dot product is always a scalar
- [[Matrix Multiplication]] — generalization of dot product; each output element is a dot product
- [[Artificial Neuron]] — computes w·x + b, a dot product plus bias
- [[Activation Function]] — applied after the dot product
- [[Gradient]] — the gradient direction is found via dot products with basis vectors
- [[Work]] — W = F·d = |F||d|cosθ is the dot product of force and displacement vectors; the same cosine-alignment formula as the attention score in transformers
- [[Voltage]] — in EE, power P = V·I uses the same scalar-from-vectors structure; in circuits with phasors, real power is the dot product of voltage and current vectors
