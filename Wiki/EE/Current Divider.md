# Current Divider

**One-liner:** A current divider describes how total current splits between two or more parallel resistors, with each branch carrying a fraction inversely proportional to its resistance.

## Core Idea
$$I_x = I_{total} \cdot \frac{R_{other}}{R_x + R_{other}}$$
For two parallel resistors $R_1$ and $R_2$ carrying total current $I_{total}$, the current through $R_1$ is $I_1 = I_{total} \cdot R_2/(R_1 + R_2)$. Notice: the formula uses the *other* resistor in the numerator, not $R_1$ itself. Lower resistance attracts more current — the branch that fights least gets the most flow. This is the dual of the [[Voltage Divider]], which uses the *same* resistor in the numerator.

## Why It Exists
Whenever two or more paths exist between the same two nodes, current splits among them. Without a quantitative rule, you cannot predict how much current each branch carries — which means you cannot size components, predict power dissipation, or verify that a sense resistor or protection circuit is receiving the expected current. Engineers formalized the current divider to make parallel-branch analysis as systematic as voltage dividers are for series circuits.

## Real-World Applications
- **Parallel sensor bias networks (Baymax):** Two IR sensors biased from the same current source split current according to their input resistances. A current divider calculation confirms each sensor stays within its operating range.
- **Current sense shunts:** A shunt resistor $R_{shunt}$ is placed in parallel with a load. The fraction of total current through the shunt is $I_{shunt} = I_{total} \cdot R_{load}/(R_{shunt} + R_{load})$. Since $R_{shunt} \ll R_{load}$, only a tiny fraction diverts — but that fraction is measurable.
- **H-bridge motor driver fault detection:** If a winding develops a partial short (lower resistance path), KCL and the current divider predict excess current in the shorted branch — triggering overcurrent protection.
- **Transistor current mirrors:** In analog design, a current mirror forces a precise current through one branch by duplicating it in another. The current divider ratio is controlled by transistor geometry.
- **Parallel battery cells:** Two cells with slightly different internal resistances $r_1, r_2$ in parallel divide charge current according to the current divider. The lower-resistance cell charges faster — a key concern in Baymax's battery management.

## Intuition
Ohm's Law says $I = V/R$. In a parallel circuit, both branches see the *same voltage* (they share the same two nodes). So the branch with lower resistance naturally carries more current — it's not that current "chooses" the easy path; it's that for the same voltage, lower resistance produces higher current by force of Ohm's Law.

The dual relationship with the voltage divider is worth internalizing:

| | Voltage Divider (series) | Current Divider (parallel) |
|---|---|---|
| What's shared | Current | Voltage |
| Formula numerator | Same element | Other element |
| Larger element → | Larger voltage | Smaller current |

The "other element in the numerator" rule for current dividers trips up students constantly. Memorize it with the logic: if $R_2 \to \infty$, all current flows through $R_1$, so $I_1 = I_{total} \cdot R_2/(R_1 + R_2) \to I_{total}$ — the infinity in the numerator gives 100%. The formula handles the edge cases correctly.

## Derivation
**From KCL and Ohm's Law:**

Two resistors $R_1$ and $R_2$ are connected in parallel between nodes A and B. Total current $I_{total}$ enters node A and leaves node B.

**Step 1 — shared voltage.** Both branches span the same two nodes, so by KVL they share the same voltage $V_{AB}$:
$$V_{AB} = I_1 R_1 = I_2 R_2$$

**Step 2 — KCL at node A:**
$$I_{total} = I_1 + I_2$$

**Step 3 — express $I_2$ in terms of $I_1$.**
From the shared voltage: $I_2 = I_1 \cdot R_1/R_2$. Substituting into KCL:
$$I_{total} = I_1 + I_1 \cdot \frac{R_1}{R_2} = I_1 \left(1 + \frac{R_1}{R_2}\right) = I_1 \cdot \frac{R_1 + R_2}{R_2}$$

**Step 4 — solve for $I_1$:**
$$\boxed{I_1 = I_{total} \cdot \frac{R_2}{R_1 + R_2}}$$

By symmetry, $I_2 = I_{total} \cdot R_1/(R_1 + R_2)$.

**Verification:** $I_1 + I_2 = I_{total} \cdot (R_2 + R_1)/(R_1 + R_2) = I_{total}$ ✓

**Conductance form (generalizes to $n$ branches):**

In terms of conductance $G_k = 1/R_k$, for $n$ parallel branches:
$$I_k = I_{total} \cdot \frac{G_k}{G_1 + G_2 + \cdots + G_n}$$
Here the formula uses the *same* element in the numerator — matching the voltage divider form. This is the clean symmetric version, and it extends to any number of branches without modification.

## Worked Example
**Problem:** Baymax's motor controller supplies $I_{total} = 2\,\text{A}$ to two parallel motor windings. Winding A has resistance $R_A = 3\,\Omega$ (cold) and winding B has $R_B = 6\,\Omega$ (higher resistance due to a partial fault). How much current flows through each winding?

**Current divider:**
$$I_A = 2\,\text{A} \times \frac{R_B}{R_A + R_B} = 2 \times \frac{6}{3 + 6} = 2 \times \frac{6}{9} = \frac{12}{9} \approx 1.33\,\text{A}$$
$$I_B = 2\,\text{A} \times \frac{R_A}{R_A + R_B} = 2 \times \frac{3}{9} = \frac{6}{9} \approx 0.67\,\text{A}$$

**Verify KCL:** $1.33 + 0.67 = 2.00\,\text{A}$ ✓

**Verify shared voltage:** $V = I_A \cdot R_A = 1.33 \times 3 = 4\,\text{V}$; $V = I_B \cdot R_B = 0.67 \times 6 = 4\,\text{V}$ ✓

**Interpretation:** Winding A (lower $R$) carries twice the current of Winding B. Power in A: $P_A = I_A^2 R_A = (1.33)^2 \times 3 = 5.3\,\text{W}$. Power in B: $P_B = (0.67)^2 \times 6 = 2.7\,\text{W}$. Winding A dissipates nearly double the power — thermal monitoring should flag it first.

## See Also
- [[Voltage Divider]] — the series-circuit dual; same formula structure but with roles of elements swapped
- [[Kirchhoff's Current Law]] — the current divider is a direct consequence: currents at a parallel node must sum to $I_{total}$
- [[Parallel Circuit]] — the topology that gives rise to current division; every parallel circuit is a current divider
- [[Ohm's Law]] — the mechanism: equal voltage across branches with different R forces unequal current
- [[Kirchhoff's Voltage Law]] — confirms that both branches share the same terminal voltage
- [[Electric Power]] — unequal current split means unequal power dissipation; critical for thermal design
- [[Conservation of Energy]] — total power delivered equals sum of power in each branch
