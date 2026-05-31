# Object

**One-liner:** An object is a live, in-memory instance of a class — it has its own copy of every instance field and can receive method calls.

## Core Idea
When you write `new ClassName(...)` in Java, the JVM allocates memory on the **heap** for that object's fields and returns a **reference** (a pointer) stored on the **stack** (or in another heap object). The object lives on the heap; the variable that names it lives on the stack.

```java
ProximitySensor front = new ProximitySensor("front");
//              ^^^^^   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
//         stack variable        heap allocation
```

## Why It Exists
Procedural programs treat data as values floating between functions. Objects bundle state with behavior and give that bundle an **identity** that persists across multiple method calls. Two calls to `front.update(...)` modify the *same* physical block of memory — that's what makes stateful modeling possible.

## Real-World Applications
- Every Android UI widget (`Button`, `TextView`) is a heap object.
- PyTorch: `model = MyNet()` creates a heap object holding all weight tensors.
- ROS 2: every running node is a `Node` object alive for the duration of the process.
- Baymax: each physical sensor corresponds to one heap object; its latest reading is state persisted between loop iterations.

## Intuition
A class is a floor plan; an object is the actual house. Two houses built from the same floor plan are independent — painting one doesn't affect the other. Each has its own rooms (fields) filled with its own furniture (values).

## Deep Dive
**Heap vs. Stack**

| | Stack | Heap |
|---|---|---|
| Holds | local variables, method frames, primitive values | objects (instances) |
| Size | small, fixed at thread creation | large, grows dynamically |
| Lifetime | frame popped when method returns | until garbage collected (no references remain) |
| Speed | very fast (pointer bump) | slower (GC overhead) |

```java
void example() {
    int x = 42;                           // x lives on the stack — it IS the value
    ProximitySensor s = new ProximitySensor("front"); // s is a stack reference;
                                          // the object lives on the heap
}
// When example() returns: x and s (the reference) vanish
// The ProximitySensor object on the heap may be GC'd once no references remain
```

**Object identity vs. equality**
```java
ProximitySensor a = new ProximitySensor("front");
ProximitySensor b = new ProximitySensor("front");
ProximitySensor c = a;

a == b    // false — different heap addresses (different objects)
a == c    // true  — same heap address (same object, two references)
a.equals(b) // true IF equals() is overridden to compare state
```

**Python**
Python hides the reference/value distinction less than Java but the heap model is the same. Everything is an object — even `int`. `is` tests identity (same heap address); `==` tests equality.

```python
a = ProximitySensor("front")
b = ProximitySensor("front")
c = a

a is b   # False
a is c   # True
a == b   # depends on __eq__
```

**Null / None**
A stack variable can hold a null reference — it points to no object. Calling a method on `null` raises `NullPointerException` in Java. This is one of the most common runtime bugs. In Baymax code, always initialize sensor objects before the control loop.

## Worked Example
```java
// Baymax context: a robot frame holds multiple sensor objects
public class RobotFrame {
    private ProximitySensor frontSensor;
    private ProximitySensor leftSensor;
    private ProximitySensor rightSensor;

    public RobotFrame() {
        // Three separate heap objects created
        frontSensor = new ProximitySensor("front");
        leftSensor  = new ProximitySensor("left");
        rightSensor = new ProximitySensor("right");
    }

    public ProximitySensor getSensor(String location) {
        // Returns the reference — caller gets a handle to the same heap object
        return switch (location) {
            case "front" -> frontSensor;
            case "left"  -> leftSensor;
            case "right" -> rightSensor;
            default -> throw new IllegalArgumentException("Unknown location: " + location);
        };
    }
}

RobotFrame robot = new RobotFrame();
ProximitySensor s = robot.getSensor("front");  // s and robot.frontSensor point to same object
s.update(8.0);  // modifies the heap object — visible through robot too
```

## See Also
- [[Class]] — the blueprint that defines every object
- [[Instance]] — synonym for object, emphasizes instantiation relationship
- [[Constructor]] — the code that runs to initialize an object at `new`
- [[Encapsulation]] — the principle that keeps an object's state private
- [[Inheritance]] — objects of a subclass type have extra fields/methods layered on
