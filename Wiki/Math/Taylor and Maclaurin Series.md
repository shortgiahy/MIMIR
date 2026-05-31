# Taylor and Maclaurin Series

**One-liner:** A way to represent any sufficiently smooth function as an infinite polynomial, built by matching the function's derivatives at a single point — making transcendental functions computable, approximable, and analyzable.

## Why It Exists

You cannot compute $\sin(0.1)$ with basic arithmetic. You cannot solve differential equations involving $e^x$ in closed form when $e^x$ is embedded nonlinearly. Computers don't have a native "sine" circuit — they approximate it using polynomials.

Taylor series exist because **polynomials are easy and everything else is hard**. Addition, multiplication, differentiation, and integration are all trivially easy for polynomials. If you can rewrite $\sin x$, $e^x$, or $\ln(1+x)$ as an infinite polynomial that converges to the true function, you've turned a hard problem into a sequence of easy ones.

This is not a purely mathematical curiosity. In control theory and robotics, you linearize nonlinear systems using first-order Taylor approximations — that's how you design a controller for a robot arm. In circuit analysis, small-signal models of transistors come from Taylor expanding the $I$-$V$ curve around the operating point. In signal processing, the Fourier series (a cousin of Taylor series) breaks signals into sinusoids, and the convergence theory comes from the same ideas. Every numerical algorithm — ODE solvers, root finders, optimization methods — uses Taylor series under the hood.

## The Concept

### The Core Question: Can a Function Equal an Infinite Polynomial?

Ask: if a function $f(x)$ could be written as a power series, what would its coefficients have to be?

Suppose:
$$f(x) = c_0 + c_1 x + c_2 x^2 + c_3 x^3 + c_4 x^4 + \cdots$$

Plug in $x = 0$:
$$f(0) = c_0$$

So $c_0 = f(0)$. Now differentiate both sides:
$$f'(x) = c_1 + 2c_2 x + 3c_3 x^2 + 4c_4 x^3 + \cdots$$

Plug in $x = 0$:
$$f'(0) = c_1$$

Differentiate again:
$$f''(x) = 2c_2 + 6c_3 x + 12c_4 x^2 + \cdots$$

Plug in $x = 0$:
$$f''(0) = 2c_2 \implies c_2 = \frac{f''(0)}{2}$$

Differentiate once more:
$$f'''(x) = 6c_3 + 24c_4 x + \cdots$$

$$f'''(0) = 6c_3 \implies c_3 = \frac{f'''(0)}{6} = \frac{f'''(0)}{3!}$$

The pattern: after differentiating $n$ times and plugging in $x = 0$, only the $c_n$ term survives, and its coefficient is $n!$. This forces:

$$c_n = \frac{f^{(n)}(0)}{n!}$$

where $f^{(n)}(0)$ means the $n$th derivative of $f$ evaluated at $0$.

Substituting back:

$$f(x) = \sum_{n=0}^{\infty} \frac{f^{(n)}(0)}{n!} x^n = f(0) + f'(0)x + \frac{f''(0)}{2!}x^2 + \frac{f'''(0)}{3!}x^3 + \cdots$$

This is the **Maclaurin series** — the Taylor series centered at $x = 0$.

### Centering at a Different Point: The General Taylor Series

The above centered the expansion at $x = 0$. But we can expand around any point $x = a$ by the same argument — just evaluate all derivatives at $x = a$ instead:

$$f(x) = \sum_{n=0}^{\infty} \frac{f^{(n)}(a)}{n!} (x - a)^n$$

This is the **Taylor series** of $f$ centered at $a$. The Maclaurin series is the special case $a = 0$.

Why center at a point other than zero? Because you want the approximation to be accurate near $a$, not near zero. If you're computing $\ln(3.001)$, you'd expand $\ln x$ around $a = 3$ — the series converges quickly for inputs near $3$, while expanding around $0$ would require many terms (and $\ln 0$ is undefined anyway).

### Partial Sums: Taylor Polynomials

The $n$th-degree **Taylor polynomial** $T_n(x)$ is the partial sum through the $x^n$ term:

$$T_n(x) = \sum_{k=0}^{n} \frac{f^{(k)}(a)}{k!}(x-a)^k$$

This is the best polynomial approximation of degree $\leq n$ to $f$ near $x = a$. "Best" means it matches $f$ and all its first $n$ derivatives exactly at the point $a$:

