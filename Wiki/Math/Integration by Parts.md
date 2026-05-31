# Integration by Parts

**One-liner:** A technique for integrating products of functions, derived directly by reversing the product rule for derivatives.

## Why It Exists

When you encounter an integral like $\int x e^x \, dx$ or $\int x \ln x \, dx$, no basic substitution works — the integrand is a *product* of two fundamentally different types of functions. Integration by parts exists because differentiation has the product rule, and integration is differentiation's inverse. If you can run a differentiation rule backwards, you get an integration rule. That's the entire game.

In Electrical Engineering, this shows up constantly: computing energy stored in inductors ($E = \int_0^t v(t) i(t) \, dt$), solving differential equations for RLC circuits, and working with Laplace transforms. In physics, it's how you derive work-energy theorems and handle momentum integrals. Learning it once means you can apply it across every quantitative course you take.

## The Concept

Start with the product rule. If $u$ and $v$ are both functions of $x$:

$$\frac{d}{dx}[u \cdot v] = u \frac{dv}{dx} + v \frac{du}{dx}$$

Now integrate both sides with respect to $x$:

$$\int \frac{d}{dx}[u \cdot v] \, dx = \int u \frac{dv}{dx} \, dx + \int v \frac{du}{dx} \, dx$$

The left side simplifies by the Fundamental Theorem of Calculus — integrating a derivative gives back the function:

$$uv = \int u \, dv + \int v \, du$$

Rearrange to isolate one integral:

$$\int u \, dv = uv - \int v \, du$$

That's the formula. You've transformed one integral ($\int u \, dv$) into another ($\int v \, du$). The entire point is that the *new* integral on the right should be easier than what you started with. If it isn't, you chose $u$ and $dv$ wrong.

### The LIATE Heuristic — and Why It Works

LIATE is a priority list for choosing which factor in the product to call $u$:

| Letter | Category | Examples |
|--------|----------|---------|
| **L** | Logarithms | $\ln x$, $\log x$ |
| **I** | Inverse trig | $\arctan x$, $\arcsin x$ |
| **A** | Algebraic (polynomials) | $x^2$, $x^3 - 1$ |
| **T** | Trigonometric | $\sin x$, $\cos x$ |
| **E** | Exponential | $e^x$, $2^x$ |

Choose $u$ from whichever category appears *higher* in the list. The other factor becomes $dv$.

**Why does this work?** The goal is to make $\int v \, du$ simpler than $\int u \, dv$. Think about what happens when you differentiate each type:

- Logarithms and inverse trig functions become algebraic (simpler) when differentiated. That's why they're at the top — differentiating them reduces their complexity.
- Polynomials get *simpler* with each differentiation (eventually reaching a constant and then zero). They belong in the middle.
- Exponentials and trig functions loop back on themselves when differentiated — they don't simplify, but they don't complicate things either. They're flexible as $dv$ because integrating them is just as easy as differentiating.

So LIATE is really a ranking of "which function simplifies most when differentiated?" Higher = simpler after differentiating = better choice for $u$.

### When LIATE Isn't Enough: Repeated Application

Some integrals require applying integration by parts multiple times. For instance, $\int x^2 e^x \, dx$ requires applying it twice — each time, the polynomial factor loses a degree until it's gone. A tabular method (also called the "DI method") organizes this: make two columns, one for successive derivatives of $u$ and one for successive integrals of $dv$, then multiply diagonally with alternating signs.

### The Circular Case

For integrals like $\int e^x \sin x \, dx$, applying integration by parts twice returns you to the *original* integral. This isn't failure — it's a solvable algebraic equation. Call the original integral $I$, apply integration by parts twice, and you'll get $I = (\text{something}) - I$, which gives $2I = (\text{something})$, so $I = \frac{\text{something}}{2}$.

## Intuition

Think of it this way: you're trading complexity. You have a hard integral. Integration by parts lets you "transfer" a derivative from one factor to another — at the cost of evaluating a boundary term $uv$. If differentiating $u$ makes it simpler and integrating $dv$ keeps things manageable, the trade is worth it.

A physical analogy: in mechanics, when you integrate $\int F \cdot x \, dt$ (force times position, over time), you sometimes want to move the $x$ "onto" something else so the integral becomes easier. Integration by parts is the tool that lets you do that transfer rigorously.

## Key Formula / Rule

$$\boxed{\int u \, dv = uv - \int v \, du}$$

Equivalently written with explicit functions $f$ and $g$:

$$\int f(x) g'(x) \, dx = f(x) g(x) - \int f'(x) g(x) \, dx$$

**LIATE:** Choose $u$ from: **L**ogarithms → **I**nverse trig → **A**lgebraic → **T**rig → **E**xponential

## Worked Example

**Problem:** Evaluate $\int x e^x \, dx$.

**Step 1 — Identify the product and apply LIATE.**
We have the product $x \cdot e^x$. $x$ is algebraic (A), $e^x$ is exponential (E). Algebraic ranks higher in LIATE, so:
$$u = x, \quad dv = e^x \, dx$$

**Step 2 — Compute $du$ and $v$.**

Differentiate $u$: $du = dx$

Integrate $dv$: $v = \int e^x \, dx = e^x$ (no constant needed yet)

**Step 3 — Substitute into the formula.**

$$\int x e^x \, dx = \underbrace{x}_{u} \cdot \underbrace{e^x}_{v} - \int \underbrace{e^x}_{v} \, \underbrace{dx}_{du}$$

$$= x e^x - \int e^x \, dx$$

**Step 4 — Evaluate the remaining integral.**

$\int e^x \, dx = e^x$, so:

$$= x e^x - e^x + C$$

**Step 5 — Factor (optional but clean).**

$$= e^x(x - 1) + C$$

**Verification:** Differentiate the answer. Using the product rule on $e^x(x-1)$:
$$\frac{d}{dx}[e^x(x-1)] = e^x(x-1) + e^x(1) = e^x(x - 1 + 1) = x e^x \checkmark$$

## Gotchas

**Gotcha 1 — Choosing $u$ and $dv$ backwards.** If you pick $u = e^x$ and $dv = x \, dx$ for the example above, you get $v = x^2/2$ and $du = e^x \, dx$, which gives you $\int \frac{x^2}{2} e^x \, dx$ — harder, not easier. Always check that the new integral is simpler before proceeding.

**Gotcha 2 — Forgetting the $-$ sign.** The formula is $uv \mathbf{-} \int v \, du$. Losing that negative is the single most common point-killer on exams.

**Gotcha 3 — Dropping the constant of integration.** For indefinite integrals, you must write $+ C$ at the end. It's not optional — it represents an entire family of functions.

**Gotcha 4 — Changing your $u$ choice mid-problem.** When applying integration by parts multiple times, your $u$ choice must be consistent with the same "side" each time (same function type), or you'll undo your own work and land back at zero.

**Gotcha 5 — Integration by parts on a single function.** You can integrate $\int \ln x \, dx$ by writing it as $\int \ln x \cdot 1 \, dx$, setting $u = \ln x$ and $dv = dx$. Students often don't recognize this trick because it looks like there's only one function.

## See Also

- [[Trigonometric Substitution]] — another integral technique for radical expressions; sometimes used in combination with integration by parts
- [[Taylor and Maclaurin Series]] — the coefficients involve repeated differentiation, and integration by parts governs the error term derivation
- [[Sequences and Series]] — integration by parts appears in deriving convergence of improper integrals related to series
- [[Convergence Tests]] — the integral test requires evaluating improper integrals, often via integration by parts
