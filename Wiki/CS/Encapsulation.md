# Encapsulation

**One-liner:** Encapsulation bundles state and behavior together in one unit (a class) and hides the state from direct outside access, so the object can guarantee its own consistency.

## Core Idea
Encapsulation has two parts that are often confused:
1. **Bundling** — data (fields) and the code that operates on them (methods) live together in one class.
2. **Information hiding** — fields are `private`; the outside world interacts only through a controlled public API (methods). The class can validate, transform, and react when its state changes.

```java
public class BatteryMonitor {
    private double chargePercent;   // PRIVATE — hidden

    public void setCharge(double percent) {  // PUBLIC — controlled entry point
        if (percent < 0 || percent > 100)
            throw new IllegalArgumentException("Charge must be 0–100");
        this.chargePercent = percent;
    }

    public boolean isCritical() { return chargePercent < 10.0; }
    public double getCharge()   { return chargePercent; }
}
```

## Why It Exists
Without information hiding, any code anywhere can reach into an object and corrupt its state:
```java
// Without encapsulation — nothing stops this:
battery.chargePercent = -999;  // now the object is in an impossible state

// With encapsulation:
battery.setCharge(-999);  // throws exception — object stays consistent
```
The object can enforce **invariants** (rules that must always be true) because all mutations go through its methods.

A second benefit: the internal representation can change without breaking callers. If you later decide to store charge as milliamp-hours internally, you update `setCharge` and `getCharge` — callers never know.

## Real-World Applications
- Java's `ArrayList` hides its internal array; you never call `list.elementData[i]` directly.
- Python's `@property` decorator is the Pythonic encapsulation pattern.
- In ML: `scikit-learn` estimators hide their fitted parameters behind `predict()` — you don't touch `model.coef_` in production code.
- Baymax: `BatteryMonitor.setCharge()` prevents the control loop from accidentally setting an impossible battery level; `isCritical()` centralizes the threshold logic in one place.

## Intuition
An ATM is perfectly encapsulated. You interact with it through buttons and a card slot (public API). You cannot reach inside and directly move cash between compartments. The machine enforces its own rules — it won't let you withdraw more than your balance — because all transactions go through its interface, not your hands.

## Deep Dive
**Getters and setters — when to use them**
Don't reflexively write getters and setters for every field. A setter that does nothing but `this.x = x` provides zero encapsulation benefit. Ask:
- Does the setter need to validate? (Use it.)
- Does the getter need to transform or compute? (Use it.)
- Is the field truly read-only after construction? (Use `final` + no setter.)
- Is the field pure internal implementation detail? (No getter or setter at all.)

```java
// Bad — this is just public fields with extra steps
public void setName(String name) { this.name = name; }
public String getName() { return name; }

// Good — getter adds value by being computed
public double getDistanceMeters() { return rawDistanceCm / 100.0; }

// Good — setter adds validation
public void setAngle(int degrees) {
    if (degrees < 0 || degrees > 180)
        throw new IllegalArgumentException("Servo angle 0–180 only");
    this.angleDegrees = degrees;
}
```

**Python — encapsulation by convention and `@property`**
Python has no true `private`. Convention: `_field` means "internal, don't touch"; `__field` triggers name-mangling (harder but not impossible to access).

```python
class BatteryMonitor:
    def __init__(self):
        self._charge_percent = 100.0   # convention: private

    @property
    def charge(self) -> float:
        return self._charge_percent

    @charge.setter
    def charge(self, value: float):
        if not 0.0 <= value <= 100.0:
            raise ValueError("Charge must be 0–100")
        self._charge_percent = value

    @property
    def is_critical(self) -> bool:
        return self._charge_percent < 10.0

# Usage
monitor = BatteryMonitor()
monitor.charge = 8.5       # calls the setter — validated
print(monitor.is_critical) # True
```

**Encapsulation and thread safety**
Encapsulation is a prerequisite for thread safety. If fields are `private`, you can add `synchronized` to methods to prevent data races without changing any caller code.

**Immutable objects — the strongest form of encapsulation**
If all fields are `final` and set only in the constructor, the object cannot change state at all. No getters/setters needed for mutation. Examples: `String`, `Integer`, `LocalDate` in Java.

## Worked Example
```java
// Baymax context: encapsulated PID controller state
public class PIDController {
    private final double kp, ki, kd;
    private double integral  = 0.0;
    private double lastError = 0.0;

    public PIDController(double kp, double ki, double kd) {
        this.kp = kp; this.ki = ki; this.kd = kd;
    }

    // The outside world just calls update() — internal state is hidden
    public double update(double error, double dt) {
        integral    += error * dt;
        double deriv = (error - lastError) / dt;
        lastError    = error;
        return kp * error + ki * integral + kd * deriv;
    }

    // Safe reset via controlled method
    public void reset() {
        integral  = 0.0;
        lastError = 0.0;
    }
}

// Caller never touches integral or lastError directly — can't corrupt them
PIDController pid = new PIDController(1.2, 0.05, 0.3);
double output = pid.update(5.0, 0.01);  // error=5.0, dt=10ms
```

## See Also
- [[Access Modifier]] — the Java keywords (`private`, `public`, `protected`) that implement information hiding
- [[Class]] — the unit that bundles state and behavior
- [[Object]] — each object's state is independently encapsulated
- [[Constructor]] — enforces invariants at creation time
- [[Inheritance]] — subclasses can break encapsulation of the parent if fields are `protected` rather than `private`
