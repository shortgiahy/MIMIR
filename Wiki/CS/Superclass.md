# Superclass

**One-liner:** A superclass (parent class) is the class being extended — it provides shared state and behavior that all its subclasses inherit automatically.

## Core Idea
A superclass defines the **common contract**: the fields and methods that every subclass is guaranteed to have. It sits above its subclasses in the inheritance hierarchy. In Java, a class uses `extends ClassName` to declare its superclass.

```java
// Sensor is the superclass
public class Sensor {
    protected final String id;
    protected double latestReading;

    public Sensor(String id) { this.id = id; }
    public String getId()     { return id; }
    public double getReading(){ return latestReading; }
}

// UltrasonicSensor is a subclass of Sensor
public class UltrasonicSensor extends Sensor { ... }
```

Everything in `Sensor` is automatically available in `UltrasonicSensor` without re-writing it.

## Why It Exists
A superclass is the vehicle for the **Don't Repeat Yourself (DRY)** principle across related classes. When you have five sensor types that all need an ID, a latest reading, and a `getReading()` method, you put that in `Sensor` once. Bug fixed in `Sensor` is fixed for all five subclasses automatically.

The superclass also defines the **polymorphic type**: code that accepts a `Sensor` reference works with any subclass object. See [[Polymorphism]].

## Real-World Applications
- `java.lang.Object` is the ultimate superclass of every Java class.
- `java.lang.Exception` is the superclass of all exception types.
- In Android: `AppCompatActivity` is the superclass you extend for every screen.
- PyTorch: `torch.nn.Module` is the superclass for every neural network module.
- Baymax: `Sensor` is the superclass; `UltrasonicSensor`, `IRSensor`, `IMUSensor` are subclasses — all sharing the same polling and reading interface.

## Intuition
A superclass is the job description template. A company has one `Employee` template (ID, salary, department, `getPaycheck()`). Then specific roles — `Engineer`, `Manager`, `Intern` — fill in the template and add their own specifics. The template is the superclass.

## Deep Dive
**`super` keyword — accessing the parent**
The `super` keyword in a subclass refers to the superclass portion of the same object. You use it to:
1. Call the superclass constructor: `super(args)` — must be first line in subclass constructor.
2. Call a superclass method that the subclass has overridden: `super.methodName()`.

```java
public class RangeSensor extends Sensor {
    private double maxRange;

    public RangeSensor(String id, double maxRange) {
        super(id);              // call Sensor's constructor
        this.maxRange = maxRange;
    }

    @Override
    public String toString() {
        return super.toString() + " [max=" + maxRange + "m]";  // extend parent's toString
    }
}
```

**Abstract superclasses**
A superclass can be `abstract` — it defines the shape of the hierarchy but cannot be instantiated directly. This is appropriate when the superclass concept is too general to have a meaningful standalone object. See [[Abstract Class]].

**Protected members**
Superclass members marked `protected` are accessible in subclasses. `private` members exist in subclass objects (the memory is there) but the subclass code cannot directly read or write them — they must go through `protected` or `public` methods on the parent. See [[Access Modifier]].

**Designing a good superclass**
- Should model a genuine abstraction shared by all subclasses (Liskov Substitution Principle).
- Should NOT change frequently — changes ripple to all subclasses (fragile base class problem).
- Should be open for extension, closed for modification (Open/Closed Principle).
- If you find yourself adding behavior to a superclass that only one subclass uses, it probably belongs in that subclass, not the parent.

**`Object` — the root superclass**
Every class implicitly extends `Object`. That's where these come from:
- `toString()` — default: classname@hashcode
- `equals(Object o)` — default: reference equality (`==`)
- `hashCode()` — default: memory address–based
- `getClass()` — returns the runtime Class object

Override `equals` and `hashCode` together; never override one without the other.

## Worked Example
```java
// Baymax context: Actuator superclass for all output devices

public abstract class Actuator {
    protected final String actuatorId;
    protected boolean enabled = false;

    public Actuator(String actuatorId) {
        this.actuatorId = actuatorId;
    }

    // Template method: subclasses override doEnable/doDisable
    public final void enable() {
        doEnable();
        enabled = true;
        System.out.println(actuatorId + " enabled");
    }

    public final void disable() {
        doDisable();
        enabled = false;
        System.out.println(actuatorId + " disabled");
    }

    protected abstract void doEnable();
    protected abstract void doDisable();
    public boolean isEnabled() { return enabled; }
}

// Motor and Servo both extend Actuator — they inherit enable/disable lifecycle
public class Motor extends Actuator {
    public Motor(String id) { super(id); }

    @Override protected void doEnable()  { /* set PWM pin high */ }
    @Override protected void doDisable() { /* set PWM pin low, brake */ }

    public void setSpeed(double rpm) {
        if (!enabled) throw new IllegalStateException("Motor not enabled");
        /* ... */
    }
}
```

## See Also
- [[Class]] — a superclass is just a class being extended by another
- [[Subclass]] — the class that extends this one
- [[Inheritance]] — the mechanism connecting superclass and subclass
- [[Abstract Class]] — a superclass that enforces a contract on subclasses
- [[Method Overriding]] — how subclasses replace superclass behavior
- [[Polymorphism]] — enables treating subclass objects as their superclass type
