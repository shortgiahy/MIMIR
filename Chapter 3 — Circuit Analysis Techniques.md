## Pick a method in 10 seconds

| Circuit clue | Start with |

|---|---|

| Obvious series/parallel | Reduce, dividers (Ch. 2) |

| Few unknown nodes / current sources | **Nodal** (KCL) |

| Few meshes / voltage sources | **Mesh** (KVL) |

| Voltage source between unknown nodes | **Supernode** |

| Current source between adjacent meshes | **Supermesh** |

| Several independent sources | **Superposition** |

| Two-terminal equivalent / varying load | **Thévenin / Norton** |

| Max power in load | **Thévenin + $R_L=R_{Th}$** |

  

## 3.1 Linearity

- **Homogeneity:** scale input $\Rightarrow$ response scales.

- **Additivity:** responses to independent inputs add.

- Fixed resistors and linear dependent sources: linear. **Do not superimpose power** ($p\propto i^2$ or $v^2$).

  

## 3.2 Nodal analysis (KCL): follow exactly

1. Pick **ground** ($0$ V). Label unknown nodes $V_1,V_2,\ldots$.

2. For **each unknown node**, write **currents leaving = 0**.

3. Resistor from node $a$ to $b$: $\boxed{(V_a-V_b)/R}$; to ground: $V_a/R$.

4. Source arrow **leaving** node: $+I_s$; **entering**: $-I_s$.

5. Solve simultaneous equations. Requested branch current: $I=(V_a-V_b)/R$.

  

**Template:** node $V_1$ linked to $V_2$ by $R_{12}$, ground by $R_g$, known voltage $V_s$ by $R_s$, with current $I_s$ entering:

$$\boxed{\frac{V_1-V_2}{R_{12}}+\frac{V_1}{R_g}+\frac{V_1-V_s}{R_s}-I_s=0}$$

  

**Supernode:** voltage source between **two unknown nodes**.

1. Enclose both nodes and source; write **one KCL around outside boundary**.

2. Add source constraint $\boxed{V_a-V_b=V_s}$ (check polarity).

3. Solve KCL + constraint. For dependent source, use $V_a-V_b=\mu V_x$ and express $V_x$ from circuit.

  

## 3.3 Mesh analysis (KVL): follow exactly

1. Use only on **planar** circuits. Draw smallest loops; assign all mesh currents **clockwise** ($I_1,I_2,\ldots$).

2. For each mesh, sum resistor **drops** and source **drops** to zero.

3. Exclusive resistor: $RI_1$; shared resistor: $\boxed{R(I_1-I_2)}$ for mesh 1.

4. Voltage source traversed $-\to+$: **$-V_s$ drop**; $+\to-$: **$+V_s$ drop**.

5. Solve; actual shared-branch current is signed difference of mesh currents.

  

**Template:** two meshes, left exclusive $R_1$, right exclusive $R_2$, shared $R_s$, left source rise $V_s$:

$$\boxed{(R_1+R_s)I_1-R_sI_2=V_s}$$

$$\boxed{-R_sI_1+(R_2+R_s)I_2=0}$$

  

**Current source on outer edge:** mesh current fixed by arrow: $I_k=\pm I_s$; skip KVL for that mesh.

  

**Supermesh:** current source shared between two meshes.

1. Draw one outer loop around both meshes, excluding current-source branch.

2. Write KVL on outer loop.

3. Add constraint $\boxed{I_1-I_2=\pm I_s}$ based on source arrow; solve both.

  

**Quiz trap:** if right mesh is clockwise, its current travels **down** right-hand edge. An upward $10\,\mathrm{mA}$ source means $I_2=-10\,\mathrm{mA}$.

  

## 3.4 By-inspection equations

- **Nodal matrix:** $\boxed{\mathbf G\mathbf V=\mathbf I}$; diagonal = sum connected conductances $1/R$; off-diagonal = negative shared conductance; RHS = net current injected.

- **Mesh matrix:** $\boxed{\mathbf R\mathbf I=\mathbf V}$; diagonal = sum resistances in mesh; off-diagonal = negative shared resistance; RHS = signed voltage rises.

