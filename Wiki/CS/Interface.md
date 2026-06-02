# Interface

**One-liner:** An interface is a pure capability contract — a named set of method signatures a class promises to fulfill — enabling unrelated types to be used interchangeably without sharing an inheritance hierarchy.

## Core Idea
An interface defines *what* a class can do, not *what it is*. A class that `implements` an interface commits to providing every method the interface declares. Multiple unrelated classes can implement the same interface; the code that uses them needs only to know about the interface.

```java
public interface Pollable {
    void poll();              // abstract by default — implementors must provide this
    String statusReport();    // abstract by default
}

public class UltrasonicSensor implements Pollable {
    @Override public void poll() { /* hardware read */ }
    @Override public String statusReport() { return "US: 1.5m"; }
}

public class NetworkFeed implements Pollable {
    @Override public void poll() { /* HTTP request */ }
    @Override public String statusReport() { return "Feed: 200 OK"; }
}

// Both are Pollable — code that uses Pollable doesn't care which concrete type it holds
List<Pollable> sources = List.of(new UltrasonicSensor("front"), new NetworkFeed("api"));
sources.forEach(Pollable::poll);
```

`UltrasonicSensor` and `NetworkFeed` have nothing in common except the capability contract. No shared ancestor needed.

## Why It Exists
[[Inheritance]] only works between related types. If you want to write code that works on both a `Sensor` and a `DatabaseLogger` uniformly, you would need a common superclass — but that ancestry might not make logical sense ("a logger IS-A sensor" is absurd). Interfaces solve this by separating the *capability* from the *type hierarchy*. Any class, from any hierarchy, can implement `Pollable`.

The second problem interfaces solve is **decoupling**. A component that depends on `Pollable` does not depend on any specific implementation. You can swap `UltrasonicSensor` for a `MockSensor` in tests, swap a real `DataStore` for an in-memory stub — without changing the component's code at all.

## Real-World Applications
- Java standard library: `Comparable<T>`, `Iterable<T>`, `Runnable`, `Callable`, `AutoCloseable` — every one is an interface that unrelated classes implement.
- Android: `View.OnClickListener` — any object can be a click listener by implementing `onClick(View v)`.
- scikit-learn: Python's duck-typed equivalent — every estimator implements `fit()` and `predict()`. This is the interface pattern without formal enforcement.
- Spring / dependency injection: components depend on interfaces; the framework wires concrete implementations at startup. Swapping implementations requires zero code changes in the components.
- Baymax: `Actuatable`, `Pollable`, `Calibratable`, `Diagnostics` — capability interfaces let the robot's control loop drive heterogeneous hardware without knowing the concrete types.

## Intuition
An interface is a professional certification. A "Licensed Electrician" certification says "this person can wire a circuit" without specifying anything about their age, education history, or employer. You can hire any certified electrician and know the capability is there. Two electricians from completely different backgrounds are interchangeable for your wiring job because they both hold the same certification.

Similarly, `implements Runnable` is a certification that says "this object can be run in a thread" — Java's thread system accepts any `Runnable` regardless of what the class is otherwise.

## Deep Dive
**Interface syntax rules in Java**
```java
public interface Calibratable {
    // Method signatures — implicitly public and abstract
    void calibrate();
    boolean isCalibrated();

    // Constant — implicitly public static final
    int MAX_CALIBRATION_ATTEMPTS = 3;   // = public static final int

    // Default method (Java 8+) — has a body; implementors inherit it but can override
    default String calibrationStatus() {
        return isCalibrated() ? "CALIBRATED" : "UNCALIBRATED";
    }

    // Static method (Java 8+) — utility method on the interface itself; not inherited
    static Calibratable noOp() {
        return new Calibratable() {
            public void calibrate()      { /* nothing */ }
            public boolean isCalibrated() { return true; }
        };
    }

    // Private method (Java 9+) — helper for default methods; not visible outside
    private void logAttempt(int n) {
        System.out.println("Calibration attempt " + n);
    }
}
```

**Multiple `implements`**
A class can implement as many interfaces as it needs, inheriting all their type contracts. Java allows only one `extends` (one superclass) but unlimited `implements`.

```java
public class UltrasonicSensor extends Sensor
        implements Calibratable, Diagnostics, Serializable {
    // Must implement: calibrate(), isCalibrated(), selfTest(), getHealthReport()
    // Sensor takes care of: poll(), statusReport()
}
```

**Interface as the primary tool for decoupling**
The canonical pattern: define an interface, write production code against the interface, inject the concrete implementation externally.

```java
// The interface — the only thing the control loop knows about
public interface DrivePolicy {
    double[] computeWheelSpeeds(RobotState state, Goal goal);
}

// Two implementations — nothing else about them needs to match
public class PIDDrivePolicy implements DrivePolicy { ... }
public class NeuralNetDrivePolicy implements DrivePolicy { ... }

// Control loop — depends ONLY on the interface
public class DriveController {
    private final DrivePolicy policy;   // could be PID or NN — controller doesn't care

    public DriveController(DrivePolicy policy) {
        this.policy = policy;           // dependency injection
    }

    public void update(RobotState state, Goal goal) {
        double[] speeds = policy.computeWheelSpeeds(state, goal);
        applyWheelSpeeds(speeds);
    }
}

// At startup — wire up the concrete implementation:
DriveController ctrl = new DriveController(new NeuralNetDrivePolicy(modelWeights));
// In tests:
DriveController testCtrl = new DriveController(new MockDrivePolicy());
```

