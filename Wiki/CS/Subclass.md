# Subclass

**One-liner:** A subclass (child class) extends a superclass — it inherits everything the parent provides and either extends it (new fields/methods) or specializes it (overriding existing methods).

## Core Idea
A subclass uses `extends` in Java to declare its parent. It automatically has all `public` and `protected` members of the superclass, plus whatever it adds. The subclass satisfies an **is-a** relationship: an `UltrasonicSensor` is-a `Sensor`.

```java
public class UltrasonicSensor extends Sensor {   // subclass
    private double maxRangeM;                    // new state
    private static final double SOUND_SPEED = 343.0;

    public UltrasonicSensor(String id, double maxRangeM) {
        super(id);   // must call parent constructor
        this.maxRangeM = maxRangeM;
    }

    @Override
    public void poll() {    // specialized behavior — replaces parent version
        double us = readEchoPin();
        lastReading = (us * SOUND_SPEED) / 2_000_000.0;
    }

    public boolean isObstacleClose() { return lastReading < 0.3; } // new behavior
}
```

## Why It Exists
A subclass exists when you need a type that is **mostly like** an existing type but differs in specifics. Rather than duplicating the entire parent class, you write only the delta — the new fields and overridden methods. This enforces DRY and keeps type relationships explicit.

## Real-World Applications
- `IOException` extends `Exception` — it IS an exception with extra I/O context.
- `LinkedList` extends `AbstractSequentialList` — gets default implementations, only overrides efficient ones.
- PyTorch: your custom layer `class MyLayer(nn.Module)` is a subclass.
- Baymax: `UltrasonicSensor extends RangeSensor extends Sensor` — three levels, each adding specificity.

## Intuition
A subclass is a resume with a specialization. You already have the general "Software Engineer" job description (superclass). A "Robotics Software Engineer" (subclass) keeps everything on that description but adds "experience with ROS 2, motor controllers, sensor fusion." Same base, specialized extension.

## Deep Dive
**Extension vs. specialization**
- **Extension**: adding new fields/methods the parent doesn't have. `UltrasonicSensor.isObstacleClose()` is new behavior — the parent knows nothing about it.
- **Specialization**: overriding existing methods to provide a more specific implementation. `poll()` overriding the parent's generic implementation to do HC-SR04–specific math.

Both are valid uses of subclasses.

**Liskov Substitution Principle (LSP)**
The key rule for correct subclassing: a subclass should be substitutable for its superclass without breaking the program. If code that works with `Sensor` breaks when you pass an `UltrasonicSensor`, your subclass violates LSP.

Violating LSP:
```java
public class MockSensor extends Sensor {
    @Override
    public void poll() {
        throw new UnsupportedOperationException();  // BREAKS: callers expect poll() to work
    }
}
```

**Narrowing vs. widening**
A subclass type variable can hold only that subclass (or sub-subclasses). A superclass variable can hold any subclass instance — this widening is the basis of polymorphism. See [[Upcasting and Downcasting]].

```java
Sensor s = new UltrasonicSensor("front", 4.0);  // OK — widening (upcasting)
UltrasonicSensor u = (UltrasonicSensor) s;       // narrowing (downcasting) — needs cast
```

**`final` subclasses**
Mark a class `final` to prevent it from being subclassed. Useful when further extension would be dangerous (e.g., security-sensitive classes).

**Constructor requirement**
Subclass constructors must call `super(...)` (or rely on the implicit `super()` if the parent has a no-arg constructor). The parent's constructor runs first — the parent part of the object is fully initialized before the subclass constructor body runs.

**Python**
```python
class UltrasonicSensor(Sensor):  # (Sensor) = extends Sensor
    SOUND_SPEED_M_S = 343.0

    def __init__(self, id: str, max_range_m: float):
        super().__init__(id)   # call Sensor.__init__
        self.max_range_m = max_range_m

    def poll(self):            # override
        us = self._read_echo_pin()
        self.latest_reading = (us * self.SOUND_SPEED_M_S) / 2_000_000.0

    def is_obstacle_close(self) -> bool:   # new method
        return self.latest_reading < 0.3
```

## Worked Example
```java
// Baymax context: arm joints as subclasses of a generic Joint

public abstract class Joint {
    protected final String name;
    protected int currentAngle;
    protected final int minAngle;
    protected final int maxAngle;

    public Joint(String name, int minAngle, int maxAngle, int initialAngle) {
        this.name = name;
        this.minAngle = minAngle;
        this.maxAngle = maxAngle;
        this.currentAngle = initialAngle;
    }

    public void moveTo(int angle) {
        this.currentAngle = Math.clamp(angle, minAngle, maxAngle);
    }

    public int getAngle() { return currentAngle; }
}

// Specialization: ElbowJoint adds a physical constraint check
public class ElbowJoint extends Joint {
    public ElbowJoint() {
        super("elbow", 0, 145, 90);  // elbow can't straighten past 145°
    }

    @Override
    public void moveTo(int angle) {
        // Additional constraint: don't move more than 30° per command (hardware safety)
        int delta = Math.abs(angle - currentAngle);
        if (delta > 30) {
            System.out.println("Elbow: large movement capped for safety");
            angle = currentAngle + Integer.signum(angle - currentAngle) * 30;
        }
        super.moveTo(angle);  // delegate to parent for range clamping
    }
}

// Extension: WristJoint adds rotation tracking
public class WristJoint extends Joint {
    private double totalRotationDegrees = 0.0;   // new state

    public WristJoint() { super("wrist", -90, 90, 0); }

    @Override
    public void moveTo(int angle) {
        totalRotationDegrees += Math.abs(angle - currentAngle);   // track wear
        super.moveTo(angle);
    }

    public double getTotalRotation() { return totalRotationDegrees; }  // new behavior
}
```

## See Also
- [[Superclass]] — the class being extended
- [[Inheritance]] — the mechanism that connects subclass to superclass
- [[Method Overriding]] — how subclasses replace parent method implementations
- [[Polymorphism]] — subclass objects can masquerade as their superclass type
- [[Abstract Class]] — a superclass that forces subclasses to implement specific methods
- [[Upcasting and Downcasting]] — the type-conversion mechanics when moving between superclass and subclass references
- [[Composition over Inheritance]] — the design alternative when is-a doesn't truly hold
- [[Vector]] — in linear algebra a vector IS-A matrix (a single-column matrix), exactly mirroring the subclass is-a superclass relationship; Vector inherits matrix operations and adds its own (dot product, magnitude)
- [[Agent]] — concrete RL agent types (PPOAgent, DQNAgent) are subclasses of a base Agent; they inherit the training loop and replay buffer while overriding the policy update logic
