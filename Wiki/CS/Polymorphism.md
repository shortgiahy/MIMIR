# Polymorphism

**One-liner:** Polymorphism means one interface, many implementations — the same method call produces different behavior depending on which actual object receives it.

## Core Idea
"Polymorphism" is Greek for "many forms." In OOP, it means you can write code that works on a general type (`Sensor`) and it will automatically do the right thing for any specific subtype (`UltrasonicSensor`, `IRSensor`, `IMUSensor`) without you writing `if (type == ultrasonic)` checks.

```java
// One loop — handles all sensor types correctly
List<Sensor> sensors = getSensors();
for (Sensor s : sensors) {
    s.poll();               // calls the right poll() for each subtype
    log(s.statusReport());  // calls the right statusReport() for each subtype
}
```

The caller doesn't know or care which subtype it has. The object itself knows — and [[Dynamic Dispatch]] routes the call to the correct implementation.

## Why It Exists
Without polymorphism, you write explicit type-checking code:
```java
// Without polymorphism — fragile, O(n) maintenance
if (sensor instanceof UltrasonicSensor u) {
    u.pollUltrasonic();
} else if (sensor instanceof IRSensor ir) {
    ir.pollInfrared();
} else if (sensor instanceof IMUSensor imu) {
    imu.pollIMU();
}
// Add a new sensor type → have to find and update EVERY if-chain
```

With polymorphism, adding a new sensor type means writing the new class and implementing `poll()`. Zero changes to the control loop. This is the **Open/Closed Principle**: code is open for extension, closed for modification.

## Real-World Applications
- Java collections: `List<Shape>` holding `Circle`, `Rectangle`, `Triangle` — iterate and call `area()` uniformly.
- GUI frameworks: `Component.paint()` — the framework calls it on every component; each type knows how to draw itself.
- Python ML: scikit-learn's `fit()`/`predict()` interface — `LogisticRegression`, `RandomForest`, `SVM` all implement the same API. You can swap models without changing training code.
- PyTorch: `nn.Module.forward()` — every layer type implements it differently; `model(x)` routes correctly.
- Baymax: `Actuator.execute(Command)` — `Motor`, `Servo`, `LED` all implement it; the command dispatcher doesn't know which hardware it's talking to.

## Intuition
A TV remote is polymorphic. Press "volume up" and the TV raises its volume, a soundbar raises its volume, a Bluetooth speaker raises its volume. Same button (same method call), different devices (different implementations). The remote doesn't have a Samsung mode and a Sony mode. It speaks the same language; each device interprets it correctly.

## Deep Dive
**Three kinds of polymorphism in Java**

1. **Subtype polymorphism** (runtime, via [[Inheritance]] + [[Method Overriding]]) — what most people mean when they say "polymorphism."
2. **Interface polymorphism** (runtime, via [[Interface]]) — same as subtype but without requiring a shared ancestor class.
3. **Parametric polymorphism** (compile-time, via Generics) — `List<T>` works for any `T` without rewriting the class.

**Subtype polymorphism**
```java
// Reference type = Sensor (supertype); actual object = UltrasonicSensor (subtype)
Sensor s = new UltrasonicSensor("front", 4.0);
s.poll();   // Dynamic dispatch: UltrasonicSensor.poll() is called
```

**Interface polymorphism**
```java
public interface Pollable { void poll(); }
public class UltrasonicSensor implements Pollable { @Override public void poll() {...} }
public class NetworkFeed implements Pollable { @Override public void poll() {...} }

List<Pollable> sources = List.of(new UltrasonicSensor(...), new NetworkFeed(...));
sources.forEach(Pollable::poll);  // no if-chains needed
```

**Compile-time vs. runtime polymorphism**
- Method **overloading** (same method name, different param types in the same class) is resolved at **compile time** — not "true" polymorphism in the OOP sense.
- Method **overriding** is resolved at **runtime** — true polymorphism.

