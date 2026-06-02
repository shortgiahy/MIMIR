# Parallel Circuit

**One-liner:** In a parallel circuit, all components share the same voltage, and the total current is the sum of individual branch currents.

## Core Idea
$$\frac{1}{R_{total}} = \frac{1}{R_1} + \frac{1}{R_2} + \cdots + \frac{1}{R_n}$$
$$V_{total} = V_1 = V_2 = \cdots = V_n$$
$$I_{total} = I_1 + I_2 + \cdots + I_n$$
Components are connected between the same two nodes — they see identical voltage. Each branch independently draws current according to Ohm's Law. Total current is the sum. Adding more parallel branches always decreases total resistance (more paths = less total opposition).

## Why It Exists
Real power systems must supply multiple independent loads simultaneously. Every USB port, LED driver, sensor, and motor controller in Baymax is connected in parallel across the power rails — they all receive the same supply voltage but draw independent currents. Without understanding parallel circuits, you can't calculate total supply current (and thus battery life), can't size power supplies, and can't understand why adding a load drops rail voltage under high current.

## Real-World Applications
- **Power rails in embedded systems:** Baymax's 5 V rail powers the Raspberry Pi (~800 mA), sensors (~50 mA each), and LED indicators (~20 mA each). All are in parallel. Total current = sum of all branch currents — this determines the regulator rating needed.
- **Battery parallel configuration:** Two LiPo cells in parallel: same voltage (3.7 V), doubled capacity. Current splits between cells based on their internal resistances.
- **Speaker/load switching:** Multiple loads can be switched on/off independently without affecting others — the defining advantage of parallel topology. Turning off one load doesn't break the other branches' circuits.
- **Shunt resistors (current sensing):** A small shunt resistor in parallel with a load allows measuring current without significantly affecting the circuit (low resistance = small voltage drop).

## Intuition
Think of a multi-lane highway: voltage is the speed limit (same on all lanes), and each lane is a branch. More lanes → more total throughput (current). Each lane carries traffic independently. Closing one lane (opening a switch) doesn't affect the other lanes' traffic.

The key insight: **parallel reduces resistance.** This seems counterintuitive — how does adding another resistor reduce total resistance? Because you're adding another path for current to flow. Even a very high resistance branch adds a little extra current flow, so total resistance must drop. Two equal resistors in parallel give $R/2$; three give $R/3$.

**Special case — two resistors in parallel:**
$$R_{parallel} = \frac{R_1 R_2}{R_1 + R_2}$$
Memorize this. It's the most-used parallel formula.

## Derivation
**From KCL:**

At the top node of a parallel circuit, KCL states:
$$I_{total} = I_1 + I_2 + \cdots + I_n$$

Since all branches share the same voltage $V$, apply Ohm's Law to each branch: $I_k = V/R_k$:
$$I_{total} = \frac{V}{R_1} + \frac{V}{R_2} + \cdots + \frac{V}{R_n} = V\left(\frac{1}{R_1} + \frac{1}{R_2} + \cdots + \frac{1}{R_n}\right)$$

Defining total resistance by $I_{total} = V/R_{total}$:
$$\frac{1}{R_{total}} = \frac{1}{R_1} + \frac{1}{R_2} + \cdots + \frac{1}{R_n}$$

**Why voltage is the same:** KVL around any loop formed by two parallel branches shows no source — therefore: $V_1 - V_2 = 0 \implies V_1 = V_2$. They share both nodes, so they share the same potential difference.

**Conductance form:** Defining $G_k = 1/R_k$ (conductance, in Siemens S):
$$G_{total} = G_1 + G_2 + \cdots + G_n$$
Conductances add in parallel — the dual of resistances adding in series.

## Worked Example
**Problem:** Baymax's 5 V power rail supplies the following parallel loads:
- Raspberry Pi: $R_1 = 5000/800 = 6.25\,\Omega$ (800 mA at 5 V)
- IR sensor × 3: each 250 Ω (20 mA each at 5 V)
- Servo logic × 2: each 500 Ω (10 mA each at 5 V)

What is the total equivalent resistance and total current?

$\frac{1}{R_{total}} = \frac{1}{6.25} + \frac{3}{250} + \frac{2}{500}$
$= 0.16 + 0.012 + 0.004 = 0.176\,\text{S}$
$R_{total} = 1/0.176 = 5.68\,\Omega$

Total current: $I = V/R_{total} = 5/5.68 = 880\,\text{mA}$

Verify by summation: $800 + 3(20) + 2(10) = 800 + 60 + 20 = 880\,\text{mA}$ ✓

This tells you the 5 V regulator must supply at least 880 mA — specify a 1 A or 2 A regulator with margin.

## See Also
- [[Kirchhoff's Current Law]] — parallel circuits are the direct application of KCL: currents add
- [[Kirchhoff's Voltage Law]] — confirms same voltage across all parallel branches
- [[Ohm's Law]] — applied independently to each branch: $I_k = V/R_k$
- [[Current Divider]] — how total current splits between two parallel resistors
- [[Series Circuit]] — the complementary topology: same current, voltages add
- [[Resistance]] — parallel resistances combine by reciprocal summation
- [[Electric Power]] — total power = sum of powers in each parallel branch
