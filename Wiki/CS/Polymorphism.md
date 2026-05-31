# Polymorphism

**One-liner:** Polymorphism means "many forms" — it's the ability for a single interface (a method call, a variable type) to work correctly across objects of different types, each doing the right thing for its type.

## Why It Exists

Suppose you have three sensor classes: `UltrasonicSensor`, `TemperatureSensor`, and `IRSensor`. Each has a `read()` method. Now you want to poll all sensors in a loop. Without polymorphism:

```java
// Fragile, ugly, and grows with every new sensor type
if (sensor instanceof UltrasonicSensor) {
    ((UltrasonicSensor) sensor).read();
} else if (sensor instanceof TemperatureSensor) {
    ((TemperatureSensor) sensor).read();
} else if (sensor instanceof IRSensor) {
    ((IRSensor) sensor).read();
}
```

This code has a deep problem: every time you add a new sensor type, you must find every `if/else` chain in the codebase and add a new branch. You've coupled your polling logic to the complete list of sensor types. It's a maintenance guarantee of future pain.

Polymorphism is the solution. If every sensor extends `Sensor` (or implements a `Readable` interface), you can write:

```java
for (Sensor s : sensors) {
    s.read();  // Each sensor does the right thing automatically
}
```

This loop never changes. You can add 50 new sensor types and this code stays identical. The knowledge of what `read()` means for each type lives inside the type itself — exactly where it belongs.

## The Concept

Polymorphism in Java comes in two forms, which work at different points in time:

**1. Runtime polymorphism (dynamic dispatch / method overriding)**

This is the dominant form. When you call a method through a superclass reference, Java determines *at runtime* which overridden version to call based on the actual type of the object — not the declared type of the variable.

```java
Animal a = new Dog("Rex", 3, "Lab");
a.speak();  // Calls Dog's speak(), not Animal's — even though 'a' is declared as Animal
```

The JVM looks at what `a` actually points to (a `Dog`), finds `speak()` in `Dog`, and runs that. This is called **dynamic dispatch** — the method is dispatched to the right implementation dynamically, at runtime.

**2. Compile-time polymorphism (method overloading)**

Same method name, different parameter lists. The compiler resolves which version to call at compile time based on the argument types.

```java
public class Calculator {
    public int add(int a, int b) { return a + b; }
    public double add(double a, double b) { return a + b; }
    public String add(String a, String b) { return a + b; }
}
```

`calc.add(2, 3)` vs `calc.add(2.0, 3.0)` — the compiler picks the right version. No runtime decision needed. This is useful but less powerful than runtime polymorphism.

**The Liskov Substitution Principle (LSP)**

The formal statement of what polymorphism requires: if `S` is a subtype of `T`, then objects of type `T` may be replaced with objects of type `S` without altering the correctness of the program. In plain English: wherever you use an `Animal`, you should be able to substitute a `Dog` without breaking anything. A `Dog` must honor every promise `Animal` makes.

This is why "a stack shouldn't extend ArrayList" — `ArrayList.get(index)` makes promises a stack shouldn't keep. The substitution would break the program's expectations.

**Upcasting and downcasting**

When you store a `Dog` in an `Animal` variable, that's an **upcast** — you're treating the specific thing as the more general type. Always safe; no explicit cast needed.

```java
Animal a = new Dog("Rex", 3, "Lab");  // Upcast — implicit, always safe
```

When you go the other direction, that's a **downcast** — treating a general reference as a specific type. Requires an explicit cast, and fails at runtime if the object isn't actually that type:

```java
Dog d = (Dog) a;          // Downcast — only safe if 'a' actually points to a Dog
Cat c = (Cat) a;          // Throws ClassCastException at runtime — 'a' is a Dog!

if (a instanceof Dog) {   // Check before casting
    Dog d = (Dog) a;
}
// Modern Java (16+):
if (a instanceof Dog d) { // Pattern matching — check and cast in one step
    d.fetch();
}
```

Frequent downcasting is a signal that your design has a problem. If you find yourself constantly checking `instanceof` and casting down, you've probably not modeled your hierarchy correctly — or you need an interface or abstract method to handle the varying behavior.

## Intuition

Think of a remote control for a TV. The remote has a "power" button. It doesn't know or care whether it's controlling a Samsung, an LG, or a Sony. It sends the "power" signal. Each TV knows what to do when it receives that signal.

The remote is your code. The "power" button is `device.power()`. Each TV brand is a different subclass. You never need to open the remote and add a new button for every TV brand that exists — the interface is fixed. Only the TVs change.

For Baymax: your main control loop doesn't need to know whether it's commanding a wheel motor or an arm servo. It just calls `actuator.execute(command)`. Each actuator knows its own mechanics. The loop stays simple; complexity is pushed to where it belongs.

## Key Example

