# Matrix Multiplication

**One-liner:** The composition of two linear transformations into one — not element-wise, but row-times-column dot products that encode "apply B's transformation, then A's."

## Core Idea
$$C = AB, \quad c_{ij} = \sum_{k=1}^{p} a_{ik} b_{kj}$$
For $A \in \mathbb{R}^{m \times p}$ and $B \in \mathbb{R}^{p \times n}$, the product $C \in \mathbb{R}^{m \times n}$. Element $c_{ij}$ is the [[Dot Product]] of row $i$ of $A$ with column $j$ of $B$. The inner dimensions must match: $(m \times p)(p \times n) = (m \times n)$.

## Why It Exists
You need to compose transformations. A neural network's [[Forward Pass]] is a sequence: "encode input → compress → decode." Each step is a matrix; composing them should be a single matrix (or a chain of matrix-vector products). Matrix multiplication is how linear transformations chain — it's the reason [[Neural Network]] forward passes are just a sequence of `W @ x` operations.

## Real-World Applications
- **Deep learning forward pass:** $\mathbf{h}_2 = \text{ReLU}(W_2 \cdot \text{ReLU}(W_1 \mathbf{x} + b_1) + b_2)$ — each $W \cdot$ is matrix multiplication.
- **Batch processing:** multiply weight matrix $W$ by input matrix $X$ where each *column* of $X$ is one example — compute all examples simultaneously.
- **Robot kinematics:** multiply homogeneous transform matrices to find end-effector pose: $T_{base}^{end} = T_1 T_2 \cdots T_n$.
- **Transformer attention:** `Attention(Q,K,V) = softmax(QK^T/√d)V` — three matrix multiplications.
- **Graphics:** rotation, scaling, and translation as matrix products — every 3D game does this.

## Intuition
The key mental model: **matrix multiplication composes functions**. If $B$ rotates space 45° and $A$ scales it by 2, then $AB$ first rotates then scales. The columns of $B$ tell you where the input basis vectors land after $B$; $A$ then transforms those results.

Element-wise multiplication (`*` in NumPy) is *not* this — it's a completely different operation (Hadamard product) that has no transformation interpretation.

**Why inner dimensions must match:** $B$ outputs $p$-dimensional vectors; $A$ must accept $p$-dimensional input to continue the chain. The $p$ must be the same.

## Derivation
**Shape rule derivation from composition:**
$B: \mathbb{R}^n \to \mathbb{R}^p$ and $A: \mathbb{R}^p \to \mathbb{R}^m$
Composing: $A \circ B: \mathbb{R}^n \to \mathbf{R}^m$, so result is $m \times n$. ✓

**Why AB ≠ BA (non-commutativity):**
"Rotate then scale" ≠ "Scale then rotate" in general.
Also, if $A$ is $3 \times 2$ and $B$ is $2 \times 3$, then $AB$ is $3 \times 3$ but $BA$ is $2 \times 2$ — totally different shapes.

**Associativity: $(AB)C = A(BC)$**
This is crucial for efficiency: choose the parenthesization that minimizes operations.

**Transpose rule:** $(AB)^T = B^T A^T$ — order reverses.

## Worked Example
```python
import numpy as np

# Basic matrix multiplication — shape (2,3) @ (3,2) = (2,2)
A = np.array([[1, 2, 3],
              [4, 5, 6]])   # (2, 3)
B = np.array([[7,  8],
              [9,  10],
              [11, 12]])    # (3, 2)

C = A @ B   # preferred over np.matmul or np.dot
print(C)
# [[ 58  64]
#  [139 154]]
# Verify c[0,0] = row0(A) · col0(B) = 1*7 + 2*9 + 3*11 = 58 ✓

# Shape check — this is the most common error in deep learning
print(f"A: {A.shape}, B: {B.shape}, C: {C.shape}")
# A: (2, 3), B: (3, 2), C: (2, 2)

# Batch processing: X has 32 examples, each of dim 784
W = np.random.randn(512, 784)  # weight matrix
X = np.random.randn(784, 32)   # 32 inputs stacked as columns
H = W @ X                       # (512, 32) — all 32 examples at once
print(f"Batch output shape: {H.shape}")  # (512, 32)

# Non-commutativity demonstration
A2 = np.array([[1, 2], [3, 4]])
B2 = np.array([[0, 1], [1, 0]])  # swap rows
print("AB:\n", A2 @ B2)
print("BA:\n", B2 @ A2)   # different!

# Composition: two rotations
def rot_2d(theta):
    return np.array([[np.cos(theta), -np.sin(theta)],
                     [np.sin(theta),  np.cos(theta)]])

R45  = rot_2d(np.pi/4)   # rotate 45°
R90  = rot_2d(np.pi/2)   # rotate 90°
R135 = rot_2d(3*np.pi/4) # rotate 135°

# 45° then 90° should equal 135°
composed = R90 @ R45
print(np.allclose(composed, R135))  # True
```

## See Also
- [[Matrix]] — the operands; understand matrix structure first
- [[Dot Product]] — each output element $c_{ij}$ is one dot product
- [[Vector]] — a matrix-vector product Ax is the most common use case
- [[NumPy Array]] — `@` operator or `np.matmul`; never element-wise `*`
- [[Forward Pass]] — neural network layers are matrix multiplications
- [[Backpropagation]] — computes gradients via transposed matrix multiplications
- [[Node Voltage Method]] — circuit equations Gv = I are a system of linear equations solved by matrix multiplication; the same operation that transforms layer inputs into outputs in a neural network
- [[Kinematic Equations]] — rotation matrices transform position and velocity vectors; chaining joint transforms in robot kinematics is a sequence of matrix multiplications
