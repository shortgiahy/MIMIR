# Constructor

**One-liner:** A constructor is a special method that runs exactly once per object — at the moment of `new` — to initialize the object into a valid state.

## Core Idea
A constructor has the same name as the class and no return type. Its job is to set every instance field to a meaningful starting value so the object is safe to use immediately after creation. If you don't write one, Java silently inserts a **default constructor** with no parameters that zero-initializes all fields.

```java
public class IMUSensor {
    private double roll, pitch, yaw;

    // Parameterized constructor
    public IMUSensor(double initialRoll, double initialPitch, double initialYaw) {
        this.roll  = initialRoll;
        this.pitch = initialPitch;
        this.yaw   = initialYaw;
    }
}
```

## Why It Exists
Without constructors, you'd create an object and then call a bunch of setter methods to initialize it — a window of time when the object exists but is in a broken half-initialized state. Constructors close that window: initialization is **atomic** with creation. You can enforce invariants (throw exceptions for invalid arguments) before any other code can touch the object.

## Real-World Applications
- `ArrayList<String> list = new ArrayList<>(100)` — the `100` is a capacity hint passed to the constructor.
- `Scanner sc = new Scanner(System.in)` — binds the scanner to a stream at construction time.
- PyTorch: `nn.Linear(in_features=128, out_features=64)` — constructor allocates weight tensors immediately.
- Baymax: `ProximitySensor("front")` — constructor pins the sensor to its physical location label.

## Intuition
A constructor is like the activation procedure for a new piece of equipment. When a new Baymax sensor unit is deployed, you run the activation checklist (calibration, ID assignment, self-test) before it enters service. The constructor is that checklist — mandatory, and it runs before anything else can happen.

## Deep Dive
**Constructor overloading**
A class can have multiple constructors with different parameter lists. Java picks the right one at compile time based on argument types (overload resolution).

```java
public class Motor {
    private final String id;
    private double maxRpm;
    private boolean reversed;

    // Default constructor — sensible defaults
    public Motor(String id) {
        this(id, 300.0, false);   // delegates to parameterized constructor
    }

    // Parameterized constructor
    public Motor(String id, double maxRpm, boolean reversed) {
        if (id == null || id.isBlank()) throw new IllegalArgumentException("id required");
        if (maxRpm <= 0) throw new IllegalArgumentException("maxRpm must be positive");
        this.id       = id;
        this.maxRpm   = maxRpm;
        this.reversed = reversed;
    }
}
```

**`this(...)` — constructor chaining**
`this(...)` calls another constructor in the same class. It must be the very first statement. This avoids duplicating initialization logic across overloads.

**Inheritance and `super(...)`**
When a subclass constructor runs, the parent's constructor must also run first. Java inserts an implicit `super()` call if you don't provide one. If the superclass has no no-arg constructor, you must call `super(...)` explicitly as the first line.

```java
public class UltrasonicSensor extends Sensor {
    private double maxRangeM;

    public UltrasonicSensor(String id, double maxRangeM) {
        super(id);   // MUST be first — calls Sensor's constructor
        this.maxRangeM = maxRangeM;
    }
}
```

**Python — `__init__`**
Python's constructor equivalent is `__init__`. There's also `__new__` (rarely used) which actually allocates memory; `__init__` just initializes the already-allocated object.
```python
class IMUSensor:
    def __init__(self, initial_roll=0.0, initial_pitch=0.0, initial_yaw=0.0):
        self.roll  = initial_roll
        self.pitch = initial_pitch
        self.yaw   = initial_yaw

# "Overloading" via default args or *args/**kwargs — no separate definitions needed
sensor = IMUSensor()                        # all defaults
sensor2 = IMUSensor(initial_yaw=45.0)      # keyword arg
```

**Immutability pattern**
Fields marked `final` in Java can only be set in the constructor. If all fields are `final`, the object is immutable — safe to share across threads with no synchronization.

```java
public final class SensorConfig {
    public final String id;
    public final double sampleRateHz;

    public SensorConfig(String id, double sampleRateHz) {
        this.id = id;
        this.sampleRateHz = sampleRateHz;
    }
}
```

## Worked Example
```java
// Baymax context: motor with validation and sensible defaults
public class DriveMotor {
    private final String wheelPosition;  // "FL", "FR", "RL", "RR"
    private final double maxRpm;
    private double currentRpm;

    public DriveMotor(String wheelPosition) {
        this(wheelPosition, 250.0);  // default max RPM
    }

    public DriveMotor(String wheelPosition, double maxRpm) {
        if (maxRpm <= 0) throw new IllegalArgumentException("maxRpm must be > 0");
        this.wheelPosition = wheelPosition;
        this.maxRpm        = maxRpm;
        this.currentRpm    = 0.0;   // always start stopped
    }

    public void setSpeed(double rpm) {
        this.currentRpm = Math.clamp(rpm, -maxRpm, maxRpm);
    }

    public String getWheelPosition() { return wheelPosition; }
}

// Usage
DriveMotor fl = new DriveMotor("FL");           // 250 RPM limit
DriveMotor fr = new DriveMotor("FR", 300.0);    // custom limit
fl.setSpeed(180);
```

## See Also
- [[Class]] — defines the constructor
- [[Object]] — created and initialized by the constructor
- [[Instance]] — each `new` call produces one instance
- [[Inheritance]] — constructors must call `super()` up the chain
- [[Encapsulation]] — constructor is the first line of defense for invariants
- [[Neural Network]] — `nn.Linear(in_features=128, out_features=64)` is a constructor call that allocates and initializes weight and bias tensors; every layer's `__init__` is the constructor that sets up the learnable parameters
- [[NumPy Array]] — `np.array([...])` and `np.zeros(shape)` are constructor-like factory calls that allocate and initialize the underlying data buffer, mirroring how `new` initializes a Java object
