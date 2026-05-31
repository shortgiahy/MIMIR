# NumPy Arrays

**One-liner:** NumPy's `ndarray` is a fixed-type, multi-dimensional array that replaces Python loops with C-speed vectorized operations — the foundation of all numerical computing in Python.

## Why It Exists

Python is slow at math. Not "a little slow" — orders of magnitude slow. A Python `for` loop that adds two lists of a million numbers takes roughly 500ms. NumPy does it in under 1ms. That's a 500× speedup.

The reason is fundamental: Python lists are arrays of *pointers* to arbitrary Python objects. Each object carries type information, reference counts, and other overhead. When you loop over a list doing arithmetic, Python checks types at every step, dispatches to the right C function, unpacks the object, does the math, repacks the result — for every single element.

NumPy arrays store data as a contiguous block of raw bytes in memory, all the same type (e.g., 64-bit floats). No pointers, no Python objects, no type-checking per element. When you add two NumPy arrays, a single C loop runs over raw memory. Modern CPUs can also apply SIMD (Single Instruction Multiple Data) vectorization — processing 4 or 8 floats in a single clock cycle.

For robotics and ML, this isn't a nice-to-have — it's a hard requirement. Training a neural network involves billions of floating-point operations. Running a physics simulation has thousands of matrix multiplications per timestep. You cannot do this in pure Python. NumPy (and its GPU cousin, PyTorch tensors) makes the difference between "runs in an hour" and "runs in a day."

## The Concept

### The ndarray

The core object is `numpy.ndarray`. Key properties:

- **dtype** — the type of every element (e.g., `float64`, `int32`, `bool`). All elements share one type. This is what makes the memory layout efficient.
- **shape** — a tuple describing the dimensions. `(3,)` is a 1D array of 3 elements. `(3, 4)` is a 2D array (matrix) with 3 rows, 4 columns. `(2, 3, 4)` is a 3D array.
- **ndim** — number of dimensions (length of the shape tuple).
- **strides** — how many bytes to skip to move one step along each axis. This is what makes slicing essentially free — a slice is just a new view with different strides, not a copy.

```python
import numpy as np

a = np.array([1.0, 2.0, 3.0])
print(a.shape)   # (3,)
print(a.dtype)   # float64
print(a.ndim)    # 1

M = np.array([[1, 2, 3],
              [4, 5, 6]])
print(M.shape)   # (2, 3)
print(M.ndim)    # 2
```

### Creating Arrays

```python
np.zeros((3, 4))          # 3x4 matrix of 0.0
np.ones((2, 2))           # 2x2 matrix of 1.0
np.eye(3)                 # 3x3 identity matrix
np.arange(0, 10, 2)       # [0, 2, 4, 6, 8]  (like range())
np.linspace(0, 1, 5)      # [0, 0.25, 0.5, 0.75, 1.0]  (evenly spaced)
np.random.randn(3, 3)     # 3x3 matrix of standard-normal random floats
```

### Indexing and Slicing

NumPy indexing mirrors Python list indexing but extends to multiple dimensions:

```python
M = np.array([[1, 2, 3],
              [4, 5, 6],
              [7, 8, 9]])

M[0]        # array([1, 2, 3])  — first row
M[:, 1]     # array([2, 5, 8])  — second column
M[0:2, 1:]  # array([[2, 3], [5, 6]])  — submatrix
M[1, 2]     # 6  — single element (row 1, col 2)
```

Slices are **views**, not copies. Modifying a slice modifies the original array. This is intentional — it avoids unnecessary memory allocation — but it surprises beginners.

```python
row = M[0]
row[0] = 999
print(M[0])  # array([999, 2, 3])  — original was changed!

# To get a real copy:
row = M[0].copy()
```

### Vectorized Operations

Any arithmetic operator applied to a NumPy array operates **elementwise**:

```python
a = np.array([1.0, 2.0, 3.0])
b = np.array([4.0, 5.0, 6.0])

a + b      # array([5., 7., 9.])
a * b      # array([4., 10., 18.])
a ** 2     # array([1., 4., 9.])
np.sqrt(a) # array([1., 1.414, 1.732])
```

