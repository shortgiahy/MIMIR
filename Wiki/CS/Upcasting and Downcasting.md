# Upcasting and Downcasting

**One-liner:** Upcasting treats a subclass object as its superclass type (always safe, done implicitly); downcasting goes the other direction and requires an explicit cast plus a runtime check or you risk a `ClassCastException`.

## Core Idea
Every subclass object simultaneously *is* its subclass type and *is* its superclass type. Casting is not converting — it is choosing which "lens" you look at the same object through.

```java
// Upcasting — implicit, always safe
UltrasonicSensor us = new UltrasonicSensor("front", 4.0);
Sensor s = us;           // upcast: UltrasonicSensor → Sensor (no cast syntax needed)

// Downcasting — explicit, requires care
Sensor s2 = new UltrasonicSensor("rear", 2.0);
UltrasonicSensor us2 = (UltrasonicSensor) s2;   // downcast: Sensor → UltrasonicSensor
double range = us2.getMaxRange();                // now you can call subclass-specific methods
```

## Why It Exists
[[Polymorphism]] requires upcasting: you store heterogeneous objects in a `List<Sensor>` by upcasting each one. Later, if you need capabilities specific to a subtype (e.g., `getMaxRange()` only exists on `UltrasonicSensor`), you downcast — but only after verifying the actual type with `instanceof`. The design tension is: the more you downcast, the more your code depends on concrete subtypes, which partially undoes the benefits of polymorphism.

## Real-World Applications
- Java collections: `List<Sensor>` upcasts everything going in; you downcast when retrieving if you need subtype-specific behavior.
- Android: `findViewById(R.id.button)` returns `View` — you downcast to `Button` to call `setOnClickListener`.
- PyTorch / scikit-learn: Python is dynamically typed, so upcasting/downcasting don't apply directly — but the same logic shows up when you retrieve an object from a heterogeneous list and call type-specific methods after a type check.
- Baymax: A `List<Actuatable>` holds motors and servos; if you need servo-specific `setAngle()`, you downcast after an `instanceof` check.

## Intuition
A Swiss Army knife is a knife, a screwdriver, a can opener, and scissors simultaneously. When you hand it to someone saying "here's a knife," you've upcast it — they see only knife behavior. If they later need to use it as scissors, they check "is this actually a Swiss Army knife?" and then use the scissors blade. The object never changed — only the perspective on it.

## Deep Dive
**Upcasting in detail**
Upcasting is always implicit and always safe because the Liskov Substitution Principle guarantees that a subclass object fulfills every contract of the superclass. The compiler allows it without question.

```java
// These are all valid upcasts — Java does them automatically
UltrasonicSensor us  = new UltrasonicSensor("front", 4.0);
Sensor       s       = us;         // explicit syntax optional
Object       o       = us;         // every class upcasts to Object
Serializable serial  = us;         // upcast to interface (if UltrasonicSensor implements it)
```

After upcasting, you lose access to subclass-specific methods through that reference. The compiler enforces this — `s.getMaxRange()` does not compile because `Sensor` has no `getMaxRange()`.

**Downcasting in detail**
Downcasting asks the JVM to re-expose the full subclass interface. At runtime the JVM checks the actual type stored in the object header. If it's not compatible, `ClassCastException` is thrown.

```java
Sensor s = new IMUSensor("body");

// This compiles but throws ClassCastException at runtime:
UltrasonicSensor bad = (UltrasonicSensor) s;  // ← BOOM: IMUSensor is not UltrasonicSensor

// Safe pattern — always check before casting
if (s instanceof UltrasonicSensor) {
    UltrasonicSensor us = (UltrasonicSensor) s;
    double r = us.getMaxRange();
}
```

**Java 16+ pattern matching for `instanceof`**
Java 16 introduced pattern matching that combines the `instanceof` check and the cast into one expression, eliminating the redundant cast:

```java
// Old (Java ≤15): check then cast
if (s instanceof UltrasonicSensor) {
    UltrasonicSensor us = (UltrasonicSensor) s;  // cast repeated
    System.out.println(us.getMaxRange());
}

// New (Java 16+): pattern matching — check AND bind in one step
if (s instanceof UltrasonicSensor us) {
    System.out.println(us.getMaxRange());  // us is already cast and in scope
}

// Java 21: switch pattern matching
String report = switch (s) {
    case UltrasonicSensor us -> "US range: " + us.getMaxRange();
    case IMUSensor imu       -> "IMU active";
    default                  -> "Unknown sensor";
};
```