$$T_n^{(k)}(a) = f^{(k)}(a) \quad \text{for } k = 0, 1, 2, \ldots, n$$

The **Taylor remainder** $R_n(x) = f(x) - T_n(x)$ quantifies the approximation error. By **Taylor's Remainder Theorem**:

$$R_n(x) = \frac{f^{(n+1)}(c)}{(n+1)!}(x-a)^{n+1}$$

for some $c$ between $a$ and $x$. This gives a computable bound on the error — crucial for numerical computation and for proving that a series actually converges to the function (not just to some other function).

### Radius of Convergence

A Taylor series doesn't necessarily converge for all $x$. The set of $x$ values where it converges is an interval centered at $a$, called the **interval of convergence**, with radius $R$ called the **radius of convergence**.

$$R = \frac{1}{\limsup_{n \to \infty} \sqrt[n]{|c_n|}}$$

In practice, the **ratio test** determines $R$: apply it to the series coefficients, solve for the values of $x$ that make $\rho < 1$, and that's your interval of convergence. Then check the endpoints separately (they can converge or diverge independently).

Examples:
- $e^x$ converges for all $x$ ($R = \infty$)
- $\sum x^n = \frac{1}{1-x}$ converges only for $|x| < 1$ ($R = 1$)
- $\ln(1+x)$ converges for $-1 < x \leq 1$ ($R = 1$, with the right endpoint included)

### The Essential Maclaurin Series

These must be memorized — they appear everywhere and can be manipulated algebraically to produce new series.

**Exponential:**
$$e^x = \sum_{n=0}^{\infty} \frac{x^n}{n!} = 1 + x + \frac{x^2}{2!} + \frac{x^3}{3!} + \cdots, \quad R = \infty$$

Derivation: every derivative of $e^x$ is $e^x$, and $e^0 = 1$. So all coefficients are $\frac{1}{n!}$.

**Sine:**
$$\sin x = \sum_{n=0}^{\infty} \frac{(-1)^n x^{2n+1}}{(2n+1)!} = x - \frac{x^3}{3!} + \frac{x^5}{5!} - \cdots, \quad R = \infty$$

Derivation: $\sin^{(n)}(0)$ cycles through $0, 1, 0, -1, 0, 1, \ldots$ — only odd-indexed terms survive.

**Cosine:**
$$\cos x = \sum_{n=0}^{\infty} \frac{(-1)^n x^{2n}}{(2n)!} = 1 - \frac{x^2}{2!} + \frac{x^4}{4!} - \cdots, \quad R = \infty$$

Note: the cosine series is the derivative of the sine series, term by term. This is always valid within the radius of convergence.

**Geometric series:**
$$\frac{1}{1-x} = \sum_{n=0}^{\infty} x^n = 1 + x + x^2 + x^3 + \cdots, \quad |x| < 1$$

This is the Maclaurin series for $\frac{1}{1-x}$, consistent with the geometric series formula.

**Natural logarithm:**
$$\ln(1+x) = \sum_{n=1}^{\infty} \frac{(-1)^{n+1} x^n}{n} = x - \frac{x^2}{2} + \frac{x^3}{3} - \cdots, \quad -1 < x \leq 1$$

Derivation: Start from $\frac{1}{1+x} = \sum (-x)^n = \sum (-1)^n x^n$ (geometric series). Integrate term by term from $0$ to $x$: the integral of $\frac{1}{1+t}$ is $\ln(1+x)$, and integrating each $(-1)^n t^n$ gives $\frac{(-1)^n x^{n+1}}{n+1}$. Shift the index and you get the formula above.

**Arctangent:**
$$\arctan x = \sum_{n=0}^{\infty} \frac{(-1)^n x^{2n+1}}{2n+1} = x - \frac{x^3}{3} + \frac{x^5}{5} - \cdots, \quad |x| \leq 1$$

Derivation: Start from $\frac{1}{1+x^2} = \sum (-1)^n x^{2n}$ (geometric series with $x^2$). Integrate from $0$ to $x$: the integral of $\frac{1}{1+t^2}$ is $\arctan x$, and integrating each term gives the series above. At $x = 1$: $\arctan(1) = \pi/4 = 1 - \frac{1}{3} + \frac{1}{5} - \frac{1}{7} + \cdots$ (Leibniz formula for $\pi$).

### Manipulating Series Algebraically

One of the most powerful skills: derive new series from known ones without computing any derivatives.

