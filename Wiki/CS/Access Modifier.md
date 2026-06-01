# Access Modifier

**One-liner:** Access modifiers are keywords that control which other classes are allowed to read or call a field, method, or constructor.

## Core Idea
Java has four access levels, from most restrictive to least:

| Modifier | Same class | Same package | Subclass | Everywhere |
|---|---|---|---|---|
| `private` | yes | no | no | no |
| *(package-private, no keyword)* | yes | yes | no | no |
| `protected` | yes | yes | yes | no |
| `public` | yes | yes | yes | yes |

The modifier goes on the declaration:
```java
private double chargePercent;       // only this class can read/write it
protected String sensorId;          // this class + subclasses
public double getCharge() { ... }   // anyone
```

## Why It Exists
Without access control, any code in any class could set `battery.chargePercent = -999`, bypassing all validation. Access modifiers are the mechanical implementation of [[Encapsulation]]'s information-hiding principle. They turn a convention ("please don't touch this") into a compile-time guarantee ("you literally cannot").

## Real-World Applications
- Java standard library: `ArrayList.elementData` is `transient Object[]` (package-private effectively) — you can't directly access the backing array.
- Android: Activity lifecycle callbacks (`onCreate`, `onResume`) are `protected` — subclasses override them but outside code doesn't call them directly.
- In large codebases: APIs that ship as JARs use `public` only for the intended public API; everything else is `private` or package-private, letting the team refactor internals freely.
- Baymax: servo angles should be `private` with a validated setter — prevents the main loop from setting physically impossible positions.

## Intuition
Think of a submarine's compartments. `public` is the bridge — everyone can be there. `protected` is the crew quarters — only crew members (and any visiting officers = subclasses). Package-private is a specific compartment — only the team assigned to that section. `private` is the captain's safe — nobody but the captain (the class itself).

## Deep Dive
**`private` — tightest, default recommendation**
Start every field `private`. Make it less restrictive only when you have a reason. This principle of **least privilege** keeps the blast radius of bugs small.

```java
public class UltrasonicSensor {
    private double rawReading;          // internal — never exposed directly
    private final String mountPosition; // immutable after construction

    public double getDistanceCm() { return rawReading; }
    // no setter — reading is updated only by the hardware interface method
}
```

**Package-private (no modifier) — module-internal API**
Useful for helper classes that multiple classes in a package share but that shouldn't be part of the public API. Less commonly used in CS 1410, but important in real projects.

**`protected` — inheritance coupling**
`protected` fields and methods are accessible in subclasses, but this creates a coupling between parent and child: the child can see and depend on the parent's internals, making refactoring the parent risky. Prefer `protected` methods over `protected` fields.

```java
// Better: protected METHOD — child can call it but not see the field directly
public abstract class Sensor {
    private double calibrationOffset;  // private — hidden from subclasses

    protected double applyCalibration(double raw) {
        return raw + calibrationOffset;  // subclasses call this, don't touch the field
    }
}
```

**`public` — the API boundary**
`public` is a promise to the rest of the world. Changing a `public` method signature breaks all callers. Be deliberate about what you make `public`.

**Classes and access**
Top-level classes can only be `public` or package-private. Inner classes can use all four modifiers.

**Python comparison**
Python has no access modifiers — it uses naming conventions:
- `field` — public (no restriction)
- `_field` — internal by convention (please don't, but you technically can)
- `__field` — name-mangled to `_ClassName__field` (harder to access from outside)

```python
class Sensor:
    def __init__(self, id: str):
        self.id = id              # public
        self._calibration = 0.0  # "private by convention"
        self.__raw = 0.0         # name-mangled — Sensor.__raw won't work externally
```

## Worked Example
```java
// Baymax context: Sensor hierarchy with deliberate access levels
public class Sensor {
    private final String id;           // PRIVATE: immutable, only this class touches it
    private double latestReading;      // PRIVATE: always go through update() for validation

    protected static final double MAX_VALID = 1000.0;  // PROTECTED: subclasses need this constant

    public Sensor(String id) { this.id = id; }

    // PUBLIC API: what the robot's control loop calls
    public String getId() { return id; }
    public double getReading() { return latestReading; }

    // PROTECTED hook: subclasses override this for sensor-specific update logic
    protected void update(double rawValue) {
        if (rawValue > MAX_VALID) {
            System.err.println(id + ": out-of-range reading ignored");
            return;
        }
        latestReading = rawValue;
    }
}

public class TemperatureSensor extends Sensor {
    private static final double ABSOLUTE_ZERO_C = -273.15;

    public TemperatureSensor(String id) { super(id); }

    @Override
    protected void update(double rawCelsius) {
        if (rawCelsius < ABSOLUTE_ZERO_C) return;  // physics violation check
        super.update(rawCelsius);                   // can call parent's protected method
    }
}
```

## See Also
- [[Encapsulation]] — the principle that access modifiers implement
- [[Inheritance]] — `protected` members are visible to subclasses
- [[Class]] — access modifiers appear on class members
- [[Abstract Class]] — abstract methods are often `public` or `protected`
- [[Interface]] — all interface members are implicitly `public`
- [[NumPy Array]] — ndarray's internal C buffer is inaccessible from Python (effectively `private`); the public API (`array.data`, `.shape`, `.dtype`) mirrors how Java `private` fields are exposed only through controlled public methods
