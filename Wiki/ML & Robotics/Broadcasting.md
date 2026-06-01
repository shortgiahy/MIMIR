# Broadcasting

**One-liner:** NumPy's rule system for performing arithmetic between arrays of different shapes by implicitly expanding smaller arrays along size-1 dimensions — without copying data.

## Core Idea
$$\text{If shapes are compatible: align right, expand size-1 dims} \Rightarrow \text{result shape}$$
Two arrays are broadcast-compatible if, for each dimension (aligned from the right), the sizes are either equal or one of them is 1. The array with size 1 is "stretched" to match the other — conceptually. Physically, no copy is made; strides trick the CPU into reading the same memory repeatedly.

## Why It Exists
Without broadcasting, you'd need explicit loops or `np.tile()` to make shapes match before arithmetic. Want to subtract the mean from each of 1000 data points? You'd loop or repeat the mean 1000 times. Broadcasting makes `data - mean` work directly — more readable, zero-copy, and vectorized. It's syntactic sugar over the strides mechanism in [[NumPy Array]].

## Real-World Applications
- **Dataset normalization:** `(X - X.mean(axis=0)) / X.std(axis=0)` where X is `(N, features)` and mean/std are `(features,)` — broadcast across N rows automatically.
- **Neural network biases:** add bias vector `b` of shape `(512,)` to a batch output of shape `(32, 512)` — broadcasting adds b to each of the 32 rows.
- **Distance computation:** compute pairwise distances between two sets of points without loops.
- **Masking and gating:** multiply activations by a binary mask `(N, C, H, W) * mask(1, C, 1, 1)` in convolutional nets.

## Intuition
Think of broadcasting as "repeating without actually repeating." A (3,) vector added to a (4, 3) matrix: the vector gets logically stacked 4 times to become (4, 3), then added element-wise. The strides implementation does this by setting the stride of the "expanded" dimension to 0 — so reading "the next row" of the virtual repeated array just re-reads the same memory.

The right-align rule: shapes `(4, 3)` and `(3,)` — right-align as `(4, 3)` vs `(1, 3)` — compatible. Shapes `(4, 3)` and `(4,)` — right-align as `(4, 3)` vs `(4, 1)` after numpy auto-adds a dimension — wait, NumPy auto-prepends, not appends. `(4,)` becomes `(1, 4)`, which is `(1, 4)` vs `(4, 3)` — dimension 1 is 1 (OK, expands to 4) but dimension 2 is 4 vs 3 — incompatible. This is a common bug!

## Derivation
**Broadcasting rules (NumPy specification):**
1. If arrays have different ndim, prepend 1s to the smaller shape.
2. Dimensions are compatible if: they're equal, OR one of them is 1.
3. Output shape: take max along each dimension.
4. Error if neither condition holds for any dimension.

**Example trace:**
```
A: (8, 1, 6, 1)
B:    (7, 1, 5)

Step 1 — prepend 1s to B: (1, 7, 1, 5)
Step 2 — check compatibility:
  dim0: 8 vs 1  ✓ (expand B's)   → 8
  dim1: 1 vs 7  ✓ (expand A's)   → 7
  dim2: 6 vs 1  ✓ (expand B's)   → 6
  dim3: 1 vs 5  ✓ (expand A's)   → 5
Result: (8, 7, 6, 5)
```

## Worked Example
```python
import numpy as np

# ── 1. Subtract mean from dataset (most common ML use) ───
X = np.random.randn(100, 4)  # 100 samples, 4 features
mean = X.mean(axis=0)        # shape (4,) — mean per feature
print(mean.shape)             # (4,)
X_centered = X - mean         # broadcasting: (100,4) - (4,) → (100,4)
print(X_centered.mean(axis=0))  # ≈ [0,0,0,0]

# ── 2. Bias addition in neural network ───────────────────
batch_size = 32
hidden_size = 512
activations = np.random.randn(batch_size, hidden_size)  # (32, 512)
bias = np.zeros(hidden_size)                             # (512,)
output = activations + bias  # (32, 512) + (512,) → (32, 512)
# bias broadcast across all 32 examples

# ── 3. Outer product via broadcasting ────────────────────
a = np.array([1, 2, 3])    # shape (3,)
b = np.array([10, 20, 30]) # shape (3,)
# Reshape to column and row vectors to broadcast
outer = a[:, np.newaxis] * b[np.newaxis, :]
# (3,1) * (1,3) → (3,3)
print(outer)
# [[ 10  20  30]
#  [ 20  40  60]
#  [ 30  60  90]]

# ── 4. Common shape error ─────────────────────────────────
A = np.ones((4, 3))
B = np.ones((4,))   # shape (4,) — you might expect this works
try:
    result = A + B  # ERROR: (4,3) + (1,4) — dim mismatch
except ValueError as e:
    print(f"Shape error: {e}")

# Fix: reshape B explicitly
B_col = B[:, np.newaxis]  # shape (4, 1)
result = A + B_col         # (4,3) + (4,1) → (4,3) ✓

# ── 5. Verify no memory copy ─────────────────────────────
import sys
a_big = np.ones((1000, 1000))  # 8 MB
b_row = np.ones(1000)           # 8 KB
c = a_big + b_row               # Result is 8 MB — b_row was NOT copied to 8 MB first
print(sys.getsizeof(b_row))     # 8096 bytes — unchanged
```

## See Also
- [[NumPy Array]] — strides are the mechanism behind broadcasting
- [[Vectorization]] — broadcasting enables vectorized ops between unequal shapes
- [[Matrix Multiplication]] — `@` is NOT broadcast arithmetic; it's structural composition
- [[Neural Network]] — bias terms rely on broadcasting in every layer
- [[Forward Pass]] — batch processing relies on broadcasting for bias addition
