# Composition over Inheritance

**One-liner:** Instead of building new types by extending existing ones (is-a), build them by holding references to collaborating objects (has-a) — this produces more flexible, less brittle code.

## Core Idea
**Inheritance** says: "A `Dog` is an `Animal`." The `Dog` class gets `Animal`'s behavior automatically by extending it.

**Composition** says: "A `Robot` has a `DriveSystem`, has a `SensorArray`, has a `BatteryMonitor`." The `Robot` class holds references to these collaborators and delegates work to them.

```java
// Inheritance approach — tight coupling, hard to vary independently
public class Robot extends Chassis {
    // Robot IS-A Chassis — locked in at compile time
}

// Composition approach — flexible, each piece swappable
public class Robot {
    private final DriveSystem  drive;    // HAS-A DriveSystem
    private final SensorArray  sensors;  // HAS-A SensorArray
    private final BatteryMonitor battery; // HAS-A BatteryMonitor

    public Robot(DriveSystem drive, SensorArray sensors, BatteryMonitor battery) {
        this.drive   = drive;
        this.sensors = sensors;
        this.battery = battery;
    }
}
```

## Why It Exists
**The fragile base class problem:** When you inherit, the subclass is tightly coupled to every implementation detail of the superclass. A change to the superclass — even a "safe" one — can silently break subclasses in ways the superclass author never anticipated. The subclass has no way to protect itself.

**The Stack-extends-ArrayList antipattern (classic example):**
```java
// Java's actual historical mistake (java.util.Stack extends java.util.Vector):
public class Stack<E> extends ArrayList<E> {
    public void push(E item) { add(item); }
    public E pop()           { return remove(size() - 1); }
    public E peek()          { return get(size() - 1); }
}

// This is broken because Stack inherits the entire ArrayList API:
Stack<String> stack = new Stack<>();
stack.push("a");
stack.push("b");
stack.add(0, "c");   // WRONG: ArrayList.add(index, element) bypasses stack discipline
stack.remove(0);     // WRONG: directly removes from middle, not the top
stack.get(1);        // WRONG: stacks should not allow random access

// Users can violate the stack contract because all ArrayList methods are exposed.
// There is NO WAY to hide them without breaking the inheritance contract.
```

The fix is composition:
```java
// Stack HAS-A list — only exposes what a stack should expose
public class Stack<E> {
    private final ArrayList<E> storage = new ArrayList<>();   // private implementation detail

    public void push(E item) { storage.add(item); }
    public E pop()           { return storage.remove(storage.size() - 1); }
    public E peek()          { return storage.get(storage.size() - 1); }
    public boolean isEmpty() { return storage.isEmpty(); }
    public int size()        { return storage.size(); }
    // add(0, element)? Not exposed. Random access? Not exposed. Clean contract.
}
```

## Real-World Applications
- **Java collections** (`ArrayDeque` wraps an array rather than extending one): composition hides the raw array.
- **Strategy pattern in frameworks**: Spring's `JdbcTemplate` HAS-A `DataSource`. Swap the `DataSource` implementation (H2 in tests, Postgres in production) without changing `JdbcTemplate`.
- **PyTorch**: `nn.Sequential` has a list of `nn.Module` layers — composition, not inheritance. Your custom `nn.Module` HAS-A set of sub-modules (linear layers, activation functions). The hierarchy is deep in the composition graph, not the inheritance tree.
- **ROS 2**: a robot node HAS-A publisher, HAS-A subscriber, HAS-A timer — all composed in, not inherited.
- **Baymax**: `Robot` HAS-A `DrivePolicy`, HAS-A `SensorFusion`, HAS-A `SafetyMonitor`. Swapping the `DrivePolicy` from a hand-coded PID to a trained neural network policy requires zero changes to `Robot` itself.

## Intuition
**Inheritance is welding; composition is bolting.**

Welding two pieces of metal permanently joins them — strong, but impossible to change the relationship. Bolting them together means you can swap one piece out, adjust the fit, or use the same piece in a different assembly. The bolted structure is more flexible, easier to test in isolation, and easier to evolve.