```java
// Superclass defines the shared interface
public class Shape {
    public double area() {
        return 0;  // Subclasses should override this
    }

    public String describe() {
        return "Shape with area: " + area();
    }
}

public class Circle extends Shape {
    private double radius;

    public Circle(double radius) { this.radius = radius; }

    @Override
    public double area() {
        return Math.PI * radius * radius;
    }
}

public class Rectangle extends Shape {
    private double width, height;

    public Rectangle(double width, double height) {
        this.width = width;
        this.height = height;
    }

    @Override
    public double area() {
        return width * height;
    }
}

public class Triangle extends Shape {
    private double base, height;

    public Triangle(double base, double height) {
        this.base = base;
        this.height = height;
    }

    @Override
    public double area() {
        return 0.5 * base * height;
    }
}

// Polymorphism in action — one loop handles all shape types
Shape[] shapes = {
    new Circle(5),
    new Rectangle(4, 6),
    new Triangle(3, 8)
};

for (Shape s : shapes) {
    System.out.println(s.describe());
    // Shape with area: 78.54...
    // Shape with area: 24.0
    // Shape with area: 12.0
}

double totalArea = 0;
for (Shape s : shapes) {
    totalArea += s.area();  // Each calls the right version — no instanceof needed
}
```

Python equivalent:
```python
import math

class Shape:
    def area(self):
        return 0

    def describe(self):
        return f"Shape with area: {self.area()}"

class Circle(Shape):
    def __init__(self, radius):
        self.radius = radius

    def area(self):
        return math.pi * self.radius ** 2

class Rectangle(Shape):
    def __init__(self, width, height):
        self.width = width
        self.height = height

    def area(self):
        return self.width * self.height

shapes = [Circle(5), Rectangle(4, 6)]
for s in shapes:
    print(s.describe())  # Each calls the right area()
```

Python note: Python also supports "duck typing" — if an object has a `area()` method, you can call it through that method without any formal inheritance relationship. Java requires a declared type relationship (class hierarchy or interface). For Baymax, Python's duck typing is convenient but means fewer compile-time guarantees.

## Worked Example

A Baymax motion system with multiple actuator types. The robot has wheel motors, a head servo, and an arm joint. The control loop shouldn't care which is which — it just sends commands.

```java
public class Actuator {
    protected String name;

    public Actuator(String name) { this.name = name; }

    public void execute(double value) {
        // Default — subclasses override
        System.out.println(name + " received command: " + value);
    }

    public String getName() { return name; }
}

public class WheelMotor extends Actuator {
    private double maxRPM;

    public WheelMotor(String name, double maxRPM) {
        super(name);
        this.maxRPM = maxRPM;
    }

    @Override
    public void execute(double speedPercent) {
        double rpm = (speedPercent / 100.0) * maxRPM;
        System.out.println(name + " spinning at " + rpm + " RPM");
    }
}

public class Servo extends Actuator {
    private double minAngle, maxAngle;

    public Servo(String name, double minAngle, double maxAngle) {
        super(name);
        this.minAngle = minAngle;
        this.maxAngle = maxAngle;
    }

    @Override
    public void execute(double angleDegrees) {
        double clamped = Math.max(minAngle, Math.min(maxAngle, angleDegrees));
        System.out.println(name + " moving to " + clamped + "°");
    }
}

// Control loop — completely unaware of actuator types
List<Actuator> actuators = new ArrayList<>();
actuators.add(new WheelMotor("left-wheel", 300));
actuators.add(new WheelMotor("right-wheel", 300));
actuators.add(new Servo("head-tilt", -45, 45));

for (Actuator a : actuators) {
    a.execute(50.0);  // Each does the right thing for its type
}
// left-wheel spinning at 150.0 RPM
// right-wheel spinning at 150.0 RPM
// head-tilt moving to 45.0°  (clamped from 50°)
```

Adding a new actuator type — say, a linear actuator for a gripper — requires zero changes to the control loop. You add one class, add it to the list, and the loop works.

## Gotchas

**Hiding vs overriding (static methods).** In Java, `static` methods cannot be overridden — they can only be hidden. If a subclass defines a static method with the same name as a superclass static method, calling it through a superclass reference calls the superclass version, not the subclass version. Dynamic dispatch only applies to instance methods.

```java
Animal a = new Dog("Rex", 3, "Lab");
a.speak();            // Dog's speak() — runtime dispatch, correct
Animal.staticDemo();  // Animal's version always — no dispatch
```

**Polymorphism requires inheritance or interface implementation.** You can't just call `s.read()` on objects of unrelated classes that both happen to have a `read()` method — unless they share a superclass or interface that declares `read()`. Java's type system requires a declared relationship. (This is where Python's duck typing differs — but Java's approach catches errors at compile time.)

**Accidental method hiding.** If a subclass method has the same name as a superclass method but different parameter types, it's not an override — it's an overload. The original method is still inherited and accessible. Use `@Override` to verify you're actually overriding.

**Polymorphism doesn't apply to fields.** Only methods are dynamically dispatched. If a superclass and subclass both define a field with the same name, you get two separate fields — and which one you access depends on the compile-time type of the reference, not the runtime type. This is almost always a bug. Never shadow fields in subclasses.

**`equals()` and polymorphism.** The `Object.equals()` method is inherited by everything. If you don't override it, equality checks reference identity (are these the same object in memory?), not value equality. Understanding that `==` checks reference, `.equals()` checks content (when properly overridden), and that polymorphism affects `.equals()` calls is fundamental to writing correct Java.

## See Also
- [[Inheritance]] — runtime polymorphism is only possible because of inheritance; the two concepts are deeply linked
- [[Interfaces and Abstract Classes]] — interfaces are the cleanest way to achieve polymorphism without the fragility of inheritance hierarchies
- [[Classes and Objects]] — you need objects before polymorphism means anything
- [[Encapsulation]] — polymorphism and encapsulation together are what make it possible to change implementations without changing callers
