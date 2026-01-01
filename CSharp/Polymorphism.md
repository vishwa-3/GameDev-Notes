## 1. What Polymorphism REALLY Means

**Polymorphism = same interface, different behavior at runtime**

Key words:

- SAME base type
- SAME method call
- DIFFERENT behavior
- DECIDED AT RUNTIME

Not at compile time.

---

## 2. Why Polymorphism Exists (The Core Problem)

Without polymorphism:

```csharp
if (weapon is Gun)
{
    gun.Fire();
}
else if (weapon is Bow)
{
    bow.Fire();
}
```

Problems:

- Tight coupling
- Hard to extend
- Violates Open/Closed Principle
- Every new type modifies old code

This does **not scale**.

---

## 3. Polymorphism Solution (High-Level)

```csharp
Weapon weapon = new Gun();
weapon.Fire(); // Gun fires
```

Later:

```csharp
weapon = new Bow();
weapon.Fire(); // Bow fires
```

Same call:

```csharp
weapon.Fire();
```

Different behavior.
This is polymorphism.

---

## 4. Types of Polymorphism in C#

There are **4 forms**, but only **2 matter** in production.

| Type                   | Real Use            |
| ---------------------- | ------------------- |
| Method Overloading     | Compile-time        |
| Operator Overloading   | Rare                |
| Method Overriding      | Runtime (important) |
| Interface Polymorphism | Runtime (important) |

We focus on **runtime polymorphism**.

---

## 5. Method Overriding (Class-Based Polymorphism)

### Step 1: Base class

```csharp
abstract class Weapon
{
    public abstract void Fire();
}
```

Key:

- `abstract` means “must be overridden”
- No implementation

---

### Step 2: Derived classes

```csharp
class Gun : Weapon
{
    public override void Fire()
    {
        Console.WriteLine("Gun fires");
    }
}

class Bow : Weapon
{
    public override void Fire()
    {
        Console.WriteLine("Bow shoots");
    }
}
```

---

### Step 3: Polymorphic call

```csharp
Weapon weapon;

weapon = new Gun();
weapon.Fire(); // Gun fires

weapon = new Bow();
weapon.Fire(); // Bow shoots
```

Important:

- Variable type → `Weapon`
- Object type → `Gun` / `Bow`
- Method chosen → object type

---

## 6. Runtime Dispatch (WHAT ACTUALLY HAPPENS)

At runtime:

- CLR checks actual object type
- Looks up overridden method
- Executes correct version

This is done via **virtual method table (v-table)**.

That’s why:

- Call is slightly slower
- But flexible and powerful

---

## 7. `virtual`, `override`, `abstract`

| Keyword           | Meaning                |
| ----------------- | ---------------------- |
| `virtual`         | Can be overridden      |
| `override`        | Overrides base         |
| `abstract`        | Must be overridden     |
| `sealed override` | Stops further override |

Example:

```csharp
class A
{
    public virtual void Test() { }
}

class B : A
{
    public sealed override void Test() { }
}
```

---

## 8. What Is NOT Polymorphism

### Method overloading

```csharp
void Fire(int x)
void Fire(float x)
```

Compile-time decision.
Not polymorphism.

---

### Method hiding (`new`)

```csharp
class A
{
    public void Test() { }
}

class B : A
{
    public new void Test() { }
}
```

Behavior depends on **variable type**, not object type.

---

## 9. Interface-Based Polymorphism

### Interface

```csharp
interface IDamageable
{
    void TakeDamage(int amount);
}
```

### Implementations

```csharp
class Player : IDamageable
{
    public void TakeDamage(int amount) { }
}

class Enemy : IDamageable
{
    public void TakeDamage(int amount) { }
}
```

---

### Polymorphic usage

```csharp
IDamageable target;

target = new Player();
target.TakeDamage(10);

target = new Enemy();
target.TakeDamage(10);
```

This is **cleaner than inheritance**.

---

## 10. Polymorphism in Unity

### Example: Damage system

```csharp
void ApplyDamage(IDamageable target)
{
    target.TakeDamage(10);
}
```

Now:

- Player
- Enemy
- Boss
- BreakableObject

All work **without changing this method**.

This is **production-level polymorphism**.

---

## 11. Unity Update Loop + Polymorphism

```csharp
List<IUpdatable> updatables;

foreach (var u in updatables)
{
    u.Tick();
}
```

Each object:

- Has different behavior
- Same call

This replaces massive `switch` statements.

---

## 12. Performance Reality

Polymorphism cost:

- Virtual call
- Interface dispatch

Cost:

- Very small
- Acceptable in gameplay
- Avoid inside extremely hot math loops

Rule:

> Prefer clarity first, optimize only if profiler says so.

---

## 13. Common Polymorphism Bugs

1. Forgot `virtual`
2. Forgot `override`
3. Using `new` unintentionally
4. Expecting overloading to be polymorphism
5. Deep inheritance trees

---

## 14. Interview-Level Explanation

If asked:

> “Explain polymorphism in C#”

Answer:

> Polymorphism allows different objects to be treated through a common base type or interface, where the method implementation executed is determined at runtime based on the actual object type.

---

## 15. Mental Model (THIS IS THE KEY)

Say this in your head:

> Variable decides what I can call
> Object decides what actually runs

If you understand this, polymorphism clicks forever.

---

## 16. One Full Example (End-to-End)

```csharp
abstract class Shape
{
    public abstract float Area();
}

class Circle : Shape
{
    public float Radius;

    public override float Area()
    {
        return Mathf.PI * Radius * Radius;
    }
}

class Rectangle : Shape
{
    public float Width, Height;

    public override float Area()
    {
        return Width * Height;
    }
}
```

Usage:

```csharp
Shape s = new Circle { Radius = 5 };
Debug.Log(s.Area());

s = new Rectangle { Width = 4, Height = 3 };
Debug.Log(s.Area());
```

Same call, different logic.

---

## 17. When NOT to Use Polymorphism

Do not use when:

- Behavior does not vary
- Only data differs
- Performance-critical math loops
- Simple conditional logic is clearer

---