No loops. Each of these dispatches to a single C function call.

### Matrix Operations

For actual matrix math (dot product, matrix multiplication), use `np.dot()` or the `@` operator:

```python
A = np.array([[1, 2], [3, 4]])
B = np.array([[5, 6], [7, 8]])

A @ B      # matrix multiplication: [[19, 22], [43, 50]]
A * B      # elementwise product: [[5, 12], [21, 32]]  ← NOT matrix mult!

v = np.array([1, 2])
A @ v      # matrix-vector product: [5, 11]

np.dot(v, v)  # dot product (scalar): 5
```

The `@` operator is the standard for matrix multiplication in ML code. Never confuse it with `*`.

### Broadcasting

Broadcasting is NumPy's rule for how to handle operations between arrays of different shapes. Instead of raising an error, NumPy stretches smaller arrays to match larger ones — **conceptually**, without actually copying data.

The rule: starting from the trailing (rightmost) dimension, shapes must either be equal, or one of them must be 1 (which gets "stretched"). If shapes don't match after this, you get an error.

```python
# Shape (3,) + scalar — scalar broadcasts to (3,)
a = np.array([1.0, 2.0, 3.0])
a + 10     # array([11., 12., 13.])

# Shape (3, 1) + (3,) — the (3,) broadcasts across rows
col = np.array([[1], [2], [3]])  # shape (3, 1)
row = np.array([10, 20, 30])     # shape (3,)
col + row
# array([[11, 21, 31],
#        [12, 22, 32],
#        [13, 23, 33]])
```

In ML, broadcasting is everywhere. Computing the loss for an entire batch of data at once without looping over examples — that's broadcasting. Subtracting the mean from each feature column — broadcasting. Adding a bias vector to every row of a weight matrix — broadcasting.

### Aggregation

```python
a = np.array([[1., 2., 3.],
              [4., 5., 6.]])

a.sum()          # 21.0   — total
a.sum(axis=0)    # array([5., 7., 9.])   — sum each column
a.sum(axis=1)    # array([6., 15.])      — sum each row
a.mean()         # 3.5
a.max()          # 6.0
a.argmax()       # 5   (index of max element in flattened array)
```

The `axis` parameter is critical in ML. When you average a batch of neural network outputs (shape `(batch_size, n_classes)`), you almost always want `axis=1` — average across classes per example.

### Reshaping

```python
a = np.arange(12)        # shape (12,)
a.reshape(3, 4)          # shape (3, 4)
a.reshape(2, 2, 3)       # shape (2, 2, 3)
a.reshape(-1, 4)         # shape (3, 4) — -1 means "infer this dimension"
a.flatten()              # always returns a 1D copy
a.ravel()                # returns 1D view if possible (cheaper)
```

In robotics, reshaping is constant. A batch of robot states might be shape `(batch_size, n_joints, 3)` — you'll reshape it for a neural net input as `(batch_size, n_joints * 3)`.

## Intuition

Think of a 2D NumPy array as a grid of numbers where every cell is the same size (all `float64`, or all `int32`). Because every element is the same size, the computer can jump directly to any element using simple arithmetic: `pointer + row * row_stride + col * col_stride`. There's no searching, no pointer-chasing — just arithmetic. That's why random access is O(1) and why bulk operations are so fast.

Broadcasting has a specific mental model: when you broadcast a shape `(1, 4)` array against a shape `(3, 4)` array, imagine the single row being "stamped" three times to fill the shape — but only in your head. NumPy actually reuses the memory, running the computation as if it were stamped without wasting the RAM.

## Key Formula / Rule

**Shape rule for matrix multiplication:** To multiply `A @ B`, the number of columns of `A` must equal the number of rows of `B`.

$$A_{m \times k} \cdot B_{k \times n} = C_{m \times n}$$

