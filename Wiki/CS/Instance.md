# Instance

**One-liner:** An instance is one concrete realization of a class — the word emphasizes the class it came from, where "object" emphasizes the heap entity itself.

## Core Idea
"Instance" and "object" are nearly synonymous; the distinction is linguistic and contextual. When you say **instance** you're stressing the relationship: *this* is an instance *of* some class. When you say **object** you're stressing the in-memory entity. In practice, "an instance of `Sensor`" and "a `Sensor` object" mean the same thing.

Instance members (fields and methods without `static`) belong to each individual instance — each one has its own copy of instance fields, independent of every other instance.

```java
DistanceSensor s1 = new DistanceSensor("front");  // instance 1
DistanceSensor s2 = new DistanceSensor("rear");   // instance 2
// s1 and s2 are two separate instances of DistanceSensor
// s1.id and s2.id are independent — changing one doesn't affect the other
```

## Why It Exists
The word "instance" lets programmers distinguish **instance-level** concerns (per-object state) from **class-level** concerns (shared state, `static` members). "Is this a static method or an instance method?" is a critical design question. Instance methods operate on `this`; static methods don't belong to any particular object.

## Real-World Applications
- UML diagrams draw a class as a box; objects are called "instances" in the instance diagram (object diagram).
- Java reflection: `obj.getClass()` returns the `Class` object — the meta-instance of the class at runtime.
- Python `isinstance(obj, SomeClass)` tests whether `obj` is an instance of `SomeClass` (or a subclass).
- In Baymax: each `Motor` object is an instance of the `Motor` class; they share the class's method code but hold independent speed/direction fields.

## Intuition
Think of a cookie cutter (class) and cookies (instances). Each cookie is an instance. The phrase "that cookie is an instance of the star-shaped cutter" emphasizes lineage. "That cookie is an object" emphasizes its physical existence on the plate.

## Deep Dive
**Instance fields vs. static fields**
```java
public class Motor {
    private static int totalMotors = 0;    // ONE copy, shared across all instances
    private int motorId;                   // ONE copy PER instance
    private double speedRpm;              // ONE copy PER instance

    public Motor() {
        totalMotors++;
        this.motorId = totalMotors;
    }

    // Instance method — implicitly receives `this`
    public void setSpeed(double rpm) { this.speedRpm = rpm; }

    // Static method — no `this`, no specific instance
    public static int getTotalMotors() { return totalMotors; }
}

Motor m1 = new Motor();  // motorId = 1, totalMotors = 1
Motor m2 = new Motor();  // motorId = 2, totalMotors = 2
Motor m3 = new Motor();  // motorId = 3, totalMotors = 3

m1.setSpeed(120);   // only m1's speedRpm changes
Motor.getTotalMotors();  // 3 — class-level, not instance-level
```

**Python — `self` makes instances explicit**
```python
class Motor:
    total_motors = 0          # class variable — shared

    def __init__(self):
        Motor.total_motors += 1
        self.motor_id = Motor.total_motors   # instance variable
        self.speed_rpm = 0.0

    def set_speed(self, rpm: float):
        self.speed_rpm = rpm   # operates on THIS instance

m1 = Motor()
m2 = Motor()
m1.set_speed(120)   # only m1 changes
print(Motor.total_motors)  # 2
```

**`instanceof` (type checking)**
```java
Object sensor = new UltrasonicSensor("front");
if (sensor instanceof UltrasonicSensor us) {  // Java 16+ pattern matching
    System.out.println(us.getDistance());
}
```

This checks whether `sensor` is an instance of `UltrasonicSensor` (or any subclass). Related to [[Upcasting and Downcasting]].

## Worked Example
```java
// Baymax context: creating multiple instances and showing independence
public class ServoMotor {
    private final String joint;   // "elbow", "shoulder", "wrist"
    private int angleDegrees;

    public ServoMotor(String joint, int initialAngle) {
        this.joint = joint;
        this.angleDegrees = initialAngle;
    }

    public void moveTo(int angle) {
        if (angle < 0 || angle > 180) throw new IllegalArgumentException("Out of range");
        this.angleDegrees = angle;
        System.out.printf("%s moved to %d°%n", joint, angle);
    }

    public int getAngle() { return angleDegrees; }
}

// Three instances — each tracks its own angle independently
ServoMotor elbow    = new ServoMotor("elbow", 90);
ServoMotor shoulder = new ServoMotor("shoulder", 45);
ServoMotor wrist    = new ServoMotor("wrist", 0);

elbow.moveTo(120);   // "elbow moved to 120°"
// shoulder and wrist unchanged
System.out.println(shoulder.getAngle()); // 45
```

## See Also
- [[Class]] — the definition this instance was created from
- [[Object]] — the heap entity; "instance" and "object" are near-synonyms
- [[Constructor]] — the code that initializes each new instance
- [[Encapsulation]] — each instance's state is private to itself
- [[NumPy Array]] — each `np.array(...)` call produces a distinct ndarray instance with its own data buffer and metadata; understanding instance independence explains why copying an array (`np.copy`) is needed to avoid aliasing bugs
- [[Environment]] — in RL, running multiple parallel environments means creating multiple `gym.Env` instances; each instance independently tracks its own episode state, exactly as separate Java objects hold independent field values
