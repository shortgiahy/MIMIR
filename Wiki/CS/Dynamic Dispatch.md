# Dynamic Dispatch

**One-liner:** Dynamic dispatch is the runtime mechanism by which Java looks at an object's actual type — not its declared type — to decide which overridden method to call.

## Core Idea
When you call a method on a reference, Java does not lock in which implementation to run at compile time. Instead, it defers that decision to runtime, consulting the object's actual class. This is why a `Sensor` reference pointing at an `UltrasonicSensor` object calls `UltrasonicSensor.poll()`, not `Sensor.poll()`.

```java
Sensor s = new UltrasonicSensor("front", 4.0);  // declared: Sensor, actual: UltrasonicSensor
s.poll();  // dynamic dispatch → calls UltrasonicSensor.poll(), NOT Sensor.poll()
```

The declared type (`Sensor`) governs what methods you are *allowed* to call. The actual (runtime) type governs *which version* of those methods runs.

## Why It Exists
Without dynamic dispatch, [[Polymorphism]] would be impossible. If Java wired method calls to the declared type at compile time, every `Sensor` reference would always run `Sensor.poll()` — regardless of the actual object. You would be forced back to explicit `instanceof` chains to invoke the correct subclass behavior, defeating the entire purpose of inheritance.

```java
// What you'd be stuck writing without dynamic dispatch:
if (s instanceof UltrasonicSensor) {
    ((UltrasonicSensor) s).poll();
} else if (s instanceof IMUSensor) {
    ((IMUSensor) s).poll();
}
// Fragile — add a new sensor type → update every chain
```

Dynamic dispatch moves this decision to the runtime so the object itself carries the knowledge of its own behavior.

## Real-World Applications
- Java collections: `List<Shape>` containing circles and rectangles — `shape.area()` dispatches to the right formula for each object.
- Android framework: `Activity.onResume()` — the framework calls it on whatever `Activity` subclass is active; it runs the subclass's override.
- PyTorch: `model(x)` calls `__call__`, which dispatches to the `forward()` you overrode in your `nn.Module` subclass. Every layer, every custom model, dispatches dynamically.
- Baymax: `List<Sensor>` in the robot's sensor manager — `sensor.poll()` dispatches to the ultrasonic, IR, or IMU implementation depending on which physical sensor the object represents.

## Intuition
Think of dynamic dispatch as caller ID resolution at delivery time rather than shipping time. When you mail a package to "the person at 42 Oak St," you don't know at the moment of mailing which specific person lives there. The postal carrier resolves that at delivery time by checking who actually lives there. Dynamic dispatch is the JVM checking the object's actual type at the moment the call is made, not when the code was compiled.

## Deep Dive
**The vtable (virtual method table)**
The JVM implements dynamic dispatch using a per-class data structure called a **vtable** (virtual table). Each class has a vtable — a table of function pointers, one entry per overrideable method. When you call `s.poll()`:

1. The JVM reads the object header on the heap to find the object's actual class (`UltrasonicSensor`).
2. It looks up `poll` in `UltrasonicSensor`'s vtable.
3. It jumps to that implementation.

If `UltrasonicSensor` overrides `poll`, its vtable entry points at `UltrasonicSensor.poll`. If it does not override it, the entry is inherited from the parent's vtable. This lookup is O(1) — extremely fast.

```
Sensor vtable:             UltrasonicSensor vtable:
  poll → Sensor.poll         poll → UltrasonicSensor.poll   ← overridden
  describe → Sensor.describe  describe → Sensor.describe    ← inherited
  toString → Object.toString  toString → Object.toString    ← inherited
```

**Static methods are NOT dispatched dynamically**
Static methods are resolved at compile time using the declared type. There is no vtable lookup.

```java
Sensor s = new UltrasonicSensor("front", 4.0);
s.poll();           // dynamic: runs UltrasonicSensor.poll()
Sensor.describe();  // static: always runs Sensor.describe() — declared type wins
```