```python
A = np.random.randn(3, 4)   # (3, 4)
B = np.random.randn(4, 5)   # (4, 5)
C = A @ B                   # (3, 5)  ✓

# Common mistake:
A = np.random.randn(3, 4)
B = np.random.randn(3, 4)
A @ B                       # ValueError: shapes (3,4) and (3,4) not aligned
A @ B.T                     # (3, 3)  ✓ — B.T is (4, 3), so inner dims match
```

**Broadcasting check:** align shapes right-to-left; dimensions must be equal or one must be 1.

## Worked Example

**Problem:** Given a dataset of 5 robot joint angles (each a 3D vector), subtract the mean angle from each sample and compute the norm of each result — all without a single Python loop.

```python
import numpy as np

# 5 samples, 3 joint angles each — shape (5, 3)
joint_angles = np.array([
    [0.1, 0.2, 0.3],
    [0.4, 0.5, 0.6],
    [0.7, 0.8, 0.9],
    [1.0, 1.1, 1.2],
    [1.3, 1.4, 1.5],
])

# Step 1: compute mean across samples (axis=0 → shape (3,))
mean_angles = joint_angles.mean(axis=0)
print(mean_angles)  # [0.7, 0.8, 0.9]

# Step 2: subtract mean from every row — broadcasting (5,3) - (3,) → (5,3)
centered = joint_angles - mean_angles
print(centered)
# [[-0.6, -0.6, -0.6],
#  [-0.3, -0.3, -0.3],
#  [ 0. ,  0. ,  0. ],
#  [ 0.3,  0.3,  0.3],
#  [ 0.6,  0.6,  0.6]]

# Step 3: compute L2 norm of each row — shape (5,)
norms = np.linalg.norm(centered, axis=1)
print(norms)  # [1.039, 0.520, 0.000, 0.520, 1.039]

# Without NumPy, this would be 3 nested loops.
# With NumPy: 3 lines.
```

## Gotchas

**Gotcha 1 — `*` is elementwise, not matrix multiplication.** `A * B` multiplies element by element. `A @ B` is matrix multiplication. This is the single most common mistake when coming from MATLAB, which uses `*` for matrix multiplication by default.

**Gotcha 2 — 1D arrays are not column vectors.** A NumPy array of shape `(3,)` is neither a row vector nor a column vector — it's just 1D. `A @ v` with `v.shape = (3,)` treats `v` as a column vector in that context, but `v.T` has the same shape as `v` (transposing 1D does nothing). To get a true column vector, use `v.reshape(-1, 1)` for shape `(3, 1)`.

**Gotcha 3 — Slices are views, not copies.** Modifying a slice modifies the original. Use `.copy()` when you need independence.

**Gotcha 4 — Integer division in `dtype`.** `np.array([1, 2, 3]) / 2` gives `[0.5, 1.0, 1.5]` (float), but `np.array([1, 2, 3]) // 2` gives `[0, 1, 1]` (integer floor division). Accidentally creating integer-dtype arrays when you need floats causes silent wrong results.

**Gotcha 5 — Broadcasting mismatch is a runtime error, not a compile error.** Shape mismatches only blow up when the operation runs, not when you write the code. Always print `.shape` early and often when debugging.

**Gotcha 6 — `np.dot(A, B)` vs `A @ B`.** For 2D arrays they're equivalent. For 1D arrays, `np.dot(a, b)` computes the scalar dot product. Prefer `@` for matrices to make intent clear.

## See Also

- [[Vectors and Dot Products]] — the math behind `np.dot` and `np.linalg.norm`; NumPy is just that math in fast code
- [[Matrix Multiplication]] — the operation `A @ B` is formally defined here; understand the math before trusting NumPy's output
- [[Gradient Descent]] — gradient descent in ML is entirely NumPy (or PyTorch) array arithmetic; you'll use everything in this entry daily
- [[Neural Networks - The Basics]] — neural network forward and backward passes are matrix multiplications; NumPy is the substrate
- [[Python Data Structures]] — contrast with Python lists to understand *why* NumPy arrays exist
