# CS Index

Navigation hub for all CS atomic notes. One concept per file. Use the clusters below to find what you need, then follow the suggested study order for CS 1410.

---

## Clusters

### OOP Fundamentals
The building blocks — understand these first before everything else.

| Note | What it covers |
|---|---|
| [[Class]] | Blueprint: defines fields and methods; no heap memory until instantiated |
| [[Object]] | A live instance on the heap, created from a class with `new` |
| [[Instance]] | Synonym for object; emphasizes the relationship back to its class |
| [[Constructor]] | Special method that runs at `new`; sets up initial state and invariants |
| [[Encapsulation]] | Bundle data + behavior in a class; hide fields behind a controlled public API |
| [[Access Modifier]] | `private`, `protected`, `public`, package-private — tools that enforce encapsulation |

### Inheritance
Building new types on top of existing ones.

| Note | What it covers |
|---|---|
| [[Inheritance]] | `extends` keyword; subclass inherits all public/protected fields and methods |
| [[Superclass]] | The parent being extended; provides shared behavior |
| [[Subclass]] | The child that extends; adds or overrides behavior |
| [[Method Overriding]] | Subclass replaces an inherited method's body; `@Override` enforces correctness |

### Polymorphism
One interface, many behaviors — the payoff of the inheritance cluster.

| Note | What it covers |
|---|---|
| [[Polymorphism]] | Same method call produces different behavior depending on actual object type |
| [[Dynamic Dispatch]] | The JVM's runtime vtable lookup that selects the correct overriding method |
| [[Upcasting and Downcasting]] | Moving between declared type (supertype) and actual type (subtype); `instanceof` safety |

### Abstraction
Designing contracts and hiding implementation details at the architectural level.

| Note | What it covers |
|---|---|
| [[Abstract Class]] | Cannot instantiate; mandates abstract methods; shares concrete fields and behavior |
| [[Interface]] | Pure capability contract; `implements`; enables multiple inheritance of type |
| [[Composition over Inheritance]] | Has-a over is-a; strategy pattern; dependency injection; fragile base class fix |

---

## Suggested Study Order for CS 1410

This is the order SLCC CS 1410 typically introduces these concepts; studying them in sequence minimizes "I don't know what that word means yet" moments.

```
Week 1–2   Class → Object → Instance → Constructor
Week 3–4   Encapsulation → Access Modifier
Week 5–6   Inheritance → Superclass → Subclass
Week 7     Method Overriding → Polymorphism
Week 8–9   Dynamic Dispatch → Upcasting and Downcasting
Week 10–11 Abstract Class → Interface
Week 12+   Composition over Inheritance
```

**Key dependency graph (simplified):**
```
Class ──► Object ──► Instance
 │
 ├──► Encapsulation ──► Access Modifier
 │
 └──► Inheritance ──► Superclass / Subclass
            │
            ├──► Method Overriding ──► Polymorphism ──► Dynamic Dispatch
            │
            └──► Abstract Class ──┐
                                  ├──► Interface
                                  └──► Upcasting and Downcasting
                                              │
                                              └──► Composition over Inheritance
```

---

## Baymax Cross-Reference Table

How each CS concept maps to something real in the Baymax robotics project (Python) and CS 1410 (Java).

| CS Concept | Baymax / Robotics Application | ML & Robotics Wiki Link |
|---|---|---|
| [[Class]] | `UltrasonicSensor`, `DriveMotor`, `BatteryMonitor` each modeled as a class | — |
| [[Object]] | Each physical sensor is a live object: `front = UltrasonicSensor("front")` | — |
| [[Instance]] | `front` and `rear` are two instances of the same `UltrasonicSensor` class | — |
| [[Constructor]] | `UltrasonicSensor("front", 4.0)` sets `id` and `maxRangeM` at creation | — |
| [[Encapsulation]] | `PIDController` hides `integral` and `lastError`; exposes only `update()` and `reset()` | [[Gradient Descent]] — PID and gradient descent both update internal state through a controlled loop |
| [[Access Modifier]] | `private double distanceM` on sensor; `public void poll()` for the control loop | — |
| [[Inheritance]] | `Sensor → RangeSensor → UltrasonicSensor` three-level hierarchy | [[Gradient Descent]] — PyTorch models inherit `nn.Module` to get backprop for free |
| [[Superclass]] | `Sensor` provides `id`, `getReading()`, `isStale()` to all sensor types | — |
| [[Subclass]] | `UltrasonicSensor` adds `maxRangeM`, `isInRange()`, HC-SR04 specific `poll()` | — |
| [[Method Overriding]] | Each sensor type overrides `poll()` to read its own hardware register | [[Gradient Descent]] — each `nn.Module` subclass overrides `forward()` |
| [[Polymorphism]] | `List<Sensor>` drives all sensor types with `sensor.poll()` — no if-chains | [[Vectorization]] — uniform API over heterogeneous data mirrors polymorphism |
| [[Dynamic Dispatch]] | JVM vtable routes `sensor.poll()` to `UltrasonicSensor.poll()` at runtime | — |
| [[Upcasting and Downcasting]] | Sensors stored as `Sensor`; downcast to `Calibratable` for calibration loop | — |
| [[Abstract Class]] | `abstract class Sensor` forces all subtypes to implement `poll()` and `unitLabel()` | [[Gradient Descent]] — `nn.Module` is Python's abstract class for all neural network layers |
| [[Interface]] | `Pollable`, `Calibratable`, `Diagnostics` — opt-in capability contracts | [[Loss Function]] — loss functions in PyTorch satisfy a common callable interface; swap without changing training loop |
| [[Composition over Inheritance]] | `BaymaxRobot` HAS-A `DrivePolicy`, HAS-A `SensorFusion` — swap policies without changing robot | [[Gradient Descent]] — optimizer (Adam, SGD) composed into trainer; [[Loss Function]] — loss composed into trainer |

---

## Quick Cheat Sheet — Common Confusions

**Abstract class vs. Interface**
- Need shared fields or constructors? → Abstract class
- Need multiple "parent types" or unrelated classes with the same capability? → Interface
- Need both? → Abstract class `extends` another class AND `implements` multiple interfaces

**Overriding vs. Overloading**
- Same method name, same parameters, different class (subclass) → **Overriding** (runtime, polymorphism)
- Same method name, different parameters, same class → **Overloading** (compile-time, not polymorphism)

**Upcasting vs. Downcasting**
- Subtype → Supertype: upcast, implicit, always safe
- Supertype → Subtype: downcast, explicit cast `(SubType)`, needs `instanceof` check first
- Java 16+: use `instanceof SubType name` to check and bind in one expression

**Composition vs. Inheritance**
- IS-A relationship, stable hierarchy, want inherited behavior → Inheritance
- HAS-A relationship, want swappable parts, need testability → Composition
- When in doubt → Composition (especially if you find yourself downcasting often)

---

## All CS Notes (flat list for search)

[[Class]] · [[Object]] · [[Instance]] · [[Constructor]] · [[Encapsulation]] · [[Access Modifier]] · [[Inheritance]] · [[Superclass]] · [[Subclass]] · [[Method Overriding]] · [[Polymorphism]] · [[Dynamic Dispatch]] · [[Upcasting and Downcasting]] · [[Abstract Class]] · [[Interface]] · [[Composition over Inheritance]]
