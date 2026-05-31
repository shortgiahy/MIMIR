# Interfaces and Abstract Classes

**One-liner:** Both define contracts that subclasses must fulfill — abstract classes are partially-implemented blueprints meant for closely related types, while interfaces are pure capability contracts that any unrelated class can implement.

## Why It Exists

Two problems pushed Java toward these constructs:

**Problem 1: Forcing subclasses to implement behavior.**
Consider a `Shape` class with an `area()` method. What should the default implementation be? There is none — a generic "shape" has no area. But if you leave it out, subclasses might forget to implement it, and you'd only discover the bug at runtime when `area()` returns 0 or crashes. You want the compiler to enforce the contract: "Any class that calls itself a Shape *must* implement `area()`."

Abstract classes solve this. Mark `area()` as `abstract`, and the compiler refuses to compile any subclass that doesn't provide an implementation.

**Problem 2: A class has multiple distinct capabilities with no common ancestor.**
A `RobotArm` in Baymax can both move (`Movable`) and report its status (`Diagnosable`). These are completely unrelated capabilities. You can't inherit from two classes in Java — the "diamond problem" makes multiple class inheritance ambiguous and fragile. But you absolutely should be able to say "this class does both things."

Interfaces solve this. You can implement as many interfaces as you want. An interface declares what a class can do; the class provides the how.

These two constructs are the answer to two separate questions, which is why Java has both.

## The Concept

### Abstract Classes

An **abstract class** is a class declared with the `abstract` keyword. It cannot be instantiated directly — you cannot write `new Shape()`. It exists to be extended.

Abstract classes can contain:
- **Abstract methods:** method signature with no body, declared with `abstract`. Every non-abstract subclass must implement these.
- **Concrete methods:** fully implemented methods that subclasses inherit as-is (or override).
- **Fields:** state, just like any class.
- **Constructors:** called by subclasses via `super(...)`.

The design intent: an abstract class represents a **partially-defined type**. It captures what all members of this family have in common (the concrete parts), and delegates what varies per member (the abstract parts) to subclasses. The abstract class says "here's everything we share — you fill in what makes you unique."

### Interfaces

An **interface** is a purely abstract type, declared with the `interface` keyword. It defines a contract: a set of methods that any implementing class promises to provide.

Key properties:
- All methods declared in an interface are implicitly `public abstract` (before Java 8).
- A class implements an interface with `implements`, not `extends`.
- A class can implement **multiple interfaces** — this is the core advantage.
- Interfaces cannot have constructors or instance fields (only `static final` constants).
- Since Java 8: interfaces can have `default` methods (concrete implementations) and `static` methods.

The design intent: an interface represents a **capability or role**. It says nothing about what a type is (that's inheritance's job); it says what a type can do. `Comparable`, `Serializable`, `Runnable` are all Java standard library interfaces — they define capabilities that any class can adopt regardless of its inheritance chain.

### The Key Difference

| | Abstract Class | Interface |
|---|---|---|
| Instantiate directly | No | No |
| Can have concrete methods | Yes | Yes (via `default`, Java 8+) |
| Can have instance fields | Yes | No |
| Can have constructors | Yes | No |
| A class can extend | One only | Multiple |
| Use when | Closely related types share code | Unrelated types share a capability |

**The rule of thumb:** If you're modeling an **is-a** relationship between closely related types, and they share real implementation — use an abstract class. If you're defining a **can-do** capability that unrelated classes might share — use an interface.

Ask yourself: "Does an implementing class *become* this thing, or does it *gain the ability to do* this thing?" Becomes → abstract class. Gains ability → interface.

### When interfaces win decisively

Interfaces decouple callers from implementations far more cleanly than abstract classes. Consider:

```java
List<String> names = new ArrayList<>();
// Later, if you switch to LinkedList:
List<String> names = new LinkedList<>();
```

`List` is an interface. Your code uses `List` everywhere — it doesn't know or care whether the actual object is an `ArrayList` or `LinkedList`. If `List` were a class and `ArrayList` extended it, you'd be locked into a specific implementation. By programming to the interface, you can swap implementations without touching any code that uses names.

This is the **"program to an interface, not an implementation"** principle — one of the most important design ideas in Java.

## Intuition

**Abstract class** is like an architectural template for a building type. An "ApartmentBuilding" template might already define how plumbing works (concrete method), but leaves the floor plan abstract — each actual apartment building fills it in. All apartment buildings are alike in many ways; the template captures the commonality.

