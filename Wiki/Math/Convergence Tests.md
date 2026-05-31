# Convergence Tests

**One-liner:** A toolkit of tests — Ratio, Integral, Comparison, and others — that determine whether an infinite series converges to a finite value or diverges to infinity, each working for a different structural reason.

## Why It Exists

You cannot always find a closed-form formula for the partial sums of a series. The harmonic series, for instance: $\sum \frac{1}{n}$ — the partial sums grow without bound, but slowly enough that it's not obvious. You need systematic methods to decide convergence without computing an explicit sum. Convergence tests are those methods.

In engineering and physics, this is not academic. Fourier series expansions of signals, numerical methods for solving differential equations, and power series representations of circuit behavior all depend on knowing: is this infinite sum trustworthy? Does it converge to the answer I think it does? Knowing *which* test to apply (and *why* it works) is the difference between a reliable model and a wrong one.

See [[Sequences and Series]] for the foundational definitions. This entry assumes you already know what convergence and partial sums mean.

## The Concept

Each test below works by comparing a given series to something we already understand, or by exploiting a specific structural feature of the terms. Understanding why they work — not just the rule — is what lets you choose the right one under exam pressure.

---

### Test 1: The Divergence Test (Zero Test)

**Rule:** If $\lim_{n \to \infty} a_n \neq 0$, then $\sum a_n$ diverges.

**Why it works:** If partial sums $S_N$ are converging to a finite limit $S$, then the "increments" $a_N = S_N - S_{N-1}$ must go to zero, because each new term adds a smaller and smaller amount to an already-converging total. Contrapositive: if the terms don't go to zero, the increments never become negligible, so the partial sums never settle.

**The trap:** The converse fails. $a_n \to 0$ is *necessary* but not *sufficient*. The harmonic series has $\frac{1}{n} \to 0$ but still diverges.

**Use this first.** It's the cheapest test. If the terms don't vanish, you're done — diverges.

---

### Test 2: The Geometric Series Test

**Rule:** $\sum_{n=0}^{\infty} r^n$ converges to $\frac{1}{1-r}$ if $|r| < 1$, and diverges if $|r| \geq 1$.

**Why it works:** The partial sum formula $S_N = \frac{1 - r^{N+1}}{1 - r}$ is exact (see [[Sequences and Series]]). Taking the limit $N \to \infty$: if $|r| < 1$, then $r^{N+1} \to 0$ and $S_N \to \frac{1}{1-r}$. If $|r| \geq 1$, $r^{N+1}$ does not vanish and the partial sums diverge.

**Use when:** You can write the series in the form $\sum C \cdot r^n$ with a constant ratio between successive terms.

---

### Test 3: The Integral Test

**Statement:** If $f$ is a continuous, positive, and **decreasing** function on $[1, \infty)$, and $a_n = f(n)$, then:

$$\sum_{n=1}^{\infty} a_n \text{ converges} \iff \int_1^{\infty} f(x) \, dx \text{ converges}$$

**Why it works — the visual proof.** Draw the function $y = f(x)$ on $[1, \infty)$. Now draw two sets of rectangles:
- **Left rectangles** of width 1, with heights $f(1), f(2), f(3), \ldots$, aligned to the left of each interval. These have total area $\sum_{n=1}^{\infty} a_n$.
- **Right rectangles** with heights $f(2), f(3), f(4), \ldots$, aligned to the right. These have total area $\sum_{n=2}^{\infty} a_n$.

Since $f$ is decreasing, the function curve lies *between* these two staircase approximations:

$$\sum_{n=2}^{\infty} a_n \leq \int_1^{\infty} f(x) \, dx \leq \sum_{n=1}^{\infty} a_n$$

The integral and the series are "sandwiched" together — if one blows up, so does the other; if one stays finite, so does the other. They must have the same convergence behavior.

**The $p$-series derivation using the integral test:**

$$\int_1^{\infty} \frac{1}{x^p} \, dx = \begin{cases} \frac{1}{p-1} & p > 1 \\ \infty & p \leq 1 \end{cases}$$

This is why $\sum \frac{1}{n^p}$ converges iff $p > 1$ — the integral test makes it rigorous.

**Use when:** The terms $a_n$ look like a function you can integrate: $\frac{1}{n^p}$, $\frac{\ln n}{n}$, $\frac{1}{n \ln n}$, etc. Requires that $f$ is positive and decreasing on some tail of the domain.