**Substitution:** Replace $x$ with a function of $x$.
$$e^{-x^2} = \sum_{n=0}^{\infty} \frac{(-x^2)^n}{n!} = \sum_{n=0}^{\infty} \frac{(-1)^n x^{2n}}{n!}$$

This series is crucial — it's the Gaussian function and has no closed-form antiderivative. Its Taylor series is how numerical integration is done.

**Multiplication:** Multiply known series together (Cauchy product).
$$e^x \sin x = \left(1 + x + \frac{x^2}{2} + \frac{x^3}{6} + \cdots\right)\left(x - \frac{x^3}{6} + \cdots\right)$$

Multiply termwise and collect powers of $x$: $= x + x^2 + \frac{x^3}{3} - \frac{x^3}{6} + \cdots = x + x^2 + \frac{x^3}{6} + \cdots$

**Differentiation and integration:** Differentiate or integrate series term by term (valid within the radius of convergence). This is how the $\ln(1+x)$ and $\arctan x$ series are derived from the geometric series.

### Euler's Formula — The Payoff

Using the Maclaurin series for $e^x$ with $x = i\theta$ (imaginary):

$$e^{i\theta} = \sum_{n=0}^{\infty} \frac{(i\theta)^n}{n!}$$

Separate real (even powers) and imaginary (odd powers) parts, using $i^2 = -1$:

$$e^{i\theta} = \underbrace{\left(1 - \frac{\theta^2}{2!} + \frac{\theta^4}{4!} - \cdots\right)}_{\cos\theta} + i\underbrace{\left(\theta - \frac{\theta^3}{3!} + \frac{\theta^5}{5!} - \cdots\right)}_{\sin\theta}$$

$$\boxed{e^{i\theta} = \cos\theta + i\sin\theta}$$

This is **Euler's formula**. At $\theta = \pi$: $e^{i\pi} + 1 = 0$ — Euler's identity, connecting five fundamental constants. More practically: Euler's formula is the foundation of phasor analysis in AC circuit theory, Fourier transforms, and quantum mechanics. Understanding where it comes from (Taylor series!) makes it less magical and more useful.

## Intuition

Think of a Taylor polynomial as the "shape-matching" game. A constant $T_0$ matches the function's height at $a$. Add a linear term: $T_1$ also matches the slope. Add a quadratic: $T_2$ also matches the curvature. Each added term gives the polynomial one more degree of freedom to match another derivative — another aspect of the function's shape.

As you add more terms, the polynomial "hugs" the function more and more tightly, in a wider and wider neighborhood around $a$. The full infinite series is the "limit" of this hugging process — and for well-behaved functions, it eventually reproduces the function exactly.

Geometrically: zoom in far enough on any smooth curve near a point, and it looks like a line (this is the first-order Taylor approximation). Zoom in on the difference between the curve and that line, and it looks like a parabola (second-order). Taylor series formalizes this zoom-in hierarchy to all orders.

## Key Formula / Rule

**Maclaurin Series** (centered at $a = 0$):
$$f(x) = \sum_{n=0}^{\infty} \frac{f^{(n)}(0)}{n!} x^n$$

**General Taylor Series** (centered at $a$):
$$f(x) = \sum_{n=0}^{\infty} \frac{f^{(n)}(a)}{n!} (x - a)^n$$

**Essential Series to Memorize:**

$$e^x = \sum_{n=0}^{\infty} \frac{x^n}{n!}, \quad R = \infty$$

$$\sin x = \sum_{n=0}^{\infty} \frac{(-1)^n x^{2n+1}}{(2n+1)!}, \quad R = \infty$$

$$\cos x = \sum_{n=0}^{\infty} \frac{(-1)^n x^{2n}}{(2n)!}, \quad R = \infty$$

$$\frac{1}{1-x} = \sum_{n=0}^{\infty} x^n, \quad |x| < 1$$

$$\ln(1+x) = \sum_{n=1}^{\infty} \frac{(-1)^{n+1} x^n}{n}, \quad -1 < x \leq 1$$

## Worked Example

**Problem:** Find the Maclaurin series for $f(x) = e^{-x^2}$ and use it to approximate $\displaystyle\int_0^{0.5} e^{-x^2} \, dx$ to within $0.001$.

**Step 1 — Use the known series for $e^x$.**

Start from $e^x = \sum_{n=0}^{\infty} \frac{x^n}{n!}$.

Substitute $x \to -x^2$:

$$e^{-x^2} = \sum_{n=0}^{\infty} \frac{(-x^2)^n}{n!} = \sum_{n=0}^{\infty} \frac{(-1)^n x^{2n}}{n!}$$

