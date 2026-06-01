# Vector

**One-liner:** An ordered list of numbers that simultaneously encodes magnitude and direction in n-dimensional space.

## Core Idea
$$\mathbf{v} = \begin{bmatrix} v_1 \\ v_2 \\ \vdots \\ v_n \end{bmatrix} \in \mathbb{R}^n$$
A vector lives in n-dimensional space and has two inseparable properties: *how big* (magnitude: $||\mathbf{v}|| = \sqrt{\sum_i v_i^2}$) and *which way* (direction: $\hat{v} = \mathbf{v}/||\mathbf{v}||$). The geometric view (arrow in space) and algebraic view (list of numbers) are two faces of the same object.

## Why It Exists
A single number can't capture state. A robot needs to know position in x, y, z — that's a 3-vector. A neural net's layer of 512 neurons produces a 512-vector of activations. A gradient is a vector of partial derivatives — one per parameter. Vectors let you work with structured, multi-dimensional data as a single mathematical object you can add, scale, and compare.

## Real-World Applications
- **Sensor fusion in Baymax:** IMU readings (3 accel + 3 gyro) = 6-vector of state.
- **Word embeddings:** "king" is represented as a 300-vector in Word2Vec. Semantics encode as geometry.
- **Neural net activations:** A hidden layer outputs a vector; each element is one neuron's activation.
- **Gradient:** ∇L is a vector — one partial derivative per model parameter.
- **Robot joint angles:** A 7-DOF arm's configuration is a 7-vector.

## Intuition
A 2D vector `[3, 4]` is an arrow from the origin to the point (3, 4). Its magnitude is 5 (Pythagorean theorem). The direction is 53° from the x-axis. Nothing mystical — it's coordinates. In ML, the "direction" interpretation is critical: two vectors pointing the same way have high [[Dot Product]], meaning they're similar. The neural network is constantly measuring alignment between weight vectors and input vectors.

## Derivation
**Vector addition (geometric):** tip-to-tail concatenation. Algebraically, add element-wise:
$$\mathbf{a} + \mathbf{b} = \begin{bmatrix} a_1 + b_1 \\ a_2 + b_2 \end{bmatrix}$$

**Magnitude (L2 norm):**
$$||\mathbf{v}||_2 = \sqrt{v_1^2 + v_2^2 + \cdots + v_n^2}$$

**Unit vector (normalization):** preserves direction, sets magnitude to 1:
$$\hat{v} = \frac{\mathbf{v}}{||\mathbf{v}||_2}$$

The [[Dot Product]] between two vectors $\mathbf{a} \cdot \mathbf{b} = ||\mathbf{a}|| \cdot ||\mathbf{b}|| \cos\theta$ — magnitude product scaled by cosine of angle. Zero when perpendicular, maximal when parallel.

## Worked Example
```python
import numpy as np

# A 3D vector
v = np.array([3.0, 4.0, 0.0])

# Magnitude (L2 norm)
magnitude = np.linalg.norm(v)
print(f"||v|| = {magnitude}")  # 5.0

# Unit vector
unit_v = v / magnitude
print(f"unit: {unit_v}")  # [0.6, 0.8, 0.0]
print(f"||unit_v|| = {np.linalg.norm(unit_v):.4f}")  # 1.0000

# Vector addition
a = np.array([1.0, 2.0])
b = np.array([3.0, -1.0])
print(a + b)  # [4. 1.]

# Dot product: measures similarity
word_king  = np.array([0.9, 0.1, 0.8])  # toy embedding
word_queen = np.array([0.8, 0.9, 0.7])
word_cat   = np.array([0.1, 0.1, 0.2])
print(np.dot(word_king, word_queen))  # high — similar
print(np.dot(word_king, word_cat))    # low — dissimilar
```

## See Also
- [[Scalar]] — a single number; a 0D "vector"
- [[Matrix]] — a 2D array; a [[Matrix Multiplication]] maps one vector to another
- [[Dot Product]] — the fundamental operation between two vectors
- [[Gradient]] — the gradient ∇f is a vector of partial derivatives
- [[NumPy Array]] — how vectors are represented in code
- [[Derivative]] — each component of a gradient is a partial derivative
