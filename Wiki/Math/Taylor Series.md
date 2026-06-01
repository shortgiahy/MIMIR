# Taylor Series

**One-liner:** The unique infinite polynomial centered at a point a that exactly matches a smooth function and all of its derivatives at that point — the master tool for approximating any differentiable function.

## Core Idea
$$f(x) = \sum_{n=0}^{\infty} \frac{f^{(n)}(a)}{n!}(x-a)^n = f(a) + f'(a)(x-a) + \frac{f''(a)}{2!}(x-a)^2 + \cdots$$

The coefficient of $(x-a)^n$ is forced: it must be $f^{(n)}(a)/n!$ — nothing else would make the nth derivative of the polynomial match the nth derivative of $f$ at $a$. The **Nth-order Taylor polynomial** $T_N(x)$ uses only terms up to $n = N$; the **remainder** $R_N(x) = f(x) - T_N(x)$ measures the error. By Taylor's theorem:
$$R_N(x) = \frac{f^{(N+1)}(z)}{(N+1)!}(x-a)^{N+1}$$
for some $z$ between $a$ and $x$ (Lagrange form). When $R_N(x) \to 0$ as $N \to \infty$ for $x$ in some interval, the series **converges to** $f(x)$ on that interval.

## Why It Exists
Computers, calculators, and pen-and-paper analysts all share the same problem: the functions they most need (sin, cos, eˣ, ln, √) cannot be evaluated in a finite number of arithmetic steps from their definitions. The Taylor series replaces them with polynomials — which can be evaluated with only addition and multiplication. Historically, Taylor (1715) and Brook Taylor's contemporaries needed a systematic way to extend the linearization idea (tangent line approximation) to higher-order accuracy. The result was a complete theory of function approximation.

## Real-World Applications

**GPS triangulation:** A GPS receiver computes its position by solving a nonlinear system of equations involving distances to satellites. The core algorithm (Extended Kalman Filter) linearizes nonlinear measurement equations using a first-order Taylor expansion at the current state estimate. This first-order approximation runs hundreds of times per second inside your phone. Higher-order terms would improve accuracy but require more computation — the Taylor order is a literal engineering tradeoff.

**Physics simulators (games and robotics):** Game engines and robot motion planners integrate differential equations forward in time. The Euler method is the zeroth/first-order Taylor approximation of the solution. Runge-Kutta (RK4) is a cleverly-weighted fourth-order Taylor method — it achieves fourth-order accuracy in step size h (error ∝ h⁴) by evaluating four first-order approximations per step. Every integrator in every physics engine is a truncated Taylor series.

**How sin and cos are computed in hardware:** A CPU does not have a "sine circuit." The x87 FPU and ARM NEON use CORDIC (COordinate Rotation DIgital Computer), which is equivalent to evaluating a rational approximation of the Taylor series for sin and cos. The Maclaurin series $\sin x = x - x^3/6 + x^5/120 - \cdots$ is used directly in lower-precision embedded systems (microcontrollers for servos, sensors).

**Gradient Descent (Machine Learning):** The loss function $L(\theta + \Delta\theta)$ expanded around current parameters $\theta$ is:
$$L(\theta + \Delta\theta) \approx L(\theta) + \nabla L(\theta)^T \Delta\theta$$
This is a first-order Taylor approximation. Gradient descent follows $-\nabla L$ because the linear term is the only one it uses — it is literaly optimizing the first-order Taylor model of the loss. Second-order methods (Newton's method, Adam optimizer's momentum) incorporate the Hessian (second derivative). All of deep learning optimization is applied Taylor series approximation.

**Linearization in control systems:** A nonlinear plant (robot joint, drone dynamics, chemical reactor) is linearized around an operating point using a first-order Taylor expansion. The linear model is then used to design a PID or LQR controller. Every linear control system built for a nonlinear plant uses a Taylor series approximation — implicitly.

**Error function and probability:** The Gaussian integral $\text{erf}(x) = \frac{2}{\sqrt{\pi}}\int_0^x e^{-t^2}dt$ has no closed form. Its Taylor series (from expanding $e^{-t^2}$ and integrating term-by-term) is the only exact way to compute it — and it appears in every noise and probability calculation in communications engineering.

## Intuition
**Why are the coefficients $f^{(n)}(a)/n!$?**

Suppose you want a polynomial $P(x) = c_0 + c_1(x-a) + c_2(x-a)^2 + \cdots$ that matches $f$ and all its derivatives at $x = a$. Evaluate $P$ and its derivatives at $x = a$:

- $P(a) = c_0 \Rightarrow c_0 = f(a)$
- $P'(a) = c_1 \Rightarrow c_1 = f'(a)$
- $P''(a) = 2c_2 \Rightarrow c_2 = f''(a)/2!$
- $P'''(a) = 6c_3 \Rightarrow c_3 = f'''(a)/3!$
- $P^{(n)}(a) = n!\,c_n \Rightarrow c_n = f^{(n)}(a)/n!$

