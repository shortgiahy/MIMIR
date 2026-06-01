# Abstract Class

**One-liner:** An abstract class is a class that cannot be instantiated, may declare abstract methods that subclasses are required to implement, and can also provide concrete fields and shared behavior — a partial template for a family of related types.

## Core Idea
Mark a class `abstract` and it can no longer be created with `new`. It exists only to be extended. Abstract methods inside it declare a method signature with no body — the compiler enforces that every non-abstract subclass provides an implementation.

```java
public abstract class Sensor {
    protected final String id;   // concrete field — every subclass gets this

    public Sensor(String id) { this.id = id; }  // concrete constructor

    public abstract void poll();         // abstract — MUST be overridden
    public abstract String report();     // abstract — MUST be overridden

    // Concrete method — shared behavior, subclasses inherit for free
    public String getId() { return id; }
    public boolean isStale(long ageMs) { return ageMs > 500; }
}

// This compiles:
public class UltrasonicSensor extends Sensor {
    public UltrasonicSensor(String id) { super(id); }
    @Override public void poll()       { /* hardware read */ }
    @Override public String report()   { return "[US:" + id + "]"; }
}

// This does NOT compile — missing poll() and report():
public class BrokenSensor extends Sensor { }  // ← compiler error

// This does NOT compile — cannot instantiate abstract class:
Sensor s = new Sensor("x");  // ← compiler error
```

## Why It Exists
Without abstract classes you face a fork in the road:
- Use a plain superclass with empty or stub implementations of methods every subclass must override. Nothing forces subclasses to override them, so forgetting silently produces bugs (the stub is called instead of the real implementation).
- Use an [[Interface]] for the contract. But then you lose the ability to share concrete state and default behavior — every subclass must re-implement boilerplate.

Abstract classes occupy the middle ground: **mandatory contract** (abstract methods) plus **shared implementation** (concrete methods and fields). A `Sensor` abstract class can own the `id`, provide `getId()`, and mandate that all subclasses supply `poll()` — without leaving a footgun.

