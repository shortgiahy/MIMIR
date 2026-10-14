## 1.1–1.3 Models and assumptions

| Term | Exam answer |

|---|---|

| **Model** | Simplified representation predicting relevant circuit behavior. |

| **Black box** | Describe a device by its terminal input/output behavior, without internal details. |

| **Ideal components** | Ignore parasitics, losses, tolerances unless explicitly modeled. |

| **All sources given** | Assume all external excitation is represented; no unknown inputs. |

| **Lumped circuit** | Circuit dimensions are small relative to signal wavelength; propagation delays negligible (not literally infinite light speed). |

| **Node / branch / loop** | Connected equal-potential junction / element path between nodes / closed path. |

| **Open / short** | Open: $I=0$; short: $V=0$. |

| **Passive / active** | Passive elements cannot provide net energy gain; sources/active devices can supply energy. |

  

**SI:** $\mathrm{T}=10^{12}$, $\mathrm{G}=10^9$, $\mathrm{M}=10^6$, $\mathrm{k}=10^3$, $\mathrm{m}=10^{-3}$, $\mu=10^{-6}$, $\mathrm{n}=10^{-9}$, $\mathrm{p}=10^{-12}$. Watch **M** vs **m**.

  

## 1.4 Charge and current

$$\boxed{i(t)=\frac{dq(t)}{dt}},\qquad \boxed{q(t)=q(t_0)+\int_{t_0}^{t}i(\tau)\,d\tau}$$

- Charge $q$: coulombs (C); current $i$: amperes (A = C/s).

- Conventional current flows in the direction positive charge would move.

- **Quiz derivative:** $q(t)=e^{-2t}\sin(0.04t)$:

$$\boxed{i(t)=e^{-2t}[0.04\cos(0.04t)-2\sin(0.04t)]\ \mathrm A}$$

- **Current** is defined through a specified cross-section; **voltage** is always *between two points*. Current can differ between branches, so it is **not** identical everywhere.

  

## 1.5 Voltage, energy, power

$$\boxed{v_{ab}=V_a-V_b},\quad \boxed{v=\frac{dw}{dq}},\quad \boxed{p=\frac{dw}{dt}=vi}$$

- Voltage: joules/coulomb (V); power: joules/second (W).

- **Passive sign convention:** current enters terminal marked $+$: $p=+vi$ means **absorbed**; $p<0$ means **delivered**.

- Current entering $-$ terminal: $p=-vi$ for **absorbed** power.

- **Conservation:** $\boxed{\sum p=0}$, including sources and loads.

- Energy over time: $\boxed{w=\int_{t_0}^{t_1}p(t)\,dt}$.

  

## 1.6 Sources and $i$–$v$ behavior

| Element | What it enforces | What adjusts |

|---|---|---|

| Ideal voltage source | $v=V_s$ | Current as needed |

| Ideal current source | $i=I_s$ | Voltage as needed |

| Dependent source | Voltage/current set by another circuit variable | Depends on control variable |

| Resistor | $v=iR$ | Both related by $R$ |

  

- Four controlled-source types: **VCVS** ($v=\mu v_x$), **VCCS** ($i=gv_x$), **CCVS** ($v=ri_x$), **CCCS** ($i=\beta i_x$).

- $i$–$v$ characteristic = relationship between voltage across and current through an element.

- An ideal voltage source and ideal current source are **not generally the same element**: each fixes a different variable independently of its load.

- **Safety:** prevent shorts/overheating (proper wiring, insulation, component ratings, current limits); protect people (de-energize before rewiring, verify discharge, appropriate protection, avoid exposed live conductors).

  

## 15-second checks

- Is voltage polarity marked? Is current arrow marked?

- Does $p$ use the correct sign convention?

- Did you convert $\mathrm{k}\Omega$, mA, and $\mu$ correctly?