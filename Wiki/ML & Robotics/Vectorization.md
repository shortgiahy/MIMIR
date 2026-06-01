# Vectorization

**One-liner:** Replacing explicit Python `for` loops with single array operations that execute in compiled C/Fortran — typically 10–100× faster because the loop runs outside the Python interpreter.

## Core Idea
$$\text{Python loop:} \quad \text{interpret each iteration} \quad O(n) \text{ interpreter calls}$$
$$\text{Vectorized:} \quad \text{one call to C, loop in compiled code} \quad O(1) \text{ interpreter calls}$$
Vectorization is not a new algorithm — it's the same computation, just moved from the Python interpreter into a precompiled binary. The speedup comes from eliminating per-iteration interpreter overhead and enabling SIMD (Single Instruction Multiple Data) CPU instructions that process multiple elements in one clock cycle.

## Why It Exists
Python is ~100× slower than C for numerical loops. Every iteration of a Python `for` loop incurs: bytecode dispatch, type checking, object reference counting, and memory indirection for each element. For ML workloads with millions of operations, this is unacceptable. NumPy's solution: expose a Python API that immediately delegates to C/Fortran loops, bypassing the interpreter for the inner loop.

## Real-World Applications
- **Training a neural network:** without vectorization, multiplying a 784-element input by a 512-row weight matrix would require 784 × 512 = 401,408 Python iterations. With `W @ x`, it's one C call.
- **Processing LIDAR data for Baymax:** 1000 distance readings, apply trigonometric transforms → vectorized `np.cos(angles)` instead of a Python loop.
- **Data preprocessing:** normalize a dataset of 60,000 images: `X = (X - mean) / std` — three vectorized operations vs 60,000 iterations.
- **Batch gradient computation:** gradient over a mini-batch computed as a single matrix multiplication.

## Intuition
Imagine you're managing a warehouse. The Python loop approach: you personally walk to each shelf, pick up one item, process it, put it back — 1000 trips. The vectorized approach: you hand a worker a forklift and a list; they handle all 1000 items while you wait at the desk. The work is the same; the overhead is eliminated.

More precisely: SIMD instructions on modern CPUs can add 4–8 float64 values simultaneously in one clock cycle. A Python loop can never exploit this because each iteration is a separate interpreter action. NumPy's BLAS routines are hand-tuned to use AVX/SSE instructions — they exploit hardware parallelism that's invisible to Python.

## Derivation
**Operation count comparison** (add two 1M-element arrays):

Python loop: 1,000,000 × (bytecode fetch + type check + refcount update + heap access) ≈ 1M × ~100ns = ~100ms

NumPy vectorized: 1 Python call overhead + 1M × (contiguous memory read + SIMD add) ≈ 1ms + ~1ms = ~2ms

Speedup: ~50×. (Actual numbers vary with CPU cache effects.)

**When NOT to vectorize:** Very complex per-element logic with branching or data-dependent shapes. NumPy can't always vectorize these — use Numba or Cython then.

## Worked Example
```python
import numpy as np
import time

N = 1_000_000

# ── 1. Sum of squares ────────────────────────────────────
data = np.random.rand(N)

# Python loop
start = time.perf_counter()
result = 0.0
for x in data:
    result += x * x
t_loop = time.perf_counter() - start

# Vectorized
start = time.perf_counter()
result_vec = np.dot(data, data)   # or np.sum(data**2)
t_vec = time.perf_counter() - start

print(f"Loop:        {t_loop*1000:.1f} ms")    # ~400 ms
print(f"Vectorized:  {t_vec*1000:.2f} ms")     # ~2 ms
print(f"Speedup:     {t_loop/t_vec:.0f}×")     # ~200×

# ── 2. Common vectorized patterns ────────────────────────
x = np.array([1.0, 4.0, 9.0, 16.0])

# Element-wise math (no loop needed)
print(np.sqrt(x))          # [1. 2. 3. 4.]
print(np.exp(-x))          # element-wise exp
print(np.log(x))           # element-wise log

# Logical operations (replace if/else loops)
mask = x > 5
print(x[mask])             # [9. 16.] — boolean indexing

# ── 3. Normalizing a dataset (ML preprocessing) ──────────
dataset = np.random.randn(10000, 784)  # 10k MNIST-like images
mean = dataset.mean(axis=0)            # shape (784,)
std  = dataset.std(axis=0)             # shape (784,)
normalized = (dataset - mean) / std   # shape (10000, 784) — broadcasts!
# vs: for i in range(10000): normalized[i] = (dataset[i] - mean) / std

# ── 4. Vectorized sigmoid activation ─────────────────────
def sigmoid_loop(z_array):
    return [1 / (1 + np.exp(-z)) for z in z_array]  # Python loop!

def sigmoid_vec(z_array):
    return 1 / (1 + np.exp(-z_array))  # vectorized — 50× faster

z = np.linspace(-5, 5, N)
# Try timing both — vectorized wins decisively
```

## See Also
- [[NumPy Array]] — the data structure that makes vectorization possible
- [[Broadcasting]] — how NumPy vectorizes operations between different-shaped arrays
- [[Matrix Multiplication]] — the ultimate vectorized operation: entire layer in one call
- [[Forward Pass]] — a fully vectorized computation graph
- [[Loss Function]] — computed vectorized over entire batch