A `Robot` should not inherit from `DriveSystem` because the drive system is not what a robot *is* — it is something a robot *uses*. Welding them together means you cannot test the robot's decision logic with a mock drive system, and cannot upgrade the drive system without potentially breaking the robot's other behaviors.

## Deep Dive
**Why composition is more testable**

Inheritance-based design:
```java
public class Robot extends DriveSystem {
    // Cannot test Robot without also testing DriveSystem's hardware
    // Cannot substitute a fake drive system in unit tests
}
```

Composition-based design:
```java
public interface DriveSystem {
    void setWheelSpeeds(double left, double right);
}

public class Robot {
    private final DriveSystem drive;
    public Robot(DriveSystem drive) { this.drive = drive; }
    // In production: new Robot(new RealDriveSystem())
    // In tests:      new Robot(new MockDriveSystem())
}
```

The test can verify `Robot`'s logic by injecting a `MockDriveSystem` that records what speed commands the robot issued. No hardware required. This is **dependency injection** — the dependency is composed in from outside.

**The Strategy pattern — composition's killer app**
The Strategy pattern (one of the Gang of Four patterns) is pure composition: an algorithm is encapsulated in an object behind an interface, then composed into the class that needs it.

```java
// Strategy interface
public interface ObstacleAvoidanceStrategy {
    double[] computeSteering(SensorState state);
}

// Strategies — completely independent implementations
public class PotentialFieldStrategy implements ObstacleAvoidanceStrategy {
    @Override public double[] computeSteering(SensorState state) {
        // repulsion/attraction field math
        return new double[]{0.5, 0.3};
    }
}

public class DWAStrategy implements ObstacleAvoidanceStrategy {
    @Override public double[] computeSteering(SensorState state) {
        // Dynamic Window Approach
        return new double[]{0.4, 0.4};
    }
}

// Robot composes a strategy — swappable at construction time or even at runtime
public class NavigationController {
    private ObstacleAvoidanceStrategy avoidance;

    public NavigationController(ObstacleAvoidanceStrategy avoidance) {
        this.avoidance = avoidance;
    }

    // Swap strategy at runtime — inheritance cannot do this
    public void setStrategy(ObstacleAvoidanceStrategy avoidance) {
        this.avoidance = avoidance;
    }

    public void navigate(SensorState state) {
        double[] steering = avoidance.computeSteering(state);
        applyWheelSpeeds(steering);
    }
}

// Clean: swap from hand-coded to ML-based avoidance:
NavigationController nav = new NavigationController(new PotentialFieldStrategy());
// Later in the mission, upgrade:
nav.setStrategy(new DWAStrategy());
```

**When inheritance IS correct**
Composition over inheritance is a guideline, not an absolute law. Inheritance is appropriate when:
1. The is-a relationship is genuinely stable and semantically true.
2. The subclass is clearly a specialization of the superclass (an `UltrasonicSensor` IS genuinely a `Sensor`).
3. You want to inherit and reuse concrete behavior from a well-encapsulated base class.
4. The `Liskov Substitution Principle` holds: any code expecting the superclass can work correctly with the subclass.

The sensor hierarchy (`Sensor → RangeSensor → UltrasonicSensor`) is a valid use of inheritance. `Stack extends ArrayList` is not.

**Composition in Python**
Python makes composition especially natural — no special syntax, just assign objects as instance attributes.

```python
class Robot:
    def __init__(
        self,
        drive: DriveSystem,
        sensors: SensorArray,
        policy: DrivePolicy,
    ) -> None:
        self.drive   = drive
        self.sensors = sensors
        self.policy  = policy   # could be PID, neural net, or mock

    def step(self) -> None:
        state  = self.sensors.read()
        action = self.policy.select_action(state)  # polymorphic call
        self.drive.apply(action)

# Swap the policy without touching Robot:
robot = Robot(
    drive=RealDriveSystem(),
    sensors=SensorArray(),
    policy=TrainedNeuralNetPolicy(weights_path="model.pt"),
)
```