The $n!$ comes from repeatedly differentiating $(x-a)^n$: $\frac{d^n}{dx^n}(x-a)^n = n!$. The coefficients are uniquely determined — there is one and only one power series that matches all derivatives, and it is the Taylor series.

**The remainder tells you when to stop.** Taylor's theorem with the Lagrange remainder:
$$|R_N(x)| = \left|\frac{f^{(N+1)}(z)}{(N+1)!}\right||x-a|^{N+1} \le \frac{M}{(N+1)!}|x-a|^{N+1}$$
where $M = \max|f^{(N+1)}|$ on the interval. For smooth functions, the $(N+1)!$ in the denominator eventually dominates, driving the error to zero.

## Derivation
**Taylor's Theorem with Lagrange Remainder:**

**Goal:** Show $f(x) = T_N(x) + R_N(x)$ where $R_N(x) = \dfrac{f^{(N+1)}(z)}{(N+1)!}(x-a)^{N+1}$ for some $z \in (a,x)$.

**Proof by generalized MVT:**

Fix $x$ and define $g(t) = f(x) - \sum_{k=0}^N \frac{f^{(k)}(t)}{k!}(x-t)^k$ and $h(t) = (x-t)^{N+1}$.

Note: $g(x) = 0$ and $g(a) = f(x) - T_N(x) = R_N(x)$, while $h(x) = 0$ and $h(a) = (x-a)^{N+1}$.

By the generalized Mean Value Theorem, $\exists z \in (a,x)$:
$$\frac{g(x) - g(a)}{h(x) - h(a)} = \frac{g'(z)}{h'(z)}$$

Compute $g'(t)$: all terms telescope, leaving:
$$g'(t) = -\frac{f^{(N+1)}(t)}{N!}(x-t)^N$$

And $h'(t) = -(N+1)(x-t)^N$.

So:
$$\frac{0 - R_N(x)}{0 - (x-a)^{N+1}} = \frac{-f^{(N+1)}(z)(x-z)^N/N!}{-(N+1)(x-z)^N}$$

$$\frac{R_N(x)}{(x-a)^{N+1}} = \frac{f^{(N+1)}(z)}{(N+1)!}$$

$$\Rightarrow R_N(x) = \frac{f^{(N+1)}(z)}{(N+1)!}(x-a)^{N+1} \qquad \square$$

**Convergence condition:** The Taylor series converges to $f(x)$ iff $R_N(x) \to 0$ as $N \to \infty$.

## Worked Example
**Problem:** Find the Taylor series for $f(x) = \ln x$ centered at $a = 1$, and estimate $\ln(1.1)$ with error < 0.001.

**Step 1 — Compute derivatives at $a = 1$.**

| $n$ | $f^{(n)}(x)$ | $f^{(n)}(1)$ |
|-----|-------------|--------------|
| 0 | $\ln x$ | 0 |
| 1 | $x^{-1}$ | 1 |
| 2 | $-x^{-2}$ | -1 |
| 3 | $2x^{-3}$ | 2 |
| 4 | $-6x^{-4}$ | -6 |
| n | $(-1)^{n+1}(n-1)!\,x^{-n}$ | $(-1)^{n+1}(n-1)!$ |

**Step 2 — Write the series.**
$$\ln x = \sum_{n=1}^\infty \frac{(-1)^{n+1}(n-1)!}{n!}(x-1)^n = \sum_{n=1}^\infty \frac{(-1)^{n+1}}{n}(x-1)^n$$
$$= (x-1) - \frac{(x-1)^2}{2} + \frac{(x-1)^3}{3} - \cdots$$

**Step 3 — Estimate $\ln(1.1)$, error < 0.001.** At $x = 1.1$: $(x-1) = 0.1$. This is an alternating series (for $0 < x \le 2$), so $|R_N| \le a_{N+1} = \dfrac{(0.1)^{N+1}}{N+1}$.

Need $\dfrac{(0.1)^{N+1}}{N+1} < 0.001$: at $N=2$, error ≤ $(0.1)^3/3 \approx 0.000333 < 0.001$. ✓

**Step 4 — Compute $T_2(1.1)$:**
$$T_2(1.1) = 0.1 - \frac{(0.1)^2}{2} = 0.1 - 0.005 = 0.0950$$

True value: $\ln(1.1) \approx 0.09531$. Error: $|0.09531 - 0.0950| \approx 0.00031 < 0.001$. ✓

## See Also
- [[Maclaurin Series]] — Taylor series at $a = 0$; the five essential series
- [[Power Series]] — the general framework Taylor series live in
- [[Radius of Convergence]] — determines where the Taylor series is valid
- [[Alternating Series Test]] — applies to Taylor series with alternating signs; gives error bounds
- [[Gradient Descent]] — uses first-order Taylor approximation of the loss landscape
- [[Linearization]] — the first-order Taylor approximation used in control theory and EE
- [[Integration by Parts]] — often used to prove Taylor's theorem in alternative forms
- [[Euler's Formula]] — derived via Taylor series in [[Maclaurin Series]]; bridges complex exponentials to trig functions
- [[p-Series]] — comparison target for radius of convergence analyses in Taylor series
