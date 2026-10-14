## 2.1 Ohm's law and resistance

$$\boxed{v=iR},\quad \boxed{R=\rho\frac{L}{A}=\frac{L}{\sigma A}},\quad \boxed{p=vi=i^2R=\frac{v^2}{R}}$$

- $R$ in $\Omega$; $\rho$ resistivity; $\sigma=1/\rho$ conductivity.

- Longer wire $\Rightarrow$ larger $R$; thicker wire $\Rightarrow$ smaller $R$.

- **Ohm's law** $v=iR$ with fixed $R$ describes *ohmic* elements. Real resistance can change with temperature/operating point; not every device is ohmic.

- Temperature approximation: $\boxed{R(T)=R(T_0)[1+\alpha(T-T_0)]}$.

  

## 2.2 Kirchhoff's laws

$$\boxed{\sum I_{\mathrm{in}}=\sum I_{\mathrm{out}}}\quad\text{(KCL: charge conservation)}$$

$$\boxed{\sum_{\mathrm{loop}}v=0}\quad\text{(KVL: voltage rises = drops)}$$

- **KCL:** at each node, pick entering or leaving as positive; stay consistent.

- **KVL:** trace one loop direction; crossing $-\to+$ is a rise, $+\to-$ is a drop.

- Resistor current **from node $a$ to $b$**: $\boxed{i_{a\to b}=(V_a-V_b)/R}$.

- Negative result means actual direction opposes your arrow.

  

## 2.3 Equivalent circuits and dividers

| Configuration | Formula / test |

|---|---|

| **Series** | Same current; no branch at connection; $R_{eq}=\sum R_i$. |

| **Parallel** | Same two endpoints/voltage; $1/R_{eq}=\sum 1/R_i$. |

| Two parallel resistors | $\boxed{R_{eq}=R_1R_2/(R_1+R_2)}$. |

| Voltage divider | $\boxed{V_{R_2}=V_{in}\frac{R_2}{R_1+R_2}}$ (series). |

| Current divider | $\boxed{I_1=I_{in}\frac{R_2}{R_1+R_2}}$ (two parallel branches). |

| Equivalent two-terminal networks | Same terminal $i$–$v$ relationship for any external load. |

  

**Loaded voltage divider:** load $R_L$ across lower $R_2$:

$$R_b=R_2\parallel R_L,\qquad \boxed{V_o=V_{in}\frac{R_b}{R_1+R_b}}$$

Output is **lower** than unloaded case for positive resistors.

  

**Loaded current divider:** additional parallel load takes some source current; recompute parallel conductances and branch currents. Do **not** use the original unloaded split.

  

**Both dividers in one circuit?** Yes: series portions divide voltage; parallel portions divide current.

  

**Reduction algorithm:** (1) mark terminal pair; (2) identify *true* series/parallel connections; (3) replace one group; (4) redraw; (5) repeat. **Quiz:** $1\,\mathrm{k}\Omega+[1\,\mathrm{k}\Omega\parallel(1+1+1)\,\mathrm{k}\Omega]=\boxed{1.75\,\mathrm{k}\Omega}$.

  

### Source transformation (external terminals only)

$$\boxed{V_s\text{ in series with }R\ \Longleftrightarrow\ I_s=\frac{V_s}{R}\text{ in parallel with }R}$$

- Same resistance $R$; choose current-source arrow so open-circuit terminal polarity matches original voltage source.

- Transformation preserves **terminal behavior**, not necessarily internal source/resistor currents or power.

- Voltage source with resistor in **parallel** is *not* the standard transformable pair; current source with resistor in **series** is not either.

  

## 2.4 Wye–Delta ($Y$–$\Delta$)

Let delta resistors be $R_{ab},R_{bc},R_{ca}$; wye arms $R_a,R_b,R_c$.

  

**$\Delta\to Y$:** define $S=R_{ab}+R_{bc}+R_{ca}$:

$$\boxed{R_a=\frac{R_{ab}R_{ca}}{S},\quad R_b=\frac{R_{ab}R_{bc}}{S},\quad R_c=\frac{R_{bc}R_{ca}}{S}}$$

**$Y\to\Delta$:** define $P=R_aR_b+R_bR_c+R_cR_a$:

$$\boxed{R_{ab}=\frac{P}{R_c},\quad R_{bc}=\frac{P}{R_a},\quad R_{ca}=\frac{P}{R_b}}$$

Use when a three-resistor triangle/star blocks simple series/parallel reduction.

  

## Circuit sanity checks

- Series: same current? Parallel: same voltage?

- $R_{parallel}$ must be **smaller than the smallest** parallel resistor.

- Passive resistors absorb nonnegative power: $p=i^2R\ge0$.

- KCL, KVL and $\sum p=0$ should hold.