**Note:** The integral test tells you whether convergence happens — it does NOT give you the sum. The sum $\sum \frac{1}{n^2} = \frac{\pi^2}{6}$ cannot be extracted from $\int_1^\infty \frac{1}{x^2} dx = 1$; those are different numbers.

---

### Test 4: The Comparison Test

**Statement:** Suppose $0 \leq a_n \leq b_n$ for all (sufficiently large) $n$.
- If $\sum b_n$ converges, then $\sum a_n$ converges.
- If $\sum a_n$ diverges, then $\sum b_n$ diverges.

**Why it works:** Think of partial sums as accumulating area. If $a_n \leq b_n$, the partial sums $A_N \leq B_N$ at every step. If the "larger" series has bounded partial sums, the "smaller" series certainly does too. Conversely, if the "smaller" series grows without bound, the "larger" must do the same — it's always at least as big.

**Choosing the comparison:** You need a "known" series to compare with. The two standard references are geometric series and $p$-series. The key is choosing one that dominates the right way: to prove convergence, find a *larger* convergent series; to prove divergence, find a *smaller* divergent series.

**Use when:** Your series resembles a $p$-series or geometric series but with minor perturbations. If your terms are rational functions of $n$ (polynomials in numerator and denominator), comparison test often works.

---

### Test 5: The Limit Comparison Test

**Statement:** Let $a_n > 0$, $b_n > 0$, and suppose:

$$\lim_{n \to \infty} \frac{a_n}{b_n} = L, \quad 0 < L < \infty$$

Then $\sum a_n$ and $\sum b_n$ either both converge or both diverge.

**Why it works:** If $\frac{a_n}{b_n} \to L$, then for large $n$, $a_n \approx L \cdot b_n$. The series $\sum a_n$ is essentially $L$ times the series $\sum b_n$ (in the limit). Multiplying a series by a positive constant doesn't change convergence — so they share the same fate.

**Why this is often easier than the Direct Comparison Test:** The Direct Comparison requires you to establish an inequality. The Limit Comparison only requires finding the *ratio limit* — typically done by keeping only the dominant terms in the numerator and denominator (the leading terms as $n \to \infty$).

**Example of "dominant terms" intuition:**
$$a_n = \frac{3n^2 + 2n}{5n^4 - n^3 + 7}$$

The dominant behavior as $n \to \infty$: numerator $\approx 3n^2$, denominator $\approx 5n^4$, so $a_n \approx \frac{3}{5n^2}$. Compare with $b_n = \frac{1}{n^2}$ (a $p$-series with $p=2$, which converges). Compute the limit: $\frac{a_n}{b_n} = \frac{3n^2+2n}{5n^4-n^3+7} \cdot n^2 \to \frac{3}{5}$. Since $L = 3/5$ is finite and positive, and $\sum \frac{1}{n^2}$ converges, $\sum a_n$ also converges.

---

### Test 6: The Ratio Test

**Statement:** Let $\rho = \lim_{n \to \infty} \left|\frac{a_{n+1}}{a_n}\right|$.

- If $\rho < 1$: series **converges absolutely**.
- If $\rho > 1$: series **diverges**.
- If $\rho = 1$: **inconclusive** — test fails, try another.

**Why it works:** If the ratio of consecutive terms converges to $\rho < 1$, then eventually every term is less than $\rho$ times the previous term. This means the series is eventually dominated by a geometric series with ratio $\rho < 1$ — which converges. Formally:

For large enough $N$, the tail of the series satisfies:
$$\sum_{n=N}^{\infty} a_n \leq a_N \sum_{n=0}^{\infty} \rho^n = \frac{a_N}{1-\rho} < \infty$$

If $\rho > 1$, terms are eventually *growing*, so they can't go to zero, and by the Divergence Test the series diverges.

**Why it fails at $\rho = 1$:** Both $\sum \frac{1}{n}$ (diverges) and $\sum \frac{1}{n^2}$ (converges) give $\rho = 1$ in the ratio test. The test cannot distinguish them.

**Use when:** The terms involve factorials ($n!$), exponentials ($c^n$), or products of terms where consecutive ratios simplify cleanly. Factorials almost always call for the ratio test.

**The factorial key:** $\frac{(n+1)!}{n!} = n+1$. This is why the ratio test destroys factorial expressions — they collapse to a simple product.

---

