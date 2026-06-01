# Method Overriding

**One-liner:** Method overriding lets a subclass replace a superclass method's implementation with its own version, chosen at runtime based on the object's actual type.

## Core Idea
When a subclass declares a method with the exact same signature (name + parameter types) as an inherited method, it **overrides** that method. At runtime, calling the method on a superclass-typed reference will invoke the subclass version if the actual object is a subclass instance — not the superclass version. This runtime selection is called [[Dynamic Dispatch]].

```java
public class Sensor {
    public String describe() {
        return "Generic sensor at " + mountId;
    }
}

public class UltrasonicSensor extends Sensor {
    @Override
    public String describe() {   // overrides Sensor.describe()
        return "Ultrasonic sensor (max " + maxRangeM + "m) at " + mountId;
    }
}

Sensor s = new UltrasonicSensor("front", 4.0);
s.describe();  // calls UltrasonicSensor.describe() — runtime dispatch
```

## Why It Exists
Without overriding, subclasses could only add new methods — they couldn't change inherited behavior. All `Sensor` objects would produce generic descriptions, all `Shape` objects would have the same `area()` formula. Overriding lets each subclass provide the *right* implementation for its specific type, while callers remain blissfully unaware of which subclass they hold. This is the core mechanism of [[Polymorphism]].

## Real-World Applications
- `toString()` — every class inherits `Object.toString()`, which returns `ClassName@hashcode`. Every meaningful class overrides it.
- `equals()` and `hashCode()` — must override together whenever you need value equality.
- Android: `Activity.onCreate()` is overridden in every Activity to set up the UI.
- PyTorch: `nn.Module.forward()` must be overridden in every custom network layer.
- Baymax: `Sensor.poll()` overridden by each sensor type to do hardware-specific reading.

## Intuition
A company has a `handleComplaint()` policy. Customer service reps follow the standard policy. But VIP account managers override it with a premium version — same method name in the caller's script, different behavior at runtime depending on which rep object they're calling. The caller doesn't need to check which type of rep they have; the right behavior happens automatically.

## Deep Dive
**`@Override` annotation**
Always use `@Override`. It tells the compiler to verify that the method actually overrides something. Without it, a typo in the method name silently creates a new method instead of overriding, and you'll have a baffling runtime bug.

```java
@Override           // compiler error if this doesn't actually override anything
public void poll() {
    // ...
}
```

**Signature must match exactly**
Override requires exact match of:
- Method name
- Parameter types (in order)
- Return type: must be the same OR a subtype (covariant return types — allowed since Java 5)

```java
// Superclass
public Sensor clone() { ... }

// Valid covariant override — returns a more specific type
@Override
public UltrasonicSensor clone() { ... }
```

**Access cannot be more restrictive**
You can make an overriding method more permissive (e.g., `protected` → `public`), but not more restrictive (`public` → `private`). Reason: callers using the superclass type expect to be able to call `public` methods.

**`final` prevents overriding**
A method marked `final` cannot be overridden in any subclass. Use sparingly — it locks in the implementation.

**Calling the super version**
`super.method()` explicitly calls the overridden parent version. Use when you want to extend parent behavior rather than completely replace it.

```java
@Override
public void poll() {
    super.poll();                  // do whatever parent did
    applyTemperatureCompensation(); // then add our specific adjustment
}
```

**`static` methods are NOT overridden**
Static methods are resolved at compile time based on the reference type, not the object type. This is called **hiding**, not overriding. Don't confuse the two.

```java
Sensor s = new UltrasonicSensor("front", 4.0);
s.poll();               // dynamic dispatch: calls UltrasonicSensor.poll() ✓
Sensor.staticHelper();  // compile-time: calls Sensor.staticHelper() always
```

**Python**
Python has no `@Override` equivalent (though it's being added in Python 3.12+ via type checkers). You just redefine the method in the subclass.

```python
class Sensor:
    def describe(self) -> str:
        return f"Generic sensor at {self.mount_id}"

class UltrasonicSensor(Sensor):
    def describe(self) -> str:  # overrides — same name, Python resolves via MRO
        return f"Ultrasonic (max {self.max_range_m}m) at {self.mount_id}"

    def poll(self):
        super().poll()            # call parent version explicitly
        self._apply_compensation()
```

Python uses the **Method Resolution Order (MRO)** to find the right method, searching the class hierarchy left to right, depth first (C3 linearization). `type(obj).__mro__` shows the MRO for debugging.

## Worked Example
```java
// Baymax context: sensors report their status differently

public abstract class Sensor {
    protected final String mountId;
    protected double lastReading;

    public Sensor(String mountId) { this.mountId = mountId; }

    // Default status report — subclasses override for richer output
    public String statusReport() {
        return String.format("[%s] reading=%.2f", mountId, lastReading);
    }
}

public class UltrasonicSensor extends Sensor {
    private double maxRangeM;

    public UltrasonicSensor(String mountId, double maxRangeM) {
        super(mountId);
        this.maxRangeM = maxRangeM;
    }

    @Override
    public String statusReport() {
        String base = super.statusReport();  // reuse parent's formatting
        boolean inRange = lastReading <= maxRangeM;
        return base + String.format(" maxRange=%.1fm %s",
            maxRangeM, inRange ? "OK" : "OUT_OF_RANGE");
    }
}

public class IMUSensor extends Sensor {
    private double roll, pitch;

    public IMUSensor(String mountId) { super(mountId); }

    @Override
    public String statusReport() {
        return String.format("[%s] roll=%.1f° pitch=%.1f°", mountId, roll, pitch);
        // completely replaces parent format — more appropriate for IMU
    }
}

// The power: a list of Sensor references, each printing the RIGHT report
List<Sensor> sensors = List.of(
    new UltrasonicSensor("front", 4.0),
    new IMUSensor("body"),
    new UltrasonicSensor("rear", 2.0)
);

for (Sensor s : sensors) {
    System.out.println(s.statusReport());  // dynamic dispatch — correct version every time
}
```

## See Also
- [[Dynamic Dispatch]] — the runtime mechanism that makes overriding work
- [[Polymorphism]] — overriding is the primary mechanism of polymorphism
- [[Inheritance]] — you can only override methods you've inherited
- [[Superclass]] — the class whose methods are being overridden
- [[Subclass]] — the class that provides the overriding implementation
- [[Abstract Class]] — abstract methods are a "forced override" contract
- [[Interface]] — interface methods must also be overridden in implementing classes
- [[Forward Pass]] — overriding `forward()` in a PyTorch `nn.Module` subclass is the canonical real-world method override: every layer and model replaces the parent's placeholder with its own computation
- [[Activation Function]] — each activation function type (ReLU, sigmoid, tanh) overrides the same base `forward()` method with a different nonlinearity — method overriding driving swappable behavior
