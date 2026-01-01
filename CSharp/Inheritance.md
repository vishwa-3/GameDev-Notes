## 1. What Inheritance Really Is

> Inheritance models an **“is-a” relationship**.

A derived class **is** a specialized version of a base class.

```csharp
class Character
{
    public int Health { get; protected set; }
}
```

```csharp
class Player : Character
{
}
```

A `Player` **is a** `Character`.

If that sentence feels wrong, inheritance is wrong.

---

## 2. Basic Syntax

```csharp
class Enemy : Character
{
    public void Attack() { }
}
```

- `Enemy` inherits fields and methods of `Character`
- Cannot access `private` members
- Can access `protected` members

---

## 3. Access Modifiers in Inheritance (Critical)

| Modifier             | Accessible in child? |
| -------------------- | -------------------- |
| `private`            | no                   |
| `protected`          | yes                  |
| `public`             | yes                  |
| `internal`           | Assembly dependent   |
| `protected internal` | yes                  |

Production rule:

- `private` by default
- `protected` only when extension is intended

## 4. What gets inherited in C#

| Feature           | Inherited?           |
| ----------------- | -------------------- |
| Fields            | Yes                  |
| Methods           | Yes                  |
| Constructors      | No                   |
| Private members   | Not accessible       |
| Protected members | Yes                  |
| Static members    | Yes (not overridden) |
| Sealed methods    | Yes (not overridden) |

---

## 5. Method Overriding (Virtual / Override)

### Base class

```csharp
class Character
{
    public virtual void Die()
    {
        Debug.Log("Character died");
    }
}
```

### Derived class

```csharp
class Enemy : Character
{
    public override void Die()
    {
        Debug.Log("Enemy exploded");
    }
}
```

- Method call is resolved **at runtime**
- Enables polymorphism

---

## 6. `base` Keyword

```csharp
public override void Die()
{
    base.Die();
    DropLoot();
}
```

Use when:

- Base behavior must still run

---

## 7. Abstract Classes (Common in Gameplay)

Abstract classes:

- Cannot be instantiated
- Define required behavior

```csharp
abstract class Weapon
{
    public abstract void Fire();
}
```

```csharp
class Gun : Weapon
{
    public override void Fire() { }
}
```

Use when:

- Base behavior is incomplete
- Child **must** implement something

---

## 8. Sealed Classes (Stop Inheritance)

```csharp
sealed class FinalBoss
{
}
```

Use when:

- Extension would break invariants
- Security or correctness matters

---

## 9. Unity Reality Check (VERY IMPORTANT)

### Unity already uses inheritance

```csharp
MonoBehaviour → Behaviour → Component → Object
```

Your scripts are **already deep in a hierarchy**.

Adding more inheritance increases fragility.

---

### Unity best practice

bad - Deep inheritance trees
good - Shallow inheritance + composition

Bad:

```
Character
 └─ Enemy
     └─ RangedEnemy
         └─ FireEnemy
```

Good:

```
Enemy
 + MovementComponent
 + AttackComponent
```

---

All C# classes implicitly inherit from `object`.

---

## 10. Constructor behavior in C#

Child constructor always calls parent constructor first.

```csharp
class A
{
    public A(int x)
    {
        Console.WriteLine("A: " + x);
    }
}

class B : A
{
    public B() : base(10)
    {
        Console.WriteLine("B");
    }
}
```

### Rules

- Base constructor runs **first**
- `base()` must be the **first call**
- You can call **either** `base()` or `this()`, not both
- If base has no parameterless constructor → you must call it explicitly

---

## 11. Method overriding in C#

### Java

Methods are virtual by default.

### C#

You must explicitly allow overriding.

```csharp
class A
{
    public virtual void Test()
    {
        Console.WriteLine("A");
    }
}

class B : A
{
    public override void Test()
    {
        Console.WriteLine("B");
    }
}
```

### Key rules

- Base method must be `virtual`, `abstract`, or `override`
- Child must use `override`
- Visibility cannot be reduced
- Return type must be same or covariant

---

## 12. `base` keyword (C# version of `super`)

Used for **3 purposes**, exactly like Java.

### 1️. Access base fields

```csharp
class Parent
{
    public int Value = 10;
}

class Child : Parent
{
    public int Value = 20;

    public void Show()
    {
        Console.WriteLine(Value);       // 20
        Console.WriteLine(base.Value);  // 10
    }
}
```

---

### 2️. Call base methods

```csharp
class Parent
{
    public virtual void Greet()
    {
        Console.WriteLine("Hello from Parent");
    }
}

class Child : Parent
{
    public override void Greet()
    {
        Console.WriteLine("Hello from Child");
        base.Greet();
    }
}
```

---

### 3️. Call base constructor

```csharp
class Parent
{
    public Parent()
    {
        Console.WriteLine("Parent constructor");
    }
}

class Child : Parent
{
    public Child() : base()
    {
        Console.WriteLine("Child constructor");
    }
}
```

---

## 13. Multi-level inheritance

```csharp
class A { public A() { Console.WriteLine("A"); } }
class B : A { public B() { Console.WriteLine("B"); } }
class C : B { public C() { Console.WriteLine("C"); } }

new C();
```

Output:

```
A
B
C
```

---

## 14. Abstract classes (C#)

```csharp
abstract class Weapon
{
    public abstract void Fire();

    public virtual void Reload()
    {
        Console.WriteLine("Reloading");
    }
}

class Gun : Weapon
{
    public override void Fire()
    {
        Console.WriteLine("Gun fires");
    }
}
```

---

## 15. `sealed`

```csharp
sealed class A
{
}
```

Cannot be inherited.

---

## 16. `sealed` methods

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

Stops further overriding.

---

## 14. Key Java → C# keyword mapping

| Java             | C#                   |
| ---------------- | -------------------- |
| `extends`        | `:`                  |
| `super`          | `base`               |
| `final class`    | `sealed class`       |
| `final method`   | `sealed override`    |
| `final variable` | `readonly` / `const` |
| `Object`         | `object`             |

---
