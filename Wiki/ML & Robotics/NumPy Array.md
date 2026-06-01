# NumPy Array

**One-liner:** NumPy's `ndarray` is a fixed-type, C-contiguous block of memory that makes n-dimensional math 10–100× faster than Python lists by eliminating interpreter overhead.

## Core Idea
$$\text{ndarray} = (\text{data pointer},\ \text{dtype},\ \text{shape},\ \text{strides})$$
An ndarray is not just a list — it's a descriptor for a block of raw memory. `shape` tells you dimensions (e.g., `(3, 4)` = 3 rows × 4 cols). `dtype` tells you element type (`float64` = 8 bytes each). `strides` tells you how many bytes to jump to move one step along each axis. This metadata separation is what makes slicing free (O(1)) and [[Vectorization]] possible.

## Why It Exists
Python lists are flexible but slow: each element is a Python object with overhead (type tag, reference count), stored as pointers scattered in heap memory — terrible for CPU cache. NumPy stores all elements *contiguously* in one block of a single fixed type. The CPU prefetcher loves this. BLAS/LAPACK routines (written in Fortran in the 1970s, still unbeaten) can operate directly on this memory. The result: a Python loop over 1M floats takes ~1 second; a NumPy vector operation takes ~1 millisecond.

## Real-World Applications
- **PyTorch and TensorFlow tensors** are conceptually the same idea (GPU-accelerated ndarrays) — understanding ndarray is understanding both.
- **Sensor data processing for Baymax:** 1000 LIDAR distance readings stored as `float32` ndarray, processed in one vectorized operation instead of a Python loop.
- **NumPy is the common language:** `pandas`, `scikit-learn`, `scipy`, `OpenCV` — all exchange data as ndarrays.
- **Training data batches:** a batch of 32 RGB images is a `(32, 3, 224, 224)` float32 ndarray — 32 × 3 × 224 × 224 × 4 bytes ≈ 19 MB in one contiguous block.

## Intuition
Think of an ndarray as a view into a flat buffer. The `(3, 4)` shape and `(32, 8)` strides on a float64 array mean: to move to the next row, jump 32 bytes; to move to the next column, jump 8 bytes (= 1 float64). A transpose `A.T` doesn't copy memory — it just swaps strides: `(8, 32)`. This is O(1), not O(n). The data is the same; the *interpretation* changed.

## Derivation
**Memory layout for a (3, 4) C-contiguous float64 array:**
```
Element [i, j] is at byte offset: i * (4 * 8) + j * 8
                                 = i * 32    + j * 8
Strides = (32, 8)
```
This is "row-major" or "C-contiguous" (rows stored consecutively).

**Why C-contiguity matters for performance:**
When computing `A @ B` (matrix multiply), the CPU reads rows of A sequentially — perfect for C-contiguous A. For B, it reads columns — each step skips n elements in memory. This is why transposing B before multiplication (if reusing it) can improve cache behavior.

**Views vs copies:**
```python
a = np.arange(12).reshape(3, 4)
b = a[1:, ::2]   # slice — creates a VIEW, not a copy
b[0, 0] = 999    # modifies a!
```
Slicing creates new metadata (shape, strides, pointer offset) pointing to the same buffer. `np.ascontiguousarray()` forces a copy into contiguous layout.

## Worked Example
```python
import numpy as np

# Create arrays — note dtypes
a = np.array([1.0, 2.0, 3.0])           # float64 by default
b = np.array([1, 2, 3], dtype=np.int32) # explicit int32
c = np.zeros((3, 4), dtype=np.float32)  # 32-bit for GPU compatibility

# Inspect internals
x = np.array([[1.0, 2.0, 3.0],
              [4.0, 5.0, 6.0]])
print(x.shape)    # (2, 3)
print(x.dtype)    # float64
print(x.ndim)     # 2
print(x.strides)  # (24, 8) — 3 float64s per row = 24 bytes
print(x.nbytes)   # 48 bytes total

# Reshape: change shape, same data
flat = np.arange(12, dtype=float)
matrix = flat.reshape(3, 4)   # view (no copy)
cube   = flat.reshape(2, 2, 3) # 3D

# Transpose: free (swaps strides)
print(matrix.T.shape)    # (4, 3)
print(matrix.T.strides)  # (8, 32) — reversed

# Slicing creates views — careful!
row = matrix[0]        # view of first row
row[0] = 999.0
print(matrix[0, 0])    # 999.0 — matrix was modified!

# Force a copy when needed
row_copy = matrix[1].copy()
row_copy[0] = 0.0
print(matrix[1, 0])    # unchanged

# Timing: Python loop vs NumPy
import time
N = 1_000_000
data = np.random.rand(N)

start = time.time()
total = sum(x**2 for x in data)  # Python loop
print(f"Python loop: {time.time() - start:.3f}s")   # ~0.5s

start = time.time()
total = np.sum(data**2)           # NumPy vectorized
print(f"NumPy:       {time.time() - start:.3f}s")   # ~0.003s
```

## See Also
- [[Vector]] — a 1D ndarray with `ndim=1`
- [[Matrix]] — a 2D ndarray with `ndim=2`
- [[Vectorization]] — why array operations are faster than loops
- [[Broadcasting]] — how NumPy handles operations on different shapes
- [[Matrix Multiplication]] — `@` operator on ndarrays
- [[Encapsulation]] — the ndarray encapsulates raw memory, shape, strides, and dtype behind a clean API; callers never touch the underlying buffer directly
- [[Instance]] — every ndarray is an instance of the np.ndarray class; understanding instances explains why slices share memory with the original array instance
