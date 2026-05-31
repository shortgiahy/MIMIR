# Inheritance

**One-liner:** Inheritance lets a new class automatically acquire the fields and methods of an existing class, so you can extend or specialize behavior without rewriting it from scratch.

## Why It Exists

Suppose you're building a simulation with animals. You write a `Dog` class with `name`, `age`, `eat()`, and `sleep()`. Then you need a `Cat` class — same fields, same `eat()` and `sleep()`, just a different `speak()`. Without inheritance, you copy-paste. Now you have two classes with identical code. When you find a bug in `eat()`, you fix it in two places. Later you add `Bird` — three places. You've created a maintenance trap.

The deeper problem isn't just duplication; it's that `Dog`, `Cat`, and `Bird` share a logical relationship — they're all `Animal`. The code should reflect that reality. When the codebase doesn't match the conceptual structure of the domain, it becomes harder to reason about and harder to change.

Inheritance addresses both problems: it eliminates duplicate code by putting shared behavior in one place, and it encodes the "is-a" relationship between types — a relationship the compiler can then reason about and enforce.

## The Concept

In Java, one class **extends** another using the `extends` keyword. The class being extended is the **superclass** (also called parent class or base class). The extending class is the **subclass** (child class or derived class).

The subclass automatically inherits:
- All non-private fields from the superclass
- All non-private methods from the superclass
- The ability to call the superclass constructor with `super(...)`

The subclass can then:
- **Add** new fields and methods (extension)
- **Override** inherited methods to change their behavior (specialization)
- **Call** the superclass version of an overridden method using `super.methodName()`

**The is-a relationship is the test.** Before using inheritance, ask: "Is a `Dog` truly an `Animal`?" If yes, inheritance fits. "Is a `Dog` truly a `Vehicle`?" Obviously no — don't use inheritance just to borrow a few methods.

**The inheritance chain:** Every class in Java implicitly extends `Object`. So `Dog extends Animal extends Object`. The full chain means every object has methods like `toString()`, `equals()`, and `hashCode()` inherited from `Object` — even if you never explicitly define them.

**Constructors are not inherited.** You must explicitly call the superclass constructor from the subclass constructor using `super(...)`. If you don't, Java automatically inserts a call to the no-argument superclass constructor — if that constructor doesn't exist, you get a compile error.

**Method overriding vs method overloading:** These sound similar but are completely different.
- **Overriding:** Redefining an inherited method in a subclass with the same signature. This is runtime polymorphism.
- **Overloading:** Defining multiple methods with the same name but different parameter lists, in the same class. This is compile-time resolution.

Always annotate overriding methods with `@Override`. It's not required, but it tells the compiler "I intend this to override a superclass method." If you misspell the method name, the compiler catches it instead of silently creating a new method.

**Access in inheritance:** `private` fields are inherited (they exist in the object), but subclasses cannot access them directly — they must go through public or protected methods. `protected` was specifically designed for this: visible to subclasses but hidden from the rest of the world.

## Intuition

Think of a company org chart. There's a generic "Employee" role with common attributes: name, ID, `getPaycheck()`. Then there are specific roles: `Manager` is-a Employee, `Engineer` is-a Employee. Each has everything an Employee has, plus role-specific behavior. A Manager additionally has a `teamSize` and `conductReview()`. You don't restate what an Employee is every time — you extend it.

Now push the analogy: if you discover that all employees need a `getHealthBenefits()` method, you add it to `Employee` once and every subclass inherits it immediately. This is the practical power of inheritance — change in one place, effect everywhere below.

## Key Example

```java
// Superclass — the shared foundation
public class Animal {
    protected String name;
    protected int age;

    public Animal(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public void eat() {
        System.out.println(name + " is eating.");
    }

    public void sleep() {
        System.out.println(name + " is sleeping.");
    }

    // Meant to be overridden — but has a default
    public void speak() {
        System.out.println(name + " makes a sound.");
    }

    public String describe() {
        return name + ", age " + age;
    }
}

// Subclass — inherits everything from Animal, adds and overrides
public class Dog extends Animal {
    private String breed;

    public Dog(String name, int age, String breed) {
        super(name, age);  // calls Animal's constructor
        this.breed = breed;
    }

    @Override
    public void speak() {
        System.out.println(name + " barks: Woof!");
    }

    // New method, specific to Dog
    public void fetch() {
        System.out.println(name + " fetches the ball!");
    }

    @Override
    public String describe() {
        return super.describe() + ", breed: " + breed;
    }
}

// Usage
Dog rex = new Dog("Rex", 3, "Labrador");
rex.eat();       // Inherited from Animal: Rex is eating.
rex.speak();     // Overridden: Rex barks: Woof!
rex.fetch();     // Dog-specific: Rex fetches the ball!
System.out.println(rex.describe());  // Rex, age 3, breed: Labrador
```

