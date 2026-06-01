# Maclaurin Series

**One-liner:** A Taylor series centered at zero — the five essential ones (eˣ, sin x, cos x, ln(1+x), 1/(1−x)) are the alphabet of mathematical physics, engineering, and machine learning.

## Core Idea
$$f(x) = \sum_{n=0}^{\infty} \frac{f^{(n)}(0)}{n!}\,x^n = f(0) + f'(0)x + \frac{f''(0)}{2!}x^2 + \frac{f'''(0)}{3!}x^3 + \cdots$$

A Maclaurin series is a [[Taylor Series]] with center $a = 0$. The five essential series and their intervals of convergence:

| Function | Maclaurin Series | Interval |
|----------|-----------------|----------|
| $e^x$ | $\displaystyle\sum_{n=0}^\infty \frac{x^n}{n!} = 1 + x + \frac{x^2}{2!} + \frac{x^3}{3!} + \cdots$ | $(-\infty, \infty)$ |
| $\sin x$ | $\displaystyle\sum_{n=0}^\infty \frac{(-1)^n x^{2n+1}}{(2n+1)!} = x - \frac{x^3}{6} + \frac{x^5}{120} - \cdots$ | $(-\infty, \infty)$ |
| $\cos x$ | $\displaystyle\sum_{n=0}^\infty \frac{(-1)^n x^{2n}}{(2n)!} = 1 - \frac{x^2}{2} + \frac{x^4}{24} - \cdots$ | $(-\infty, \infty)$ |
| $\ln(1+x)$ | $\displaystyle\sum_{n=1}^\infty \frac{(-1)^{n+1} x^n}{n} = x - \frac{x^2}{2} + \frac{x^3}{3} - \cdots$ | $(-1, 1]$ |
| $\dfrac{1}{1-x}$ | $\displaystyle\sum_{n=0}^\infty x^n = 1 + x + x^2 + x^3 + \cdots$ | $(-1, 1)$ |

## Why It Exists
Centering at zero simplifies computation dramatically — all odd or even pattern series collapse into clean forms. The five series above are the building blocks: virtually every other Maclaurin series is derived from these by substitution (replace x with −x², for instance, to get the series for cos x from e^x), multiplication, differentiation, or integration. They also produce one of the most beautiful results in mathematics: Euler's formula.

## Real-World Applications

**How your calculator computes sin(x):** On a microcontroller in a servo motor or IMU sensor, sin(x) is computed as the truncated Maclaurin series $x - x^3/6 + x^5/120$ after range reduction (mapping x to $[-\pi/4, \pi/4]$ using trig identities). Five multiplications and four additions give 10+ decimal digits of accuracy. The same polynomial runs in under 10 nanoseconds on modern hardware.

**The exponential in probability and ML:** The softmax function in neural networks is $e^{z_i}/\sum e^{z_j}$. The Maclaurin series for $e^x$ explains its behavior near zero: $e^x \approx 1 + x + x^2/2$, which is why softmax probabilities are approximately proportional to $(1 + z_i)$ for small logits. Numerical overflow in softmax is handled by the log-sum-exp trick, derived from the same series.

**Signal processing — small-angle approximation:** In a pendulum, $\sin\theta \approx \theta$ for small $\theta$. This is the first term of the Maclaurin series. The linearized pendulum ODE used in every control textbook is a Maclaurin approximation. The error is $\theta^3/6$ — at 10° ($\approx 0.175$ rad), error < 0.1%.

**Information theory — entropy computation:** The Shannon entropy $H = -\sum p_i \ln p_i$ involves $\ln(1+x)$. For probabilities near a uniform distribution, the Maclaurin series gives tractable entropy approximations in information-theoretic proofs.

**Euler's formula and electrical engineering:** $e^{i\omega t} = \cos\omega t + i\sin\omega t$ is the fundamental representation of sinusoidal signals in AC circuits, Fourier analysis, and control theory. It is derived directly from the Maclaurin series (see Derivation section). The entire phasor method in circuit analysis is Euler's formula applied.

## Intuition
Every smooth function, near $x = 0$, is secretly a polynomial. The Maclaurin series reveals this polynomial layer by layer:

