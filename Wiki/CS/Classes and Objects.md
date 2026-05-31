# Classes and Objects

**One-liner:** A class is a blueprint that defines what data and behavior a type of thing has; an object is one actual instance created from that blueprint.

## Why It Exists

Before OOP became mainstream, most programs were written in a procedural style: a long list of instructions operating on raw data — arrays of integers, loose variables, structs. As programs grew, this broke down. You'd have a `customer_name` variable, a `customer_balance` variable, and a `process_payment()` function — all disconnected. Nothing in the language enforced that `process_payment()` should only touch customer data. Any function could reach in and corrupt anything.

The core problem: **data and the code that acts on it lived in different places.** When code is scattered, bugs are scattered too. When a program has 10 customers, manual management works. At 10,000 customers, it collapses.

OOP solved this by bundling data and behavior together into a single unit — the object. The language itself now enforces the relationship. You don't just have a name and a balance floating around; you have a `Customer` object that *owns* its name, owns its balance, and exposes only the operations that make sense on a customer.

## The Concept

A **class** is a template or blueprint. It defines:
1. **Fields (instance variables):** what data each object of this type will hold
2. **Methods:** what operations objects of this type can perform
3. **Constructors:** how to create and initialize a new object

An **object** is a concrete instance created from a class at runtime. Every object has its own copy of the fields defined in its class. Methods are shared (not duplicated per object), but operate on each object's individual data.

The metaphor that actually holds up under scrutiny: a class is like the architectural drawings for a house. The drawings define rooms, doors, wiring. An object is the actual house built from those drawings. You can build many houses from the same blueprint — each house is distinct (different furniture, different paint), but all have the same structure. The blueprint itself is not a house.

**Memory model:** When Java runs `new Customer("Alice", 500)`, it allocates memory for a new Customer object on the heap, runs the constructor to initialize the fields, and returns a reference (pointer) to that memory. The variable `alice` holds the reference, not the object itself. This matters when you do `Customer bob = alice;` — now both variables point to the same object.

**Static vs instance members:** Fields and methods declared `static` belong to the class itself, not to any particular instance. `Math.PI` is a static field — there's only one copy. A customer's balance is an instance field — each customer has its own.

## Intuition

Think of a cookie cutter and cookies. The cookie cutter is the class: it defines the shape. Each cookie you press out is an object: it has the shape defined by the cutter, but its own physical existence. You can frost one cookie without affecting any other. You can have hundreds of cookies from one cutter.

Now push the analogy further: imagine the cookie cutter also defined *what you can do with a cookie* — the cookie cutter blueprint includes instructions like "to eat this cookie, break it in half first." That's the class defining behavior alongside structure.

## Key Example

```java
// The class — the blueprint
public class Dog {
    // Instance fields — each Dog gets its own copy
    String name;
    String breed;
    int age;

    // Constructor — how to build a Dog object
    public Dog(String name, String breed, int age) {
        this.name = name;
        this.breed = breed;
        this.age = age;
    }

    // Instance method — behavior that uses this object's data
    public void bark() {
        System.out.println(name + " says: Woof!");
    }

    public String describe() {
        return name + " is a " + age + "-year-old " + breed;
    }
}

// Using the class to create objects
public class Main {
    public static void main(String[] args) {
        Dog rex = new Dog("Rex", "Labrador", 3);
        Dog luna = new Dog("Luna", "Poodle", 5);

        rex.bark();   // Rex says: Woof!
        luna.bark();  // Luna says: Woof!

        System.out.println(rex.describe());
        // Rex is a 3-year-old Labrador
    }
}
```

Python equivalent (relevant since Baymax uses Python):
```python
class Dog:
    def __init__(self, name, breed, age):
        self.name = name
        self.breed = breed
        self.age = age

    def bark(self):
        print(f"{self.name} says: Woof!")

    def describe(self):
        return f"{self.name} is a {self.age}-year-old {self.breed}"

rex = Dog("Rex", "Labrador", 3)
luna = Dog("Luna", "Poodle", 5)

rex.bark()     # Rex says: Woof!
print(rex.describe())
```

Key difference: Python's `self` is explicit in every method signature; Java's `this` is implicit (available but not required in the signature). In Java, every instance method has access to `this` automatically.

## Worked Example

Designing a robot sensor for Baymax. A robot often has multiple sensors — ultrasonic, IR, camera. Without OOP, you'd juggle parallel arrays: `sensor_names[]`, `sensor_readings[]`, `sensor_pins[]`. With OOP:

```java
public class UltrasonicSensor {
    private String label;
    private int trigPin;
    private int echoPin;
    private double lastReading;  // in cm

    public UltrasonicSensor(String label, int trigPin, int echoPin) {
        this.label = label;
        this.trigPin = trigPin;
        this.echoPin = echoPin;
        this.lastReading = 0.0;
    }

    public double measure() {
        // In real code: send pulse, time return, convert to cm
        // Simulated here:
        this.lastReading = 42.0;
        return lastReading;
    }

    public String getStatus() {
        return label + ": " + lastReading + " cm";
    }
}

// Now you can have a list of sensors — all managed the same way
UltrasonicSensor front = new UltrasonicSensor("front", 4, 5);
UltrasonicSensor rear  = new UltrasonicSensor("rear",  6, 7);

front.measure();
System.out.println(front.getStatus());  // front: 42.0 cm
```

Each sensor is self-contained. You can add ten more sensors without changing any existing code — just create more objects.

## Gotchas

**Confusing the class with the object.** Writing `Dog.bark()` when you mean `rex.bark()`. The class doesn't bark; the individual dog does.

**Null reference errors.** `Dog rex;` declares a variable but creates no object — `rex` is null. Calling `rex.bark()` crashes. You must use `new` to actually create the object.

**`this` confusion.** When a constructor parameter has the same name as a field (`name`), you must write `this.name = name` to distinguish them. `name = name` does nothing useful.

**Static vs instance misuse.** Accessing a non-static field through the class name (`Dog.name`) is a compile error. Static fields are class-wide; instance fields belong to individual objects.

**In Python vs Java:** Python classes have no access modifiers by default — nothing is truly `private`. The convention is leading underscores (`_name`, `__name`). Java enforces access at compile time. This matters when you port Baymax concepts to a Java course: what Python allowed by convention, Java enforces by syntax.

## See Also
- [[Encapsulation]] — the principle that determines what fields and methods should be public vs private
- [[Inheritance]] — how one class can build on another class's blueprint
- [[Polymorphism]] — how objects of different classes can be used interchangeably through a shared interface
- [[Interfaces and Abstract Classes]] — how to define partial blueprints and contracts between classes