Written out: $e^{-x^2} = 1 - x^2 + \frac{x^4}{2!} - \frac{x^6}{3!} + \frac{x^8}{4!} - \cdots$

**Step 2 — Integrate term by term from 0 to 0.5.**

$$\int_0^{1/2} e^{-x^2} \, dx = \int_0^{1/2} \left(1 - x^2 + \frac{x^4}{2} - \frac{x^6}{6} + \cdots\right) dx$$

$$= \left[x - \frac{x^3}{3} + \frac{x^5}{10} - \frac{x^7}{42} + \frac{x^9}{216} - \cdots\right]_0^{1/2}$$

**Step 3 — Evaluate at $x = 1/2$.**

$$= \frac{1}{2} - \frac{1/8}{3} + \frac{1/32}{10} - \frac{1/128}{42} + \frac{1/512}{216} - \cdots$$

$$= 0.5 - 0.041\overline{6} + 0.003125 - 0.0001860 + 0.0000090 - \cdots$$

**Step 4 — Determine how many terms to keep.**

This is an alternating series — the error is bounded by the first omitted term. We want error $< 0.001$.

- 3rd term: $0.003125 > 0.001$ — not enough
- 4th term: $0.0001860 < 0.001$ — once we include through the 3rd term, the 4th term bounds the error at $0.000186 < 0.001$. Three terms suffice.

**Step 5 — Compute.**

$$\int_0^{1/2} e^{-x^2} \, dx \approx 0.5 - 0.04167 + 0.003125 = 0.4615$$

The exact value (from numerical integration) is approximately $0.4612$. Our estimate is off by $0.0003 < 0.001$. ✓

**Why this matters:** $e^{-x^2}$ has no closed-form antiderivative — it cannot be expressed in terms of elementary functions. Taylor series are the only way to compute this integral analytically, and the alternating series error bound gives you control over precision.

## Gotchas

**Gotcha 1 — Forgetting the factorial denominators.** The $n!$ in the denominator is not optional. The coefficient is $\frac{f^{(n)}(a)}{n!}$, not $f^{(n)}(a)$. A series without the factorial doesn't converge to the right function.

**Gotcha 2 — Assuming the series converges everywhere.** Some series have $R = \infty$ ($e^x$, $\sin x$, $\cos x$), but others have finite radii. $\ln(1+x)$ only converges for $-1 < x \leq 1$. Using the series outside this range gives wrong answers.

**Gotcha 3 — Confusing Taylor polynomial with Taylor series.** A Taylor polynomial $T_n(x)$ is a finite approximation. The Taylor series is the limit of those polynomials. $T_n(x) = f(x)$ only in the limit as $n \to \infty$, and only within the radius of convergence.

**Gotcha 4 — Differentiating or integrating without checking the radius.** You can differentiate and integrate power series term by term, but only within the radius of convergence. The result may have a different interval of convergence at the endpoints — check those separately.

**Gotcha 5 — Using the wrong center for the approximation task.** If you need to approximate $\sin(3.14)$, expanding around $a = \pi$ gives a much faster-converging series than expanding around $a = 0$, because $(3.14 - \pi)$ is tiny while $(3.14 - 0) = 3.14$ is not.

**Gotcha 6 — The series represents $f$ inside the radius, but may not represent it outside.** A function can have a Taylor series that converges for all $x$ but to the *wrong* value. This happens with "flat" functions like $e^{-1/x^2}$ (whose Maclaurin series is identically zero, not the function). This is rare in Calc 2 but worth knowing: convergence of the Taylor series and equality to the function are two separate conditions.

## See Also

- [[Sequences and Series]] — the language of convergence that makes "infinite polynomial" rigorous
- [[Convergence Tests]] — the ratio test determines the radius of convergence of any Taylor series
- [[Integration by Parts]] — IBP appears in the derivation of the Taylor remainder formula (Lagrange form)
- [[Trigonometric Substitution]] — trig sub can derive series for $\arcsin x$ and $\arctan x$ via integration of geometric series
- [[Newton's Second Law]] — solving $F = ma$ differential equations via Taylor-expanded solutions is a core technique in mechanics
- [[Voltage, Current, and Resistance]] — phasor analysis in AC circuits uses Euler's formula $e^{i\theta} = \cos\theta + i\sin\theta$, a direct consequence of the Taylor series for $e^x$