**ClassCastException — common causes**
1. Assuming `instanceof` check is not needed ("I put an `UltrasonicSensor` in this list, I'm sure").
2. Forgetting that generics are erased at runtime — a `List<UltrasonicSensor>` cast to `List<Sensor>` will not throw, but adding an `IMUSensor` to the original `List<Sensor>` reference will produce heap pollution.

**When downcasting is a design smell**
Heavy downcasting is often a sign that the abstraction hierarchy is wrong. If you constantly check "is this an `UltrasonicSensor`?" and cast, consider: should `getMaxRange()` be on the `Sensor` interface with a sensible default? Should the method needing this information receive `UltrasonicSensor` directly? See [[Composition over Inheritance]] and [[Abstract Class]] for alternatives.

**Python — `isinstance` and type narrowing**
Python has no static type hierarchy enforcement at runtime, but the same logic applies:

```python
sensor: Sensor = UltrasonicSensor("front", 4.0)

# "Upcast" — works naturally in Python, just use the object
sensors: list[Sensor] = [UltrasonicSensor("front", 4.0), IMUSensor("body")]

# "Downcast" — isinstance check before accessing subtype attributes
for s in sensors:
    if isinstance(s, UltrasonicSensor):
        print(s.get_max_range())   # type checker narrows type inside this block
```

With type checkers (mypy, pyright), `isinstance` inside an `if` block causes the type to be narrowed automatically — the same guarantee Java 16+ pattern matching provides.

## Worked Example
```java
// Baymax context: sensor manager with mixed types; needs to calibrate only range sensors

public abstract class Sensor {
    protected final String id;
    public Sensor(String id) { this.id = id; }
    public abstract void poll();
}

public interface Calibratable {
    void calibrate();
}

public class UltrasonicSensor extends Sensor implements Calibratable {
    private double maxRangeM;
    public UltrasonicSensor(String id, double maxRangeM) {
        super(id);
        this.maxRangeM = maxRangeM;
    }
    @Override public void poll() { /* hardware read */ }
    @Override public void calibrate() {
        System.out.println("Calibrating ultrasonic " + id);
        // send calibration pulse...
    }
    public double getMaxRange() { return maxRangeM; }
}

public class IMUSensor extends Sensor {
    public IMUSensor(String id) { super(id); }
    @Override public void poll() { /* hardware read */ }
    // Does NOT implement Calibratable — no calibrate() method
}

// Mixed list — everything upcast to Sensor
List<Sensor> sensors = List.of(
    new UltrasonicSensor("front", 4.0),
    new IMUSensor("body"),
    new UltrasonicSensor("rear", 2.0)
);

// Normal use — no downcast needed (polymorphism handles it)
for (Sensor s : sensors) {
    s.poll();
}

// Special case — calibrate only those that support it
// Java 16+ pattern matching makes this clean
for (Sensor s : sensors) {
    if (s instanceof Calibratable c) {   // pattern matching: check + bind in one line
        c.calibrate();
    }
}
// Output:
// Calibrating ultrasonic front
// Calibrating ultrasonic rear
```

The `IMUSensor` is silently skipped because it doesn't match `Calibratable`. No `ClassCastException` risk.

## See Also
- [[Dynamic Dispatch]] — once you've upcast, dispatch selects the right method at runtime
- [[Polymorphism]] — upcasting is what makes polymorphism possible
- [[Inheritance]] — the is-a relationship that makes upcasting safe
- [[Interface]] — you can upcast to an interface type, not just a superclass
- [[Abstract Class]] — abstract class references are the most common upcast target
- [[Composition over Inheritance]] — heavy downcasting is often a signal to prefer composition
- [[Policy]] — storing different policy implementations behind a common `Policy` reference is upcasting; retrieving a specific policy type to call its implementation-specific tuning methods requires a downcast with an `instanceof` check
- [[State]] — in RL, a concrete state (e.g., `ImageState`, `VectorState`) is often upcast to a generic `State` type for the agent loop; downcasting is needed when a specific algorithm requires access to subtype-specific structure
