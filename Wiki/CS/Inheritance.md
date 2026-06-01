# Inheritance

**One-liner:** Inheritance lets a new class (subclass) automatically receive all the fields and methods of an existing class (superclass), then extend or specialize them.

## Core Idea
The **is-a** relationship: a `UltrasonicSensor` *is a* `Sensor`. A `Dog` *is an* `Animal`. This allows code written for `Sensor` to work seamlessly with `UltrasonicSensor` objects — the basis of [[Polymorphism]].

```java
public class Sensor {           // superclass
    protected String id;
    public String getId() { return id; }
}

public class UltrasonicSensor extends Sensor {  // subclass — "extends"
    private double maxRangeM;
    public double getDistance() { ... }   // new behavior added by subclass
}
```

`UltrasonicSensor` inherits `id` and `getId()` automatically. It also adds `maxRangeM` and `getDistance()`.

## Why It Exists
Inheritance exists to avoid copy-paste. Without it, every specialized sensor class would re-implement the same id management, calibration, logging, etc. With it, shared behavior lives in one place (the superclass) and specialized behavior lives only in the subclasses that need it. When the shared behavior changes, it changes once.

**The second purpose** is enabling polymorphism: a variable of type `Sensor` can hold a `UltrasonicSensor` object, and the correct overriding method is dispatched at runtime. See [[Dynamic Dispatch]].

## Real-World Applications
- Java: `Exception` → `RuntimeException` → `IllegalArgumentException` — the whole exception hierarchy.
- Android: `View` → `ViewGroup` → `LinearLayout` — all UI widgets form an inheritance tree.
- PyTorch: your model class `extends nn.Module` — that's inheritance giving you backprop, `.parameters()`, `.to(device)`, etc. for free.
- Baymax: `Sensor` → `DistanceSensor` → `UltrasonicSensor`. The entire sensor hierarchy.

## Intuition
Inheritance is like biological taxonomy. A golden retriever IS-A dog IS-A mammal IS-A animal. The golden retriever inherits everything that's true of dogs and mammals — four limbs, warm blood — and adds its own specifics (retrieves objects, golden coat). You don't re-define "warm blood" in every species; it's inherited from `Mammal`.

## Deep Dive
**What is inherited (and what isn't)**

| Inherited | Not inherited |
|---|---|
| `public` and `protected` instance fields | `private` fields (exist but not accessible) |
| `public` and `protected` methods | constructors |
| `static` members (accessible, not "inherited" per se) | `final` classes can't be extended at all |

**Single inheritance in Java**
Java allows only one direct superclass (`extends`). This avoids the "diamond problem" (ambiguity when two parents define the same method). Multiple inheritance of *behavior* is achieved via [[Interface]] (`implements`).

**Constructor chain — `super(...)`**
Constructors are not inherited. The subclass constructor must call `super(...)` as its first statement to initialize the parent portion of the object. Java inserts `super()` implicitly if you omit it; if the superclass has no no-arg constructor, you must provide `super(args)` explicitly.

```java
public class Sensor {
    protected final String id;
    public Sensor(String id) { this.id = id; }
}

public class InfraredSensor extends Sensor {
    private double sensitivity;

    public InfraredSensor(String id, double sensitivity) {
        super(id);   // required: initializes the Sensor part
        this.sensitivity = sensitivity;
    }
}
```

**`final` — prevents extension**
Mark a class `final` to prevent subclassing. Mark a method `final` to prevent overriding. `String` is `final` — nobody can extend it and break string semantics.

**Depth and the fragile base class problem**
Deep inheritance hierarchies (5+ levels) become brittle: changing the superclass can break all subclasses in unexpected ways. This is the **fragile base class problem**. The alternative is [[Composition over Inheritance]].

**Java's implicit root: `Object`**
Every class in Java implicitly extends `java.lang.Object`. That's where `toString()`, `equals()`, `hashCode()`, and `getClass()` come from. Every class you write inherits those methods.

**Python**
```python
class Sensor:
    def __init__(self, id: str):
        self.id = id
    def get_id(self) -> str:
        return self.id

class UltrasonicSensor(Sensor):
    def __init__(self, id: str, max_range_m: float):
        super().__init__(id)   # explicit super() call
        self.max_range_m = max_range_m
    def get_distance(self) -> float: ...

# Python supports multiple inheritance:
class Robot(Movable, Senseable): ...  # Java can't do this (use interfaces instead)
```

## Worked Example
```java
// Baymax context: three-level sensor hierarchy

public abstract class Sensor {
    protected final String mountId;
    protected double lastReading = 0.0;

    public Sensor(String mountId) { this.mountId = mountId; }
    public abstract void poll();   // subclasses implement hardware read
    public double getReading()     { return lastReading; }
    public String getMountId()     { return mountId; }
}

public class RangeSensor extends Sensor {
    protected double maxRangeMeters;

    public RangeSensor(String mountId, double maxRangeMeters) {
        super(mountId);
        this.maxRangeMeters = maxRangeMeters;
    }

    public boolean isInRange() { return lastReading <= maxRangeMeters; }

    @Override
    public void poll() { /* generic range logic */ }
}

public class UltrasonicSensor extends RangeSensor {
    private static final double SPEED_OF_SOUND_M_S = 343.0;

    public UltrasonicSensor(String mountId) {
        super(mountId, 4.0);  // 4m max range for HC-SR04
    }

    @Override
    public void poll() {
        // Specific HC-SR04 timing calculation
        double travelTimeUs = readEchoPin();
        lastReading = (travelTimeUs * SPEED_OF_SOUND_M_S) / (2 * 1_000_000.0);
    }

    private double readEchoPin() { return 2300.0; /* hardware stub */ }
}
```

## See Also
- [[Class]] — inheritance is a relationship between classes
- [[Superclass]] — the parent being extended
- [[Subclass]] — the child doing the extending
- [[Method Overriding]] — how subclasses redefine inherited behavior
- [[Polymorphism]] — the power that inheritance enables
- [[Abstract Class]] — a superclass with some behavior deliberately left abstract
- [[Interface]] — Java's way to achieve multiple-inheritance-of-type
- [[Composition over Inheritance]] — when NOT to use inheritance
- [[What is Reinforcement Learning]] — RL agent classes often form an inheritance hierarchy (BaseAgent → PolicyGradientAgent → PPOAgent)