### Test 7: The Root Test

**Statement:** Let $\rho = \lim_{n \to \infty} \sqrt[n]{|a_n|}$.

- If $\rho < 1$: series **converges absolutely**.
- If $\rho > 1$: series **diverges**.
- If $\rho = 1$: **inconclusive**.

**Why it works:** If $\sqrt[n]{|a_n|} \to \rho < 1$, then for large $n$, $|a_n| \approx \rho^n$ — again, geometric domination. The root test gives the same verdict as the ratio test (they detect the same geometric behavior) but via a different calculation.

**Use when:** The terms have the form $a_n = (f(n))^n$ — something raised to the $n$th power. The $n$th root then strips away the exponent.

---

### Test 8: The Alternating Series Test (Leibniz Test)

**Statement:** The series $\sum (-1)^n b_n$ converges if:
1. $b_n > 0$ for all $n$ (terms are positive)
2. $b_n$ is **decreasing**: $b_{n+1} \leq b_n$
3. $\lim_{n \to \infty} b_n = 0$

**Why it works:** The partial sums of an alternating series oscillate — adding a positive term, then subtracting a smaller negative term, then adding an even smaller positive term... The oscillations narrow in like a shrinking zigzag, converging on the limit from both sides.

**Alternating Series Estimation:** The error in truncating after $N$ terms is at most $|b_{N+1}|$ — the magnitude of the first omitted term. This is a remarkable and useful bound.

**Absolute vs. Conditional Convergence:** 
- $\sum a_n$ converges **absolutely** if $\sum |a_n|$ converges. Absolutely convergent series can be rearranged in any order without affecting the sum.
- $\sum a_n$ converges **conditionally** if $\sum a_n$ converges but $\sum |a_n|$ diverges. The alternating harmonic series $\sum \frac{(-1)^n}{n}$ is the classic example — it converges, but if you take absolute values you get the harmonic series, which diverges.

---

### Which Test to Use — Decision Flow

```
Start here:
1. Do terms go to 0?  No → DIVERGES (Divergence Test)
2. Does it look geometric?  Yes → Geometric Series Test
3. Are there factorials or exponentials in terms?  Yes → Ratio Test
4. Are terms of the form (f(n))^n?  Yes → Root Test
5. Does it alternate?  Yes → Alternating Series Test
6. Are terms rational functions of n?  Yes → Limit Comparison with p-series
7. Can I integrate a(n) = f(n)?  Yes → Integral Test
8. Can I bound a_n above/below a known series?  Yes → Direct Comparison
```

## Intuition

Every test is secretly just asking: "can I squeeze this unknown series between things I already understand?"

The Ratio Test says: "does this series eventually look like a geometric series?" (Geometric = understood.)

The Integral Test says: "can I replace this discrete sum with a continuous integral?" (Integrals = understood.)

The Comparison Tests say: "is this series sandwiched above/below something known?" (Known series = understood.)

When you're choosing a test, your first thought should be: "what does this series *look like* in the long run?" The dominant behavior as $n \to \infty$ determines everything — all sub-leading terms wash out. That's why the Limit Comparison Test is so powerful: it formalizes "in the long run, these series look alike."

## Key Formula / Rule

| Test | Condition | Conclusion |
|---|---|---|
| Divergence | $\lim a_n \neq 0$ | Diverges |
| Geometric | $\sum Cr^n$, $\|r\| < 1$ | Converges to $\frac{C}{1-r}$ |
| Integral | $\int_1^\infty f(x)\,dx$ converges | $\sum f(n)$ converges |
| Comparison | $0 \leq a_n \leq b_n$ | Same fate |
| Limit Comparison | $\lim \frac{a_n}{b_n} = L \in (0, \infty)$ | Same fate |
| Ratio | $\lim \left\|\frac{a_{n+1}}{a_n}\right\| = \rho$ | $\rho < 1$: converges; $\rho > 1$: diverges; $\rho = 1$: fail |
| Root | $\lim \sqrt[n]{\|a_n\|} = \rho$ | Same as ratio |
| Alternating Series | $b_n \searrow 0$ in $\sum(-1)^n b_n$ | Converges |

## Worked Example

**Problem:** Determine whether $\displaystyle\sum_{n=1}^{\infty} \frac{n!}{3^n}$ converges or diverges.

**Step 1 — Identify features.**

The terms contain $n!$ (factorial). This is the signature of the **Ratio Test**.