- **Zeroth order** ($f(0)$): where does the function start?
- **First order** ($f'(0)x$): which direction does it move, and how fast?
- **Second order** ($f''(0)x^2/2$): is it concave up or down? How much curvature?
- **Higher orders**: finer and finer shape details.

Each coefficient is forced by one derivative value. There's no choice involved — if you want to match all derivatives, the Taylor/Maclaurin series is the unique answer.

**Why sin x has only odd powers:** sin(0) = 0, so $c_0 = 0$. sin''(0) = −sin(0) = 0, so $c_2 = 0$. In general, all even derivatives of sin at 0 vanish (they're all ±sin(0) = 0), so all even-power terms are zero. Similarly, cos x has only even powers because all odd derivatives of cos at 0 vanish. The symmetry of the function (odd/even) is reflected in which powers appear.

## Derivation

### The Five Essential Series

**1. $e^x$:**

All derivatives of $e^x$ are $e^x$, so $f^{(n)}(0) = e^0 = 1$.
$$e^x = \sum_{n=0}^\infty \frac{1}{n!}x^n = 1 + x + \frac{x^2}{2} + \frac{x^3}{6} + \cdots$$

Radius of convergence: $R = \lim |a_n/a_{n+1}| = \lim (n+1)! / n! = \lim(n+1) = \infty$. Converges everywhere.

**2. $\sin x$:**

$f(x) = \sin x$: derivatives cycle with period 4: $\sin x, \cos x, -\sin x, -\cos x, \sin x, \ldots$

At $x=0$: values cycle $0, 1, 0, -1, 0, 1, \ldots$ So $f^{(2k)}(0) = 0$, $f^{(2k+1)}(0) = (-1)^k$.
$$\sin x = \sum_{n=0}^\infty \frac{(-1)^n}{(2n+1)!}x^{2n+1} = x - \frac{x^3}{6} + \frac{x^5}{120} - \cdots$$

Converges everywhere (same factorial domination argument as $e^x$).

**3. $\cos x$:**

Derivative of $\sin x$ term by term:
$$\cos x = \frac{d}{dx}\sin x = \sum_{n=0}^\infty \frac{(-1)^n}{(2n+1)!}(2n+1)x^{2n} = \sum_{n=0}^\infty \frac{(-1)^n}{(2n)!}x^{2n} = 1 - \frac{x^2}{2} + \frac{x^4}{24} - \cdots$$

Converges everywhere.

**4. $\ln(1+x)$:**

Start from $\dfrac{1}{1+x} = \dfrac{1}{1-(-x)} = \sum_{n=0}^\infty (-x)^n = \sum_{n=0}^\infty (-1)^n x^n$ for $|x| < 1$.

Integrate term-by-term (using $\ln(1+0) = 0$ for the constant):
$$\ln(1+x) = \int_0^x \frac{dt}{1+t} = \sum_{n=0}^\infty (-1)^n \frac{x^{n+1}}{n+1} = \sum_{n=1}^\infty \frac{(-1)^{n+1}}{n}x^n$$

Radius $R = 1$. At $x=1$: alternating harmonic series, converges by [[Alternating Series Test]] → endpoint included. At $x = -1$: negative harmonic series, diverges. Interval: $(-1, 1]$.

**5. $\dfrac{1}{1-x}$ (Geometric Series):**

This is the [[Geometric Series]] with ratio $x$: $\sum_{n=0}^\infty x^n$. Converges for $|x| < 1$, diverges for $|x| \ge 1$. This one formula, rewritten as $\dfrac{1}{1-(x)} = 1 + x + x^2 + \cdots$, is the source of most other Maclaurin series via substitution.

---

### Euler's Formula: $e^{i\theta} = \cos\theta + i\sin\theta$

Substitute $x = i\theta$ into the Maclaurin series for $e^x$:
$$e^{i\theta} = \sum_{n=0}^\infty \frac{(i\theta)^n}{n!} = 1 + i\theta + \frac{(i\theta)^2}{2!} + \frac{(i\theta)^3}{3!} + \frac{(i\theta)^4}{4!} + \cdots$$

Use powers of $i$: $i^0=1,\ i^1=i,\ i^2=-1,\ i^3=-i,\ i^4=1,\ \ldots$ (cycle of 4):
$$e^{i\theta} = \left(1 - \frac{\theta^2}{2!} + \frac{\theta^4}{4!} - \cdots\right) + i\left(\theta - \frac{\theta^3}{3!} + \frac{\theta^5}{5!} - \cdots\right)$$

The first parenthesis is exactly the Maclaurin series for $\cos\theta$; the second is exactly $\sin\theta$:
$$\boxed{e^{i\theta} = \cos\theta + i\sin\theta}$$

Setting $\theta = \pi$: $e^{i\pi} = \cos\pi + i\sin\pi = -1 + 0 = -1$, giving **Euler's identity**: $e^{i\pi} + 1 = 0$.

## Worked Example
**Problem:** Find the Maclaurin series for $f(x) = x^2 \sin(x^3)$ without computing derivatives directly.

**Step 1 — Start from the known series.**
$$\sin u = u - \frac{u^3}{6} + \frac{u^5}{120} - \cdots = \sum_{n=0}^\infty \frac{(-1)^n u^{2n+1}}{(2n+1)!}$$

**Step 2 — Substitute $u = x^3$.**
$$\sin(x^3) = x^3 - \frac{x^9}{6} + \frac{x^{15}}{120} - \cdots = \sum_{n=0}^\infty \frac{(-1)^n x^{6n+3}}{(2n+1)!}$$

**Step 3 — Multiply by $x^2$.**
$$x^2\sin(x^3) = x^5 - \frac{x^{11}}{6} + \frac{x^{17}}{120} - \cdots = \sum_{n=0}^\infty \frac{(-1)^n x^{6n+5}}{(2n+1)!}$$

This is far easier than computing the 6th, 12th, 18th derivatives of $x^2\sin(x^3)$ directly.

## See Also
- [[Taylor Series]] — the general version; Maclaurin is the $a=0$ special case
- [[Power Series]] — the algebraic object both are instances of
- [[Radius of Convergence]] — where each series is valid
- [[Euler's Formula]] — the crown jewel, derived above
- [[Geometric Series]] — the 1/(1−x) series; foundation for all others
- [[Alternating Series Test]] — proves convergence of ln(1+x) at x=1, validates sin/cos error bounds
- [[Integration by Parts]] — used to derive Taylor remainder; also connects ∫sin, ∫cos back to series
- [[Gradient Descent]] — first-order Maclaurin approximation of the loss function
- [[Fourier Series]] — decomposes signals using sin and cos; Maclaurin series connects back via Euler's formula
- [[Complex Numbers]] — Euler's formula lives here; essential for AC circuit analysis and signal processing
- [[Activation Function]] — sigmoid = 1/(1+e^−x) is Taylor-expandable via the e^x series; its linear approximation near 0 explains why deep networks train stably at small weights
- [[Electric Current]] — exponential decay of current in RC circuits (i(t) = I₀e^{−t/RC}) uses the e^x Maclaurin series for small-time approximations