## Real-World Applications
- Java standard library: `java.io.InputStream` is abstract. `poll()` is like its abstract `read()` — every concrete substream (`FileInputStream`, `ByteArrayInputStream`) implements it differently; shared buffering logic lives in concrete methods.
- PyTorch: `torch.nn.Module` is effectively abstract — your model class must implement `forward()`. This is the Python equivalent of an abstract method enforced by convention (Python's `abc.ABC` enforces it properly).
- Android: `RecyclerView.Adapter` is abstract; `onCreateViewHolder()` and `onBindViewHolder()` must be overridden.
- Baymax: `Sensor` is a natural abstract class — every real sensor has a `mountId`, a `poll()` that reads hardware, and a `statusReport()`. The abstract class provides the shared fields, forces the hardware read to be implemented, and offers shared utility methods.

## Intuition
An abstract class is a partially filled-in form. It has some fields already completed (shared state), some instructions already printed (concrete methods), and some blank lines labeled "fill this in" (abstract methods). You cannot submit the form blank — you must complete those fields. But the parts already printed come for free.

A blueprint for a house can specify "must have a front door" (abstract method) and provide standard plumbing diagrams (concrete method), but you cannot live in the blueprint itself — you must build a specific house from it.

## Deep Dive
**Abstract class vs. Interface — the decision matrix**

| Criterion | Abstract Class | Interface |
|---|---|---|
| Instantiate directly? | No | No |
| Can have instance fields? | Yes | No (only `static final`) |
| Can have concrete methods? | Yes | Yes (via `default`) |
| Can have constructors? | Yes | No |
| Multiple inheritance of type? | No (`extends` one class) | Yes (`implements` many) |
| Models a "is-a" relationship? | Yes | Models "can-do" / "has-capability" |
| Ideal for | Family of related types with shared state | Capability contract across unrelated types |

**Rule of thumb:**
- Related types that share fields and some behavior → abstract class.
- Unrelated types that share a capability → interface.
- When in doubt, prefer interface (more flexible, allows multiple).
- Combine both: `public class UltrasonicSensor extends Sensor implements Calibratable`.

**Abstract classes can implement interfaces**
An abstract class can implement an interface *without* providing all the method bodies. Subclasses of the abstract class then inherit the obligation to implement those methods.

```java
public interface Diagnostics {
    String selfTest();
    Map<String, String> getHealthReport();
}

public abstract class Sensor implements Diagnostics {
    // Provides selfTest() concretely:
    @Override
    public String selfTest() { return poll() != null ? "OK" : "FAIL"; }

    // Leaves getHealthReport() abstract — subclasses decide:
    @Override
    public abstract Map<String, String> getHealthReport();
}
```

**Python — `abc.ABC`**
Python enforces abstract methods via the `abc` module. Attempting to instantiate a class with unimplemented abstract methods raises `TypeError` at runtime (not compile time, since Python has no compiler).

```python
from abc import ABC, abstractmethod

class Sensor(ABC):
    def __init__(self, id: str):
        self.id = id                # concrete field

    @abstractmethod
    def poll(self) -> None: ...     # abstract — subclasses must implement

    @abstractmethod
    def report(self) -> str: ...    # abstract

    def get_id(self) -> str:        # concrete — shared for free
        return self.id

class UltrasonicSensor(Sensor):
    def poll(self) -> None:
        self._distance = self._read_hardware()
    def report(self) -> str:
        return f"[US:{self.id}] {self._distance:.2f}m"
    def _read_hardware(self) -> float:
        return 1.5  # stub

# TypeError: Can't instantiate abstract class Sensor with abstract methods poll, report
# s = Sensor("x")   ← would fail at runtime
```

This mirrors Java's compile-time enforcement. In PyTorch, `nn.Module` uses a similar pattern: `forward()` raises `NotImplementedError` if not overridden (a manual enforcement rather than `@abstractmethod`).

**Design consideration: the fragile base class**
Abstract classes with concrete methods create coupling. If you change a concrete method in the abstract class, every subclass is affected — including ones in third-party libraries. This is the **fragile base class problem** (see [[Composition over Inheritance]]). Favor small, well-defined abstract contracts over large abstract classes with many concrete methods.

## Worked Example
```java
// Baymax context: complete sensor hierarchy with shared and enforced behavior

public abstract class Sensor {
    protected final String mountId;
    protected double lastReading = Double.NaN;
    protected long   lastPollMs  = 0;

    public Sensor(String mountId) {
        this.mountId = mountId;
    }

    // Abstract — each sensor type reads hardware differently
    public abstract void poll();
    public abstract String unitLabel();   // "m", "°", "lux", etc.

    // Concrete — every sensor inherits this staleness check
    public boolean isStale() {
        return (System.currentTimeMillis() - lastPollMs) > 500;
    }

    // Concrete — default report format; subclasses may override for richer output
    public String statusReport() {
        return String.format("[%s] %s: %.2f %s%s",
            getClass().getSimpleName(), mountId, lastReading, unitLabel(),
            isStale() ? " (STALE)" : "");
    }

    public String getMountId()    { return mountId; }
    public double getLastReading() { return lastReading; }
}

public class UltrasonicSensor extends Sensor {
    private final double maxRangeM;

    public UltrasonicSensor(String mountId, double maxRangeM) {
        super(mountId);
        this.maxRangeM = maxRangeM;
    }

    @Override
    public void poll() {
        // HC-SR04 logic
        lastReading = readEchoPulse() * 343.0 / 2_000_000.0;
        lastPollMs  = System.currentTimeMillis();
    }

    @Override
    public String unitLabel() { return "m"; }

    // Extra behavior: in-range check (not on abstract class — not all sensors have this)
    public boolean isInRange() { return lastReading <= maxRangeM; }

    private double readEchoPulse() { return 2_300.0; /* stub */ }
}

public class LightSensor extends Sensor {
    public LightSensor(String mountId) { super(mountId); }

    @Override
    public void poll() {
        lastReading = readADC() * 3.3 / 1024.0 * 1000; // convert to lux
        lastPollMs  = System.currentTimeMillis();
    }

    @Override
    public String unitLabel() { return "lux"; }

    private double readADC() { return 512.0; /* stub */ }
}

// Usage
List<Sensor> sensors = List.of(
    new UltrasonicSensor("front", 4.0),
    new LightSensor("ambient")
);

for (Sensor s : sensors) {
    s.poll();
    System.out.println(s.statusReport());
}
// [UltrasonicSensor] front: 0.39 m
// [LightSensor] ambient: 1650.00 lux
```

Both concrete classes got `statusReport()`, `isStale()`, `getMountId()`, and `getLastReading()` for free. Both were *forced* to implement `poll()` and `unitLabel()` — the compiler rejected any attempt to skip them.

## See Also
- [[Interface]] — the pure contract sibling; prefer when classes are unrelated or you need multiple inheritance of type
- [[Inheritance]] — abstract classes are extended exactly like concrete superclasses
- [[Method Overriding]] — abstract methods are a compiler-enforced override requirement
- [[Dynamic Dispatch]] — abstract methods still dispatch to the correct subclass implementation at runtime
- [[Polymorphism]] — abstract classes are the most common polymorphism enabler in Java OOP
- [[Composition over Inheritance]] — when abstract class hierarchies grow too deep, switch to this
- [[Gradient Descent]] — PyTorch's `nn.Module` (abstract class pattern) is used to build every neural network that runs gradient descent
- [[Neural Network]] — each layer in a neural network is a concrete subclass of an abstract "layer" type that mandates `forward()` — the canonical real-world abstract class pattern
- [[Agent]] — RL agent hierarchies (BaseAgent → PolicyGradientAgent → PPOAgent) use abstract classes to share training infrastructure while forcing subclasses to implement the core decision logic