In ML, this pattern is ubiquitous: `stable-baselines3` agents have a composed policy network, environment, and optimizer — not an inheritance tree.

## Worked Example
```java
// Baymax: composition-based robot brain vs. what inheritance would look like

// === INHERITANCE APPROACH (fragile) ===
// public class BaymaxRobot extends DriveSystem
//                           extends SensorFusion   ← Java can't do this anyway
//                           extends BatteryMonitor
//                           extends SafetyMonitor  ← rapidly becomes incoherent

// === COMPOSITION APPROACH (flexible) ===

public interface DrivePolicy {
    double[] computeWheelSpeeds(RobotState state);
}

public interface SafetyMonitor {
    boolean isEmergencyStop(RobotState state);
}

public class BaymaxRobot {
    private final DrivePolicy    policy;
    private final SensorFusion   sensors;
    private final SafetyMonitor  safety;
    private final BatteryMonitor battery;

    // All dependencies injected — none hardcoded
    public BaymaxRobot(
            DrivePolicy policy,
            SensorFusion sensors,
            SafetyMonitor safety,
            BatteryMonitor battery) {
        this.policy  = policy;
        this.sensors = sensors;
        this.safety  = safety;
        this.battery = battery;
    }

    public void runControlCycle() {
        RobotState state = sensors.fuse();

        if (safety.isEmergencyStop(state)) {
            emergencyStop();
            return;
        }

        if (battery.isCritical()) {
            seekChargingStation();
            return;
        }

        double[] speeds = policy.computeWheelSpeeds(state);
        applyWheelSpeeds(speeds);
    }

    private void applyWheelSpeeds(double[] speeds) { /* hardware write */ }
    private void emergencyStop() { /* halt all actuators */ }
    private void seekChargingStation() { /* navigate to dock */ }
}

// Wiring at startup — completely external to BaymaxRobot
BaymaxRobot baymax = new BaymaxRobot(
    new NeuralNetDrivePolicy("weights/model_v3.pt"),
    new SensorFusion(List.of(
        new UltrasonicSensor("front"), new IMUSensor("body"))),
    new ProximityEmergencyStop(0.15),   // stop if anything < 15 cm
    new BatteryMonitor(battery)
);

// To switch from neural net to PID — change one line, zero changes to BaymaxRobot:
BaymaxRobot testBot = new BaymaxRobot(
    new PIDDrivePolicy(1.2, 0.05, 0.3),  // ← only this changed
    new SensorFusion(List.of(new MockSensor())),
    new AlwaysSafety(),
    new FullBattery()
);
```

`BaymaxRobot` is closed to modification but open to extension — new policies, new safety monitors, new sensor fusions, none require touching the robot's control loop.

## See Also
- [[Interface]] — defines the seams at which composition connects components
- [[Abstract Class]] — valid base for the `is-a` cases where inheritance IS appropriate
- [[Inheritance]] — the alternative; use when the is-a relationship is genuine and stable
- [[Polymorphism]] — composition via interfaces gives you polymorphism without inheritance
- [[Encapsulation]] — composed components are independently encapsulated; callers see only the interface
- [[Upcasting and Downcasting]] — with composition, you rarely need to downcast; components are already accessed through their specific interfaces
- [[Loss Function]] — in ML, swapping loss functions is a textbook composition pattern: `Trainer` HAS-A `LossFunction`
- [[Gradient Descent]] — optimizer algorithms (Adam, SGD, RMSProp) are typically composed into models via an optimizer interface, not inherited
- [[Neural Network]] — `nn.Sequential` composes layers as a list of modules (HAS-A relationship), not an inheritance chain; this is the composition-over-inheritance pattern applied to deep learning
- [[Agent]] — an RL agent HAS-A policy, HAS-A value function, HAS-A replay buffer; none of these are inherited — they are composed in, making each component independently swappable
- [[Markov Decision Process]] — an MDP is itself a composition of State, Action, Reward, and Transition components, mirroring how a well-designed agent composes its building blocks
