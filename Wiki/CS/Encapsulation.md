# Encapsulation

**One-liner:** Encapsulation is the practice of bundling data with the code that manages it, and restricting direct access to that data from the outside world.

## Why It Exists

Imagine you're building a bank account class. You store a `balance` field. Now suppose another part of the program does this:

```java
account.balance = -50000;
```

Nothing stops it. The field is public. The number is now negative fifty thousand. Your bank is ruined.

This is the problem encapsulation solves. Without it, any code anywhere can reach into an object and change its internal state. There's no single place where validity is enforced. You might write a check in `deposit()`, but someone can bypass `deposit()` entirely and write to `balance` directly. The invariant — "balance is always >= 0" — is impossible to guarantee.

Before OOP enforced this, bugs in large programs were nightmarish to trace because *any function in the entire codebase could be the one that corrupted your data.* Encapsulation shrinks the blast radius: only the object's own methods can touch its own fields, so if the balance is wrong, you only need to look inside the `BankAccount` class.

## The Concept

Encapsulation has two interlocking parts:

**1. Bundling:** Keeping related data and behavior together in one class. This is the organizational half — a `BankAccount` should own its `balance` and its `deposit()` method. They shouldn't live in separate places.

**2. Access control:** Restricting which parts of the codebase can directly read or modify internal state. This is the protection half.

Java's access modifiers, from most to least restrictive:

| Modifier | Who can access |
|---|---|
| `private` | Only this class |
| `(package-private)` | Any class in the same package (default when no modifier is written) |
| `protected` | This class + subclasses + same package |
| `public` | Anyone |

The standard OOP practice: **make fields `private` by default.** Expose state only through methods you explicitly design.

**Why methods instead of direct field access?** Methods give you a checkpoint. When code goes through `deposit(double amount)`, you can validate the amount, fire an event, log the transaction, and enforce any business rule — all in one place. Direct field access bypasses every one of those checkpoints.

**Getters and setters:** The common pattern for exposing a private field is a getter (`getBalance()`) and optionally a setter (`setBalance()`). This is often taught as the definition of encapsulation, but that's a shallow reading. A getter that returns the field directly and a setter that just assigns it add almost no protection — they're glorified public fields. The real value is:

1. **Getters without setters** — read-only access. The field can never be changed from outside.
2. **Setters with validation** — the setter rejects invalid values before they corrupt state.
3. **Computed getters** — the "field" returned doesn't actually exist in memory; it's calculated on demand.
4. **Future-proofing** — if you expose `getBalance()` instead of `balance`, you can change the internal representation later without breaking any calling code.

The "just generate getters and setters for everything" reflex is a habit that defeats the purpose. If you have a setter for every field with no validation, you've typed more and gained nothing.

## Intuition

A vending machine is the canonical real-world example, and it earns that status. You interact with a vending machine through its interface: insert money, press a button, retrieve item. The internal mechanism — the spring, the coin counter, the inventory tracker — is hidden behind the panel. You can't directly grab product from the shelf or deposit coins straight into the cash box. The machine enforces the protocol.

If the machine malfunctioned and had a hole in the side where you could just reach in and take things, the invariants (you pay before getting product) would be unenforceable. Encapsulation is the panel — not to be mean, but to keep the system functioning correctly.

For Baymax specifically: a motor controller class should encapsulate its current speed and direction. External code says "move forward at 30%." It doesn't directly set voltage values. The motor controller validates the command, checks safety limits, and executes — this is where you prevent the motor from being commanded to 300% speed and burning out.

## Key Example

```java
public class BankAccount {
    private double balance;       // private — no direct outside access
    private String ownerName;

    public BankAccount(String ownerName, double initialBalance) {
        this.ownerName = ownerName;
        // Even in the constructor, we validate through a method
        if (initialBalance < 0) {
            throw new IllegalArgumentException("Initial balance cannot be negative");
        }
        this.balance = initialBalance;
    }

    // Getter — read-only access to balance
    public double getBalance() {
        return balance;
    }

    // No setBalance() — balance can only change through deposit/withdraw

    public void deposit(double amount) {
        if (amount <= 0) {
            throw new IllegalArgumentException("Deposit must be positive");
        }
        balance += amount;
    }

    public void withdraw(double amount) {
        if (amount <= 0) {
            throw new IllegalArgumentException("Withdrawal must be positive");
        }
        if (amount > balance) {
            throw new IllegalStateException("Insufficient funds");
        }
        balance -= amount;
    }

    public String getOwnerName() {
        return ownerName;
    }
}
```