- Sources spanning nodes/meshes require **extra constraint equations**.

- **Dependent sources:** retain them and add controlling relation, e.g. $I_x=(V_a-V_b)/R$.

  

## 3.5 Superposition: source-by-source

1. Keep **one independent** source active.

2. Replace other **independent voltage sources with shorts** ($V=0$).

3. Replace other **independent current sources with opens** ($I=0$).

4. **Never deactivate dependent sources.** Keep their control equations.

5. Solve the same desired voltage/current, with same reference polarity/direction.

6. Repeat and add signed results: $\boxed{x=\sum x_k}$.

7. Calculate **power from total** voltage/current, not summed powers.

  

## 3.6 Thévenin / Norton: terminal recipe

**Remove load first. Label terminals $a,b$.**

  

| Quantity | Procedure |

|---|---|

| $V_{Th}$ | **Open** load; find $V_{ab}$ at terminals. |

| $I_N=I_{sc}$ | **Short** terminals; calculate current through short ($a\to b$). |

| $R_{Th}=R_N$ | With **independent** sources deactivated, find resistance looking into terminals. |

| Alternate $R_{Th}$ | If $I_{sc}\ne0$: $\boxed{R_{Th}=V_{oc}/I_{sc}}$. |

| Thévenin circuit | $V_{Th}$ source **in series** with $R_{Th}$. |

| Norton circuit | $I_N$ source **in parallel** with $R_N$. |

  

$$\boxed{V_{Th}=I_NR_{Th}},\qquad \boxed{I_N=\frac{V_{Th}}{R_{Th}}}$$

  

**With dependent sources:**

1. Find $V_{oc}$ with **all original sources active**.

2. For $R_{Th}$, deactivate **independent only**; dependent sources stay active.

3. Attach test voltage $V_t$ at terminals, solve current $I_t$ entering network: $\boxed{R_{Th}=V_t/I_t}$. (Or use test current and solve voltage.)

4. **Black box:** measure $V_{oc}$ and $I_{sc}$, then $R_{Th}=V_{oc}/I_{sc}$ when defined; no access to internal circuit needed.

  

**Load reattached:**

$$\boxed{I_L=\frac{V_{Th}}{R_{Th}+R_L}},\qquad \boxed{V_L=V_{Th}\frac{R_L}{R_{Th}+R_L}}$$

  

## 3.7 Method comparison

- Few nodes? **Nodal.** Few meshes? **Mesh.** Current source between meshes? **Supermesh.** Voltage source between nodes? **Supernode.** Many sources? **Superposition.** Load-terminal question? **Thévenin.**

- Choose method that yields **fewest unknowns/equations**, taking source constraints into account.

  

## 3.8 Maximum power transfer (resistive DC)

$$\boxed{P_L=\frac{V_{Th}^2R_L}{(R_{Th}+R_L)^2}}$$

$$\boxed{R_L=R_{Th}\ \Rightarrow\ P_{L,\max}=\frac{V_{Th}^2}{4R_{Th}}}$$

$$\boxed{\eta=\frac{P_L}{P_L+P_{R_{Th}}}=\frac{R_L}{R_{Th}+R_L}}$$

- At max power: **50% efficiency** (for positive resistances in this model).

- If forced to choose near-matching $R_L$, compare $P_L$ for both candidates; **above** match improves efficiency, but does **not always** give higher power than a candidate below match.

- **Max power**: extracting a signal/available source power. **Max efficiency**: minimizing internal loss/heat; typically $R_L\gg R_{Th}$, though delivered power may decrease.

  

## Final exam checks

- **Polarity/arrows:** negative answer = opposite assumed direction.

- **Source off:** voltage $\to$ short; current $\to$ open; dependent $\to$ stays.

- **Units:** $\mathrm{k}\Omega\times\mathrm{mA}=\mathrm V$; $\mathrm V/\mathrm{k}\Omega=\mathrm{mA}$.

- **Verify:** KCL at a node, KVL around a loop, $\sum p=0$ if needed.

- **Common error:** $R$ in parallel only if both ends connect to the **same two nodes**.