**Interface** is like a certification or license. A car, a bus, and a motorcycle all have a driver's license in different configurations. The license says "this entity can be driven on public roads." The car, bus, and motorcycle have nothing else in common — totally different mechanisms — but they all satisfy the "drivable on roads" contract. The license doesn't know how each vehicle works; it just certifies the capability.

For Baymax: `Sensor` might be an abstract class — all sensors share fields like `label` and `lastReading`, plus a concrete `getStatus()` method, but `read()` is abstract because every sensor reads differently. Meanwhile, `Diagnosable` would be an interface — both sensors and actuators might implement it, but they're otherwise unrelated classes.

## Key Example

```java
// Abstract class — common foundation for all shapes
public abstract class Shape {
    private String color;  // Concrete field — all shapes have a color

    public Shape(String color) {
        this.color = color;
    }

    // Abstract method — must be implemented by every subclass
    public abstract double area();
    public abstract double perimeter();

    // Concrete method — inherited as-is by all subclasses
    public String getColor() { return color; }

    public String describe() {
        // Calls the abstract area() — which version? The subclass's.
        return color + " shape with area " + String.format("%.2f", area());
    }
}

// Concrete subclass — must implement area() and perimeter()
public class Circle extends Shape {
    private double radius;

    public Circle(String color, double radius) {
        super(color);
        this.radius = radius;
    }

    @Override
    public double area() { return Math.PI * radius * radius; }

    @Override
    public double perimeter() { return 2 * Math.PI * radius; }
}

// Shape cannot be instantiated
// Shape s = new Shape("red");  // Compile error!

Shape c = new Circle("red", 5);
System.out.println(c.describe());  // red shape with area 78.54
```

```java
// Interface — a capability contract
public interface Resizable {
    void resize(double factor);  // Every implementor must provide this

    // Default method — optional to override, provided as convenience
    default void doubleSize() {
        resize(2.0);
    }
}

public interface Printable {
    void print();
}

// A class can implement multiple interfaces
public class Square extends Shape implements Resizable, Printable {
    private double side;

    public Square(String color, double side) {
        super(color);
        this.side = side;
    }

    @Override
    public double area() { return side * side; }

    @Override
    public double perimeter() { return 4 * side; }

    @Override
    public void resize(double factor) {
        side *= factor;
    }

    @Override
    public void print() {
        System.out.println("Square: " + side + "x" + side);
    }
}

Square sq = new Square("blue", 4);
sq.doubleSize();   // Inherited default method — calls resize(2.0)
sq.print();        // Square: 8.0x8.0

// Polymorphism through interface reference
Resizable r = sq;
r.resize(0.5);     // Works — Square implements Resizable
```

Python equivalent:
```python
from abc import ABC, abstractmethod

# Abstract class in Python — requires abc module
class Shape(ABC):
    def __init__(self, color):
        self.color = color

    @abstractmethod  # Decorator marks method as abstract
    def area(self):
        pass

    def describe(self):
        return f"{self.color} shape with area {self.area():.2f}"

class Circle(Shape):
    def __init__(self, color, radius):
        super().__init__(color)
        self.radius = radius

    def area(self):
        import math
        return math.pi * self.radius ** 2

# Python doesn't have interfaces natively — protocols or abstract classes serve the role
# Python 3.8+ has typing.Protocol for structural subtyping (duck typing with type hints)
from typing import Protocol

class Resizable(Protocol):
    def resize(self, factor: float) -> None: ...
```

Python note: Python doesn't have a separate `interface` keyword. Abstract base classes (`ABC`) serve both roles. Python 3.8+ `Protocol` enables structural subtyping — any class with the right methods satisfies a protocol, even without explicitly declaring it. This is more flexible than Java's explicit `implements`, and useful in Baymax for typing sensor and actuator classes.

## Worked Example

Designing the Baymax sensor/actuator system with both.

