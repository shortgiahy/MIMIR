# CS Knowledge Base — Index

> Computer Science notes for CS 1410 (OOP, Java, SLCC) and the Baymax robotics project (Python). Entries are written to explain the *why*, not just the syntax. Cross-links to [[NumPy Arrays]], [[What is Reinforcement Learning]], and other robotics/ML notes are marked where CS concepts show up in real project work.

---

## OOP Fundamentals

The core building blocks of object-oriented design. Start here.

| Entry | What it covers |
|---|---|
| [[Classes and Objects]] | What a class is vs an object, the blueprint/instance distinction, memory model, `static` vs instance members |
| [[Encapsulation]] | Why hiding state matters, `private`/`protected`/`public`, getters and setters done right vs done lazily |
| [[Inheritance]] | The is-a relationship, `extends`, method overriding, when inheritance is wrong and composition is right |
| [[Polymorphism]] | One interface, many implementations; runtime dispatch vs compile-time overloading; the Liskov Substitution Principle |
| [[Interfaces and Abstract Classes]] | The difference between the two, when to use each, "program to the interface" principle |

**Recommended reading order:** Classes and Objects → Encapsulation → Inheritance → Polymorphism → Interfaces and Abstract Classes. Each entry builds on the previous.

---

## Design Principles

*(Entries to be added)*

These entries will cover principles that govern how OOP should be used — not just what the constructs are, but how to deploy them correctly at scale.

- **SOLID Principles** — Single Responsibility, Open/Closed, Liskov Substitution, Interface Segregation, Dependency Inversion. The five rules behind professional Java design. LSP is previewed in [[Polymorphism]].
- **Composition Over Inheritance** — the design pattern that avoids the fragile base class problem flagged in [[Inheritance]]. Covered conceptually in the Gotchas section there; deserves its own full entry.
- **Design Patterns** — Factory, Strategy, Observer, and others. Each pattern is a named solution to a recurring design problem. Strategy pattern is especially relevant for Baymax (swappable behaviors).

---

## Data Structures

*(Entries to be added)*

| Planned Entry | Why it matters |
|---|---|
| Arrays and ArrayLists | Java's `ArrayList` is the standard dynamic array; used constantly in CS 1410 |
| LinkedLists | When `LinkedList` beats `ArrayList` and why — O(1) insert vs O(n) |
| Stacks and Queues | Used in pathfinding algorithms relevant to Baymax navigation |
| HashMaps | Key-value lookup in O(1); used everywhere in both Java and Python |
| Trees and BSTs | Hierarchical data; prerequisite for understanding search algorithms |

---

## Algorithms

*(Entries to be added)*

| Planned Entry | Why it matters |
|---|---|
| Sorting (Merge, Quick) | Foundational; required for CS 1410 and CS fundamentals |
| Binary Search | O(log n) search; shows up in robotics sensor filtering |
| Recursion | Needed for trees; also foundational for dynamic programming |
| Big-O Notation | How to reason about algorithmic efficiency — prerequisite for all algorithm entries |
| Graph Search (BFS, DFS) | Core to robot navigation and path planning in Baymax |

---

## CS Concepts in the Baymax Project

The Baymax robotics project (Phase 1: pure Python) is where abstract CS concepts become real engineering. Here's where each CS concept shows up directly:

| CS Concept | Where it appears in Baymax |
|---|---|
| [[Classes and Objects]] | Every component — `Sensor`, `Motor`, `Controller` — is a class. The entire software architecture is OOP. |
| [[Encapsulation]] | Motor speed limits, sensor validation, safety interlocks. Direct code in [[Encapsulation]] Worked Example. |
| [[Inheritance]] | Sensor hierarchy: `Sensor` → `UltrasonicSensor`, `TemperatureSensor`. Direct code in [[Inheritance]] Worked Example. |
| [[Polymorphism]] | The main control loop calls `actuator.execute()` without knowing actuator type. Enables clean, extensible robot control. |
| [[Interfaces and Abstract Classes]] | `Diagnosable` interface applies to both sensors and motors — unrelated types, shared capability. Full design in [[Interfaces and Abstract Classes]] Worked Example. |

---

## CS Concepts in ML / Reinforcement Learning

| CS Concept | Where it appears |
|---|---|
| [[Classes and Objects]] | NumPy's `ndarray`, PyTorch's `Tensor`, and every ML library class are instances of this concept. See [[NumPy Arrays]]. |
| [[Inheritance]] | Neural network layers in PyTorch are subclasses of `nn.Module`. The framework uses inheritance to let you define custom layers. |
| [[Polymorphism]] | RL environments implement the same interface (`reset()`, `step()`, `render()`). See [[What is Reinforcement Learning]]. |
| [[Interfaces and Abstract Classes]] | OpenAI Gym / Gymnasium uses abstract base classes to define the environment contract every RL environment must implement. |

---

## Cross-Wiki Links

| Domain | Related Entry | CS Connection |
|---|---|---|
| ML & Robotics | [[NumPy Arrays]] | Arrays and array operations are the data structure underlying all ML; CS Arrays entry is a prerequisite |
| ML & Robotics | [[What is Reinforcement Learning]] | RL agents are implemented as classes; environments use polymorphism via standard interfaces |
| ML & Robotics | [[Gradient Descent]] | Implemented as objects and methods in PyTorch; understanding classes makes ML code readable |