**The polymorphism + strategy design pattern**
Polymorphism directly enables the Strategy pattern: encapsulate algorithms (navigation strategy, path planning, obstacle avoidance) behind an interface. Swap strategies at runtime without changing the robot's drive loop. See [[Composition over Inheritance]].

**Python — duck typing**
Python achieves polymorphism through duck typing: if an object has the required method, it's compatible. No inheritance required.

```python
# These classes share NO common ancestor — but both have .poll()
class UltrasonicSensor:
    def poll(self): return self._read_hardware()

class MockSensor:
    def poll(self): return 1.5   # fake distance for testing

# Works with both — polymorphic because both "quack like a Sensor"
def run_sensor_loop(sensors: list):
    for sensor in sensors:
        sensor.poll()   # duck-typed: calls whatever .poll() the object has

run_sensor_loop([UltrasonicSensor(), MockSensor()])
```

In Python robotics and ML, duck typing is used heavily. `gymnasium` (RL environments), `stable-baselines3` (RL agents), and `scikit-learn` rely on duck typing for their polymorphic APIs.

**The Baymax connection**
Baymax's reinforcement learning agent uses polymorphism: the `Policy` (see [[What is Reinforcement Learning]] and [[Markov Decision Processes]]) is an interface that many implementations satisfy — a random policy, a trained neural network policy, a rule-based policy. The training loop calls `policy.select_action(state)` without knowing which policy type it's running.

## Worked Example
```java
// Baymax context: heterogeneous actuator list — all driven through one interface

public interface Actuatable {
    void execute(double command);
    String getName();
}

public class DriveMotor implements Actuatable {
    private final String wheelPos;
    public DriveMotor(String wheelPos) { this.wheelPos = wheelPos; }

    @Override public void execute(double command) {
        System.out.printf("Motor %s: speed=%.1f RPM%n", wheelPos, command);
    }
    @Override public String getName() { return "Motor[" + wheelPos + "]"; }
}

public class ServoJoint implements Actuatable {
    private final String jointName;
    public ServoJoint(String jointName) { this.jointName = jointName; }

    @Override public void execute(double command) {
        System.out.printf("Servo %s: angle=%.0f°%n", jointName, command);
    }
    @Override public String getName() { return "Servo[" + jointName + "]"; }
}

public class LED implements Actuatable {
    private final String ledId;
    public LED(String ledId) { this.ledId = ledId; }

    @Override public void execute(double command) {
        System.out.printf("LED %s: brightness=%.0f%%%n", ledId, command * 100);
    }
    @Override public String getName() { return "LED[" + ledId + "]"; }
}

// Control loop — never changes when we add new actuator types
List<Actuatable> actuators = List.of(
    new DriveMotor("FL"), new DriveMotor("FR"),
    new ServoJoint("elbow"), new LED("status")
);

// Send "idle" command to all actuators — polymorphism handles the rest
for (Actuatable a : actuators) {
    a.execute(0.0);
}
```

## See Also
- [[Method Overriding]] — the primary mechanism of subtype polymorphism
- [[Dynamic Dispatch]] — how Java routes the method call at runtime
- [[Interface]] — enables polymorphism without inheritance
- [[Abstract Class]] — enforces that subclasses implement the overrideable methods
- [[Upcasting and Downcasting]] — how you move between the general and specific types
- [[Inheritance]] — the prerequisite for subtype polymorphism
- [[Composition over Inheritance]] — polymorphism via interfaces, without deep inheritance trees
- [[What is Reinforcement Learning]] — RL agent/policy interfaces are a real-world polymorphism example
- [[Markov Decision Processes]] — the Policy concept in MDPs is directly analogous to an interface
- [[Policy]] — swapping a random policy for a trained neural-network policy without changing the agent loop is polymorphism in action; the loop calls `select_action(state)` and the right implementation runs
- [[Activation Function]] — ReLU, sigmoid, and tanh all implement the same "activation" interface; the network layer is polymorphic over whichever activation it holds