**`private` and `final` methods are NOT dynamically dispatched**
`private` methods cannot be overridden, so the compiler resolves them statically (direct call). `final` methods are also resolved statically — the JVM can even inline them for performance.

**Compile-time polymorphism vs. runtime polymorphism**
- Method **overloading** (same name, different parameter types in the same class): resolved at **compile time** — not dynamic dispatch.
- Method **overriding** (same signature in a subclass): resolved at **runtime** — dynamic dispatch. This is what "polymorphism" normally refers to.

**Python equivalent — MRO lookup**
Python's dynamic dispatch works through the Method Resolution Order (MRO). When you call `obj.method()`, Python walks `type(obj).__mro__` until it finds a class that defines `method`. There is no vtable; it's a dictionary lookup per class in the MRO chain. Slightly slower than Java's vtable, but more flexible (supports multiple inheritance).

```python
class Sensor:
    def poll(self): print("Sensor.poll")

class UltrasonicSensor(Sensor):
    def poll(self): print("UltrasonicSensor.poll")

s: Sensor = UltrasonicSensor()  # type annotation is just a hint
s.poll()  # Python checks type(s) → UltrasonicSensor → found poll → calls it
# Output: UltrasonicSensor.poll
```

**Performance note**
Java's JIT compiler profiles vtable dispatch and can **devirtualize** (convert a virtual call to a direct call) or **inline** methods that are called on the same concrete type in a hot loop. In practice, dynamic dispatch overhead is negligible in well-written Java.

## Worked Example
```java
// Baymax context: heterogeneous sensor array, driven by one loop

public abstract class Sensor {
    protected final String id;
    public Sensor(String id) { this.id = id; }
    public abstract void poll();           // subclasses must override
    public abstract String statusReport();
}

public class UltrasonicSensor extends Sensor {
    private double distanceM;
    public UltrasonicSensor(String id) { super(id); }

    @Override public void poll() {
        distanceM = readHardware();  // specific HC-SR04 logic
    }
    @Override public String statusReport() {
        return String.format("[US:%s] %.2f m", id, distanceM);
    }
    private double readHardware() { return 1.23; }
}

public class IMUSensor extends Sensor {
    private double roll, pitch;
    public IMUSensor(String id) { super(id); }

    @Override public void poll() {
        roll  = readRoll();   // specific IMU register reads
        pitch = readPitch();
    }
    @Override public String statusReport() {
        return String.format("[IMU:%s] r=%.1f° p=%.1f°", id, roll, pitch);
    }
    private double readRoll()  { return 2.1; }
    private double readPitch() { return -0.5; }
}

// The dispatch loop — zero if-chains, zero coupling to concrete types
public class SensorManager {
    private final List<Sensor> sensors = new ArrayList<>();

    public void addSensor(Sensor s) { sensors.add(s); }

    public void runCycle() {
        for (Sensor s : sensors) {
            s.poll();                       // dynamic dispatch → correct poll()
            System.out.println(s.statusReport()); // dynamic dispatch → correct report()
        }
    }
}

// Main
SensorManager mgr = new SensorManager();
mgr.addSensor(new UltrasonicSensor("front"));
mgr.addSensor(new IMUSensor("body"));
mgr.addSensor(new UltrasonicSensor("rear"));

mgr.runCycle();
// [US:front] 1.23 m
// [IMU:body] r=2.1° p=-0.5°
// [US:rear] 1.23 m
```

Notice that `SensorManager` is closed to modification — adding a new `LidarSensor` class requires zero changes to `SensorManager`. The vtable does the routing.

## See Also
- [[Method Overriding]] — creates the alternative implementations that dispatch chooses between
- [[Polymorphism]] — the high-level concept that dynamic dispatch enables
- [[Upcasting and Downcasting]] — how objects end up under supertype references in the first place
- [[Abstract Class]] — guarantees all subclasses have an implementation to dispatch to
- [[Interface]] — interfaces also use dynamic dispatch for their method calls
- [[Inheritance]] — the inheritance hierarchy defines the vtable chain