What this buys you:
- `balance` can never go negative (enforced in `withdraw`).
- `balance` can never be set to an arbitrary value by outside code.
- You can add logging, transaction history, or interest calculation inside these methods later, and *no calling code changes*.

Python equivalent with the convention for private fields:
```python
class BankAccount:
    def __init__(self, owner_name, initial_balance):
        self._owner_name = owner_name   # convention: don't touch from outside
        if initial_balance < 0:
            raise ValueError("Initial balance cannot be negative")
        self.__balance = initial_balance  # name-mangled: harder to access externally

    @property
    def balance(self):
        return self.__balance

    def deposit(self, amount):
        if amount <= 0:
            raise ValueError("Deposit must be positive")
        self.__balance += amount

    def withdraw(self, amount):
        if amount <= 0:
            raise ValueError("Withdrawal must be positive")
        if amount > self.__balance:
            raise ValueError("Insufficient funds")
        self.__balance -= amount
```

Python's `@property` decorator creates a getter that looks like a field access (`account.balance`) but is actually a method call. This is a cleaner API than `account.get_balance()`.

## Worked Example

Consider a Baymax motor with a speed cap for safety. The motor can physically spin at 0–100%, but we impose a software limit of 75% during testing.

Without encapsulation:
```java
robot.leftMotorSpeed = 90;  // Bypasses the safety limit entirely
```

With encapsulation:
```java
public class Motor {
    private double speed;
    private static final double MAX_SAFE_SPEED = 75.0;
    private boolean isEnabled;

    public Motor() {
        this.speed = 0.0;
        this.isEnabled = false;
    }

    public void enable() { this.isEnabled = true; }
    public void disable() {
        this.isEnabled = false;
        this.speed = 0.0;
    }

    public void setSpeed(double requestedSpeed) {
        if (!isEnabled) {
            throw new IllegalStateException("Motor must be enabled before setting speed");
        }
        if (requestedSpeed < 0 || requestedSpeed > 100) {
            throw new IllegalArgumentException("Speed must be between 0 and 100");
        }
        // Enforce the safety cap
        this.speed = Math.min(requestedSpeed, MAX_SAFE_SPEED);
    }

    public double getSpeed() { return speed; }
}
```

Now when the limit changes (say, real hardware arrives and the cap becomes 60%), you change one constant in one class. Every piece of code that uses `Motor` gets the new behavior automatically.

## Gotchas

**"I'll just make everything public to avoid the hassle."** This feels pragmatic in week one and catastrophic in week six. The discipline pays off when you have multiple classes and can no longer hold all the state in your head at once.

**Generating getters and setters for every field automatically (IDE temptation).** If your IDE offers "generate getters and setters," pause before clicking. Ask: does every field need a setter? Many don't. A setter for every field is still a public field — just with more code.

**Returning mutable objects from getters.** If your class has a `private List<String> items` and your getter returns the list directly, outside code can call `account.getItems().add("fraud")`. You've exposed internal state through the getter. Fix: return `Collections.unmodifiableList(items)` or a defensive copy.

**Python's lack of true enforcement.** In Python, `__balance` (name mangling) makes accidental access harder but not impossible — `account._BankAccount__balance` still works. Python trusts the programmer. Java doesn't. When working on Baymax in Python, the convention of leading underscores (`_field`) signals "private" but relies on discipline, not the compiler.

**Encapsulation ≠ security.** Encapsulation is about controlling state and reducing coupling, not about preventing a malicious programmer from accessing fields via reflection. These are different problems.

## See Also
- [[Classes and Objects]] — encapsulation is meaningless without understanding what a class and object are
- [[Inheritance]] — subclasses can access `protected` fields, which partially breaks encapsulation — this is a real design tension
- [[Interfaces and Abstract Classes]] — one way to expose a clean public interface while hiding implementation details entirely