**Default methods — backward compatibility**
Default methods (Java 8+) exist for one reason: adding methods to a published interface without breaking all existing implementors. When Java added `stream()` and `forEach()` to `Collection`, it needed to provide default implementations or every custom `Collection` subclass in the world would fail to compile.

Use default methods for backward compatibility or genuine "optional/overrideable" behavior. Do NOT use them to sneak shared state into interfaces — that breaks the interface's purpose.

**`interface` vs. `abstract class` — quick recap**

| | Interface | Abstract Class |
|---|---|---|
| State (instance fields) | No | Yes |
| Constructors | No | Yes |
| Multiple inheritance | Yes | No |
| Models | capability / role | family / kind |
| Java keyword | `implements` | `extends` |

**Python — Protocol (structural subtyping)**
Python 3.8+ introduced `typing.Protocol` as the formal equivalent of a Java interface. Any class that has the required methods automatically satisfies the protocol — no explicit `implements` declaration needed. This is "structural subtyping" vs. Java's "nominal subtyping."

```python
from typing import Protocol

class Pollable(Protocol):
    def poll(self) -> None: ...
    def status_report(self) -> str: ...

# These satisfy Pollable without declaring it:
class UltrasonicSensor:
    def poll(self) -> None: pass
    def status_report(self) -> str: return "US: 1.5m"

class MockSensor:
    def poll(self) -> None: pass
    def status_report(self) -> str: return "Mock: OK"

def run_all(sources: list[Pollable]) -> None:
    for s in sources:
        s.poll()
        print(s.status_report())

run_all([UltrasonicSensor(), MockSensor()])  # type checker is happy
```

For abstract base classes with explicit registration, Python also has `abc.ABC` + `@abstractmethod` (the nominal approach, closer to Java interfaces/abstract classes).

**Functional interfaces and lambdas (Java 8+)**
Any interface with exactly one abstract method is a **functional interface**. Java lets you implement it with a lambda expression, making interfaces the foundation of Java's functional programming features.

```java
@FunctionalInterface
public interface SensorFilter {
    boolean test(double reading);
}

// Implement with a lambda — no need for a full class
SensorFilter closeRange = reading -> reading < 0.5;
SensorFilter tooFar     = reading -> reading > 3.0;

sensors.stream()
       .filter(s -> closeRange.test(s.getLastReading()))
       .forEach(s -> triggerAvoidance(s));
```

## Worked Example
```java
// Baymax context: capability-based design for heterogeneous hardware

// Interfaces — capability contracts
public interface Pollable    { void poll(); }
public interface Calibratable { void calibrate(); boolean isCalibrated(); }
public interface Diagnostics  { Map<String, String> healthReport(); }

// Abstract base for sensors (shared state and identity)
public abstract class Sensor implements Pollable {
    protected final String id;
    public Sensor(String id) { this.id = id; }
    public String getId() { return id; }
}

// UltrasonicSensor has sensor identity + calibration capability
public class UltrasonicSensor extends Sensor implements Calibratable, Diagnostics {
    private boolean calibrated = false;

    public UltrasonicSensor(String id) { super(id); }

    @Override public void poll()        { /* hardware read */ }
    @Override public void calibrate()   { calibrated = true; }
    @Override public boolean isCalibrated() { return calibrated; }
    @Override public Map<String, String> healthReport() {
        return Map.of("calibrated", String.valueOf(calibrated));
    }
}

// IMUSensor — sensor, but not calibratable via this interface
public class IMUSensor extends Sensor {
    public IMUSensor(String id) { super(id); }
    @Override public void poll() { /* IMU register reads */ }
}

// System startup: calibrate everything that supports it
List<Sensor> allSensors = List.of(
    new UltrasonicSensor("front"),
    new IMUSensor("body"),
    new UltrasonicSensor("rear")
);

// Calibrate only sensors that implement Calibratable (Java 16+ pattern matching)
for (Sensor s : allSensors) {
    if (s instanceof Calibratable c) {
        c.calibrate();
        System.out.println(s.getId() + " calibrated: " + c.isCalibrated());
    }
}

// Normal poll loop — uses Pollable contract
allSensors.forEach(Pollable::poll);

// Diagnostics report — only those that implement Diagnostics
for (Sensor s : allSensors) {
    if (s instanceof Diagnostics d) {
        System.out.println(s.getId() + " health: " + d.healthReport());
    }
}
```

`IMUSensor` participates in the poll loop but is silently skipped in calibration and diagnostics — no `instanceof` abuse, no stub methods. Each interface is opt-in.

## See Also
- [[Abstract Class]] — the companion concept; use abstract class for shared state, interface for capability contract
- [[Polymorphism]] — interfaces are the primary polymorphism mechanism for unrelated types
- [[Dynamic Dispatch]] — interface method calls are dispatched dynamically at runtime
- [[Upcasting and Downcasting]] — variables of interface type hold concrete objects; downcasting accesses extra methods
- [[Composition over Inheritance]] — interfaces make composition practical by defining the seams
- [[Inheritance]] — interfaces provide multiple inheritance of *type* without the diamond problem
- [[Gradient Descent]] — PyTorch's `nn.Module` pattern is the abstract-class equivalent of this interface-based design; swapping optimizers works because they implement a common interface
- [[Policy]] — a policy is an interface contract: given a state, return an action; many implementations (random, trained, rule-based) satisfy the same `select_action(state)` signature
- [[Activation Function]] — different activation functions (ReLU, sigmoid, tanh) all implement the same interface: take a tensor in, return a tensor out — swappable without changing the surrounding layer
