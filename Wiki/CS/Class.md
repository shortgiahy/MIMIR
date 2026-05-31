# Class

**One-liner:** A class is a blueprint that defines what data an object holds and what operations it can perform.

## Core Idea
A class is a compile-time template. It declares **fields** (state) and **methods** (behavior) but doesn't occupy heap memory until an object is instantiated from it. Every object that ever exists must be traced back to exactly one class.

```java
// Minimal class in Java
public class Sensor {
    private String name;      // field — state
    private double reading;   // field — state

    public double getReading() { return reading; }  // method — behavior
}
```

## Why It Exists
Before classes (procedural code), data and the functions that operated on it were separate. You'd have a `double[] sensorReadings` array and a handful of free-floating `processSensor(double[])` functions with no enforced link between them. Classes enforce the coupling: a `Sensor` object owns its data and the only sanctioned way to touch that data is through the methods on the class. This is the root idea behind [[Encapsulation]].

## Real-World Applications
- Every Java standard library type (`ArrayList`, `Scanner`, `Thread`) is a class.
- In Python ML, `torch.nn.Module` is a class — your neural network model inherits from it.
- In ROS 2 (robotics), every node is a class that inherits from `rclpy.node.Node`.
- Giahy's Baymax: sensors (ultrasonic, IMU), actuators (motors, servos), and the robot brain itself are all modeled as classes.

## Intuition
A class is the cookie cutter; objects are the cookies. The cutter defines the shape but doesn't become a cookie itself. You can stamp out as many cookies (objects) as you want from one cutter (class).

## Deep Dive
In Java, a class can contain:
- **Instance fields** — one copy per object
- **Static fields** — one copy shared across all objects (`static`)
- **Instance methods** — operate on `this` object
- **Static methods** — no `this`, utility functions
- **Constructors** — special methods invoked at `new`, see [[Constructor]]
- **Nested classes / inner classes** — advanced; classes inside classes

```java
public class DistanceSensor {
    private static int instanceCount = 0;   // static: shared
    private String id;                       // instance: per-object

    public DistanceSensor(String id) {
        this.id = id;
        instanceCount++;
    }

    public static int getInstanceCount() { return instanceCount; }
    public String getId() { return id; }
}
```

**Python equivalent** — syntactically lighter but same semantics:
```python
class DistanceSensor:
    instance_count = 0          # class variable (≈ static field)

    def __init__(self, id: str):
        self.id = id            # instance variable
        DistanceSensor.instance_count += 1

    @classmethod
    def get_instance_count(cls) -> int:
        return cls.instance_count
```

Key differences:
- Python has no access modifiers; convention uses `_` prefix for "private".
- Python's `self` is explicit; Java's `this` is implicit.
- Python uses `__init__` where Java uses the constructor.

**Design note:** A well-designed class has **single responsibility** — it should model one concept and have one reason to change. A `RobotArm` class should not also handle logging.

## Worked Example
```java
// Baymax context: modeling a proximity sensor
public class ProximitySensor {
    private final String location;   // "left", "right", "front"
    private double distanceCm;

    public ProximitySensor(String location) {
        this.location = location;
        this.distanceCm = Double.MAX_VALUE;  // safe default: no obstacle
    }

    public void update(double cm) {
        if (cm < 0) throw new IllegalArgumentException("Distance cannot be negative");
        this.distanceCm = cm;
    }

    public boolean isObstacleDetected(double thresholdCm) {
        return distanceCm < thresholdCm;
    }

    public String getLocation() { return location; }
}

// Usage
ProximitySensor front = new ProximitySensor("front");
front.update(12.5);
System.out.println(front.isObstacleDetected(20.0));  // true
```

## See Also
- [[Object]] — what you get when you call `new` on a class
- [[Instance]] — synonym for object, emphasizes the relationship back to the class
- [[Constructor]] — the method that runs when a class is instantiated
- [[Encapsulation]] — the principle that motivates keeping state inside a class
- [[Access Modifier]] — the tools that enforce encapsulation
- [[Inheritance]] — one class building on another class's definition
- [[Abstract Class]] — a class that cannot itself be instantiated