**Step 2 — Compute $\left|\frac{a_{n+1}}{a_n}\right|$.**

$$\frac{a_{n+1}}{a_n} = \frac{(n+1)!}{3^{n+1}} \cdot \frac{3^n}{n!} = \frac{(n+1)! \cdot 3^n}{n! \cdot 3^{n+1}}$$

Simplify $(n+1)! = (n+1) \cdot n!$, so $\frac{(n+1)!}{n!} = n+1$:

$$= \frac{(n+1) \cdot 3^n}{3^{n+1}} = \frac{n+1}{3}$$

**Step 3 — Take the limit.**

$$\rho = \lim_{n \to \infty} \frac{n+1}{3} = \infty$$

**Step 4 — Conclude.**

$\rho = \infty > 1$, so the series **diverges**.

**Why it makes sense:** Factorials grow *faster* than any exponential. $n!$ eventually dwarfs $3^n$ — the terms blow up, not to zero. The Ratio Test confirms this rigorously.

---

**Second Example:** Determine whether $\displaystyle\sum_{n=2}^{\infty} \frac{1}{n(\ln n)^2}$ converges.

**Step 1 — Identify features.** Terms involve $\frac{1}{n \cdot (\ln n)^2}$ — rational-ish, but the $\ln n$ factor rules out a clean $p$-series comparison. The function $f(x) = \frac{1}{x(\ln x)^2}$ is continuous, positive, and decreasing on $[2, \infty)$, and we can integrate it. **Integral Test.**

**Step 2 — Evaluate $\int_2^{\infty} \frac{1}{x (\ln x)^2} \, dx$.**

Substitution: let $u = \ln x$, so $du = \frac{1}{x} dx$:

$$\int_2^{\infty} \frac{1}{x(\ln x)^2} \, dx = \int_{\ln 2}^{\infty} \frac{1}{u^2} \, du = \left[-\frac{1}{u}\right]_{\ln 2}^{\infty} = 0 - \left(-\frac{1}{\ln 2}\right) = \frac{1}{\ln 2}$$

**Step 3 — Conclude.**

The integral converges (it equals $\frac{1}{\ln 2}$), so by the Integral Test, the series $\sum \frac{1}{n(\ln n)^2}$ **converges**.

## Gotchas

**Gotcha 1 — Using the Divergence Test to "prove convergence."** The Divergence Test only proves *divergence*. If the terms go to zero, all you know is that the test was inconclusive. You've learned nothing about convergence. This is the most common misuse.

**Gotcha 2 — Ratio Test giving $\rho = 1$.** This is the test telling you "I can't decide — try something else." Students sometimes conclude convergence or divergence from $\rho = 1$. Neither is justified. Switch to the comparison or integral test.

**Gotcha 3 — Integral Test gives you convergence, not the sum.** If $\int_1^\infty f(x) \, dx = 5$, that does NOT mean $\sum f(n) = 5$. The integral and the series have the same convergence behavior, but generally different values.

**Gotcha 4 — Comparison Test: direction matters.** To prove convergence, you need to find a convergent series that is *bigger than or equal to* your series. To prove divergence, you need a divergent series that is *smaller than or equal to* your series. Getting the direction backwards proves nothing.

**Gotcha 5 — Applying tests where conditions aren't met.** The Integral Test requires $f$ to be positive and decreasing. The Alternating Series Test requires the terms to be decreasing in magnitude. The Comparison Test requires positive terms ($a_n \geq 0$). Check the conditions before applying the test.

**Gotcha 6 — Conditional vs. absolute convergence in higher math.** If a series converges conditionally (not absolutely), the Riemann rearrangement theorem says you can rearrange the terms to make the series converge to *any* number — or even diverge. This is not an exam concern in Calc 2, but it shows why absolute convergence is the "real" convergence.

## See Also

- [[Sequences and Series]] — definitions of convergence and partial sums; read this before Convergence Tests
- [[Taylor and Maclaurin Series]] — the radius of convergence of a power series is determined by the ratio test applied to the series' coefficients
- [[Integration by Parts]] — IBP often needed to evaluate the improper integrals that appear in the integral test
- [[Trigonometric Substitution]] — trig sub sometimes needed to evaluate comparison integrals
- [[Voltage, Current, and Resistance]] — Fourier series convergence in circuit analysis relies on Dirichlet conditions, which are the EE version of these tests