```java
// Abstract class — all sensors share these fields and this implementation
public abstract class Sensor {
    protected String label;
    protected double lastReading;
    protected long lastReadTime;

    public Sensor(String label) {
        this.label = label;
        this.lastReading = 0.0;
        this.lastReadTime = 0;
    }

    // Abstract — every sensor reads differently
    public abstract double read();

    // Concrete — identical for all sensors
    public String getStatus() {
        return label + ": " + lastReading + " (at t=" + lastReadTime + ")";
    }

    protected void recordReading(double value) {
        this.lastReading = value;
        this.lastReadTime = System.currentTimeMillis();
    }
}

// Interface — a capability some sensors (and non-sensors) might have
public interface Calibratable {
    void calibrate();
    boolean isCalibrated();
}

// Interface — anything in the system can report diagnostics
public interface Diagnosable {
    String getDiagnostics();
    boolean isHealthy();
}

// Ultrasonic sensor extends Sensor (abstract class) and implements two interfaces
public class UltrasonicSensor extends Sensor implements Calibratable, Diagnosable {
    private int trigPin, echoPin;
    private boolean calibrated = false;
    private double calibrationOffset = 0.0;

    public UltrasonicSensor(String label, int trigPin, int echoPin) {
        super(label);
        this.trigPin = trigPin;
        this.echoPin = echoPin;
    }

    @Override
    public double read() {
        double raw = simulateMeasurement();
        double adjusted = raw + calibrationOffset;
        recordReading(adjusted);
        return adjusted;
    }

    @Override
    public void calibrate() {
        calibrationOffset = -0.5;  // Simulated offset correction
        calibrated = true;
    }

    @Override
    public boolean isCalibrated() { return calibrated; }

    @Override
    public String getDiagnostics() {
        return label + " | calibrated=" + calibrated + " | last=" + lastReading;
    }

    @Override
    public boolean isHealthy() { return lastReading >= 0 && lastReading < 500; }

    private double simulateMeasurement() { return 42.0; }
}

// Actuator — not a Sensor, but also Diagnosable
// Notice: Diagnosable works across completely unrelated class hierarchies
public class WheelMotor implements Diagnosable {
    private String name;
    private double currentSpeed;

    public WheelMotor(String name) {
        this.name = name;
        this.currentSpeed = 0;
    }

    public void setSpeed(double speed) { this.currentSpeed = speed; }

    @Override
    public String getDiagnostics() {
        return name + " | speed=" + currentSpeed;
    }

    @Override
    public boolean isHealthy() { return currentSpeed >= 0 && currentSpeed <= 100; }
}

// The system health check — works on any Diagnosable, regardless of type
List<Diagnosable> components = new ArrayList<>();
components.add(new UltrasonicSensor("front", 4, 5));
components.add(new WheelMotor("left-wheel"));

for (Diagnosable d : components) {
    System.out.println(d.getDiagnostics());
    if (!d.isHealthy()) {
        System.out.println("WARNING: " + d + " is unhealthy!");
    }
}
```

`WheelMotor` and `UltrasonicSensor` have no inheritance relationship, but both implement `Diagnosable`. The health check loop handles them identically. This is interfaces' decisive advantage — polymorphism without forced inheritance.

## Gotchas

**Trying to instantiate an abstract class.** `new Shape()` where `Shape` is abstract throws a compile error. This is by design — abstract classes are incomplete. You must instantiate a concrete subclass.

**Forgetting to implement abstract methods in a subclass.** If your subclass doesn't implement all abstract methods from its parent, you must declare the subclass abstract too. The compiler enforces the complete chain.

**Overusing abstract classes when interfaces are better.** A common beginner pattern: create an abstract class with only abstract methods and no fields. That's an interface. Use the right tool: if there's no shared state or shared implementation, it should be an interface.

**Default methods in interfaces creating unexpected behavior.** Java 8's `default` methods let interfaces provide concrete implementations. If a class implements two interfaces that both define the same `default` method, Java requires you to override the method in the class to resolve the conflict — otherwise it's a compile error. Be aware of this when combining interfaces.

**Interface constants are public static final — always.** If an interface declares a field, it's implicitly `public static final`. You cannot have mutable instance state in an interface. Interfaces that exist purely to hold constants ("constant interfaces") are considered bad practice — use a class or enum instead.

**"Program to the interface" doesn't mean use interfaces everywhere.** This principle means: when declaring variables and parameters, prefer the most general type that still expresses what you need. `List<String>` is better than `ArrayList<String>` for a method parameter. But inside a method where you know you need ArrayList-specific operations, use `ArrayList`. The principle is about coupling at boundaries, not a blanket rule.

## See Also
- [[Inheritance]] — abstract classes are a form of inheritance; the two are intertwined, and the decision between them is a core design choice
- [[Polymorphism]] — both abstract classes and interfaces enable polymorphism; interfaces provide it with less coupling
- [[Encapsulation]] — interfaces are the ultimate encapsulation tool: they expose a contract while hiding all implementation detail
- [[Classes and Objects]] — you must understand what a concrete class is before understanding what it means to be abstract or to implement an interface
