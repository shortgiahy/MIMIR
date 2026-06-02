# Matrix

**One-liner:** A 2D rectangular array of numbers that represents a linear transformation — a precise rule for moving vectors from one space to another.

## Core Idea
$$A \in \mathbb{R}^{m \times n}, \quad A = \begin{bmatrix} a_{11} & a_{12} & \cdots & a_{1n} \\ a_{21} & a_{22} & \cdots & a_{2n} \\ \vdots & & \ddots & \vdots \\ a_{m1} & a_{m2} & \cdots & a_{mn} \end{bmatrix}$$
An $m \times n$ matrix has $m$ rows and $n$ columns. It transforms any $n$-dimensional input vector into an $m$-dimensional output vector via [[Matrix Multiplication]]: $\mathbf{y} = A\mathbf{x}$. That transformation preserves lines and the origin — that's the definition of *linear*.

## Why It Exists
Vectorized computation requires a way to apply the *same* transformation to every element simultaneously. A matrix lets you encode a system of linear equations, a coordinate rotation, a neural network layer, or a robot's kinematics into one compact object that can be multiplied in a single hardware-optimized operation. Without matrices, you'd loop over equations one by one — slow and unstructured.

## Real-World Applications
- **Neural network weight matrix:** a 512×784 matrix transforms 784 pixel values to 512 activations in one operation (MNIST classifier).
- **Robot kinematics:** rotation matrices ($3 \times 3$) and homogeneous transform matrices ($4 \times 4$) describe how joints move relative to each other — essential for Baymax's arm planning.
- **Covariance matrix:** encodes uncertainty in a Kalman filter — Baymax needs this for sensor fusion.
- **Image convolution:** can be expressed as sparse matrix multiplication.
- **Principal Component Analysis:** eigenvectors of a covariance matrix are the "directions of variance."

## Intuition
A $2 \times 2$ matrix is a machine: feed it a 2D vector, get a transformed 2D vector. The columns of the matrix tell you where the basis vectors $\hat{x}$ and $\hat{y}$ land after transformation. If column 1 is `[2, 0]` and column 2 is `[0, 3]`, the matrix stretches x by 2 and y by 3. If it has off-diagonal terms, it also shears or rotates. The whole transformation is determined by where the basis vectors go — everything else follows by linearity.

## Derivation
**Matrix-vector product** (the fundamental operation):
$$\mathbf{y} = A\mathbf{x}, \quad y_i = \sum_{j=1}^{n} a_{ij} x_j$$
Row $i$ of $A$ dotted with $\mathbf{x}$ gives output element $y_i$.

**Transpose:** $(A^T)_{ij} = A_{ji}$ — flip rows and columns. Important for backprop.

**Identity matrix:** $I\mathbf{x} = \mathbf{x}$ — the "do nothing" transformation.

**Inverse (square matrices):** $A^{-1}A = I$ — undoes the transformation. Exists only when $\det(A) \neq 0$ (non-degenerate transformation).

**Why matrix products aren't commutative:** $AB \neq BA$ in general because "do A then B" is a different transformation than "do B then A."

## Worked Example
```python
import numpy as np

# 2x3 matrix: maps R^3 -> R^2
A = np.array([[1, 2, 3],
              [4, 5, 6]])   # shape (2, 3)

# A vector in R^3
x = np.array([1.0, 0.0, -1.0])  # shape (3,)

# Matrix-vector product: R^3 -> R^2
y = A @ x
print(y)          # [1-3, 4-6] = [-2., -2.]
print(y.shape)    # (2,)

# Rotation matrix: rotates 90° counterclockwise
theta = np.pi / 2
R = np.array([[np.cos(theta), -np.sin(theta)],
              [np.sin(theta),  np.cos(theta)]])
v = np.array([1.0, 0.0])   # pointing right
print(R @ v)  # [~0, 1] — now pointing up

# Neural network layer: W @ x + b
W = np.random.randn(512, 784)  # weight matrix
x_img = np.random.randn(784)   # flattened MNIST image
b = np.zeros(512)
activations = W @ x_img + b    # shape (512,)
```

## See Also
- [[Scalar]] — a $1 \times 1$ matrix (degenerate case)
- [[Vector]] — a column vector is an $n \times 1$ matrix
- [[Matrix Multiplication]] — how matrices compose transformations
- [[Dot Product]] — one row of A dotted with x gives one output element
- [[NumPy Array]] — ndarray with ndim=2 is a matrix
- [[Neural Network]] — weight matrices are the learnable parameters
- [[Forward Pass]] — matrix multiplications chain through layers
- [[Node Voltage Method]] — the node voltage method formulates circuit equations as a matrix system Gv = I, where the conductance matrix G and source vector I have the same structure as a neural network layer's weight matrix and bias
- [[Kinematic Equations]] — rotation matrices (3×3) and homogeneous transform matrices (4×4) are the primary tool for expressing robot kinematics; chaining them multiplies matrices
