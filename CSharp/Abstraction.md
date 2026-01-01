## 1. What Abstraction REALLY Is?

**Abstraction = exposing _what_ an object does, hiding _how_ it does it.**

---

## 2. Common Doubts You WILL Have

I’m listing these because almost everyone gets stuck here.

### Doubt 1

**“Is abstraction same as encapsulation?”**

Short answer: NO

- Encapsulation → hiding **data**
- Abstraction → hiding **implementation logic**

Encapsulation is about **state**
Abstraction is about **behavior**

---

### Doubt 2

**“Is abstraction only abstract classes?”**

NO

Abstraction in C# is achieved using:

- **Interfaces**
- **Abstract classes**

Both provide abstraction. They solve **slightly different problems**.

---

### Doubt 3

**“Why not just use concrete classes?”**

Because concrete classes:

- Expose implementation
- Create tight coupling
- Make systems rigid

Abstraction allows:

- Loose coupling
- Polymorphism
- Replaceable behavior

---

### Doubt 4

**“What is the real difference between interface and abstract class?”**

You _will_ ask this. So here it is.

| Feature                | Interface             | Abstract Class       |
| ---------------------- | --------------------- | -------------------- |
| Fields                 | no                    | yes                  |
| Constructors           | no                    | yes                  |
| Multiple inheritance   | yes                   | no                   |
| Default implementation | C# 8+ (limited)       | yes                  |
| Purpose                | Capability / contract | Shared base behavior |

Rule of thumb:

- **Interface = what it can do**
- **Abstract class = what it is**

---

### Doubt 5

**“When should I use abstraction?”**

Use abstraction when:

- You want to depend on behavior, not implementation
- You expect multiple implementations
- You want testable, replaceable code

Do NOT use it everywhere.

---

### Doubt 6

**“Is abstraction a runtime or compile-time concept?”**

Answer:

- Abstraction is a **design concept**
- Implemented via **compile-time contracts**
- Used via **runtime polymorphism**

---

## 3. Abstraction with Interface (CORE PATTERN)

### Step 1: Define abstraction

```csharp
interface IWeapon
{
    void Fire();
}
```

This says:

> “Anything that is a weapon must be able to fire.”

No implementation.

---

### Step 2: Implement details separately

```csharp
class Gun : IWeapon
{
    public void Fire()
    {
        Console.WriteLine("Gun fires");
    }
}
```

```csharp
class Bow : IWeapon
{
    public void Fire()
    {
        Console.WriteLine("Bow shoots");
    }
}
```

---

### Step 3: Use abstraction

```csharp
void UseWeapon(IWeapon weapon)
{
    weapon.Fire();
}
```

Caller doesn’t care:

- Gun
- Bow
- Laser
- MagicStaff

That’s abstraction working.

---

## 4. Abstraction with Abstract Class

Used when **some behavior is shared**.

```csharp
abstract class Enemy
{
    public int Health;

    public abstract void Attack();

    public virtual void TakeDamage(int amount)
    {
        Health -= amount;
    }
}
```

```csharp
class Zombie : Enemy
{
    public override void Attack()
    {
        Console.WriteLine("Zombie bites");
    }
}
```

Here:

- Attack is abstract (must implement)
- Damage logic is shared

---

## 5. What Abstraction PREVENTS

Without abstraction:

```csharp
Gun gun = new Gun();
gun.Fire();
```

You are locked to `Gun`.

With abstraction:

```csharp
IWeapon weapon = new Gun();
weapon.Fire();
```

Now you can swap implementations freely.

---

## 6. Abstraction + Polymorphism (They Work Together)

Abstraction alone does nothing.
Polymorphism executes it.

Think like this:

- Abstraction → contract
- Polymorphism → execution

---

## 7. Unity-Specific Reality

In Unity:

- `MonoBehaviour` already gives inheritance
- Overusing abstract base classes = rigid hierarchy

Unity best practice:

- **Interfaces for behavior**
- **Components for composition**

Example:

```csharp
interface IMovable
{
    void Move();
}
```

Attach different movement logic to different GameObjects.

---

## 8. Interview-Grade Definition

If asked:

> “What is abstraction?”

Say:

> Abstraction is the process of exposing only essential behavior through contracts while hiding implementation details, allowing systems to depend on behavior rather than concrete implementations.

---

## 9. Red Flags

- Interface with 20 methods
- Abstract class doing everything
- No alternative implementations
- Forced inheritance

---