Python equivalent:
```python
class Animal:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def eat(self):
        print(f"{self.name} is eating.")

    def speak(self):
        print(f"{self.name} makes a sound.")

class Dog(Animal):
    def __init__(self, name, age, breed):
        super().__init__(name, age)  # calls Animal.__init__
        self.breed = breed

    def speak(self):  # overrides Animal.speak
        print(f"{self.name} barks: Woof!")

    def fetch(self):
        print(f"{self.name} fetches the ball!")

rex = Dog("Rex", 3, "Labrador")
rex.eat()    # Rex is eating.
rex.speak()  # Rex barks: Woof!
```

Key Python difference: Python uses `super().__init__(...)` where Java uses `super(...)`. Python also supports multiple inheritance (a class can extend multiple parent classes), which Java deliberately does not allow for classes — Java sidesteps the resulting complexity by only allowing multiple interface implementation.

## Worked Example

Designing the sensor hierarchy for Baymax. All sensors share some behavior: they have a label, a last reading, and a method to `read()` data. But each sensor type works differently internally.

```java
public class Sensor {
    protected String label;
    protected double lastReading;

    public Sensor(String label) {
        this.label = label;
        this.lastReading = 0.0;
    }

    // Template for all sensors — subclasses must provide the implementation
    public double read() {
        // Default does nothing; subclasses override this
        return lastReading;
    }

    public String getStatus() {
        return label + ": " + lastReading;
    }
}

public class UltrasonicSensor extends Sensor {
    private int trigPin;
    private int echoPin;

    public UltrasonicSensor(String label, int trigPin, int echoPin) {
        super(label);
        this.trigPin = trigPin;
        this.echoPin = echoPin;
    }

    @Override
    public double read() {
        // In real hardware: send pulse, time echo, convert to cm
        lastReading = simulateMeasurementCm();
        return lastReading;
    }

    private double simulateMeasurementCm() { return 42.0; }
}

public class TemperatureSensor extends Sensor {
    private int pin;

    public TemperatureSensor(String label, int pin) {
        super(label);
        this.pin = pin;
    }

    @Override
    public double read() {
        // Read voltage, convert to Celsius
        lastReading = simulateTempReading();
        return lastReading;
    }

    private double simulateTempReading() { return 22.5; }
}
```

Now you can put all sensors in one list and call `read()` on all of them uniformly — this is [[Polymorphism]] in action. The inheritance hierarchy reflects reality: every sensor is a Sensor, but each has its own way of reading data.

## Gotchas

**Inheriting when you should be composing.** The classic mistake: `Stack extends ArrayList` because "a stack uses a list internally." But a stack is NOT an ArrayList — it shouldn't expose `add(index, element)` or `remove(index)`. The is-a test fails. The right design is composition: `Stack` *has-a* `ArrayList` as a private field. Inheritance for code reuse without a real is-a relationship creates classes that expose the wrong interface and break when the parent changes.

**Tight coupling to the superclass.** Subclasses are tightly bound to their parent. When a superclass changes, all subclasses are affected — sometimes in subtle, breaking ways. This is called the "fragile base class problem." A change in `Animal` that seems safe might break `Dog`, `Cat`, and `Bird` simultaneously.

**Overriding without `@Override`.** If you typo the method name — `pubic void Speak()` instead of `speak()` — Java silently creates a new method instead of overriding the parent. The `@Override` annotation catches this at compile time. Always use it.

**Calling overridden methods from a constructor.** If a superclass constructor calls a method that a subclass overrides, the subclass version runs before the subclass constructor has finished. The subclass fields aren't initialized yet. This leads to subtle null pointer bugs that are very hard to trace.

**Inheritance depth.** Deep inheritance chains (A → B → C → D → E) become hard to reason about. Where does a given method actually come from? Which level overrides which? Keep hierarchies shallow — prefer 2–3 levels maximum for most cases.

## See Also
- [[Classes and Objects]] — you must understand what a class is before extending one
- [[Encapsulation]] — `protected` access exists specifically for inheritance, and it represents a real encapsulation tradeoff
- [[Polymorphism]] — inheritance is the mechanism that makes runtime polymorphism possible; they're deeply linked
- [[Interfaces and Abstract Classes]] — abstract classes are a form of inheritance with forced overriding; interfaces avoid inheritance's fragile coupling entirely
