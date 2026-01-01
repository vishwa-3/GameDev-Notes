## 1. What an Interface REALLY Is?

An **interface is a contract**.

It says:

> “If you claim to be this, you MUST provide these behaviors.”

It contains:

- Method signatures
- Properties (no state)
- Events
- (C# 8+) optional default implementations

It contains **no object state**.

---

## 2. Interface Syntax in C#

```csharp
interface IDamageable
{
    void TakeDamage(int amount);
}
```

Key facts:

- No constructors
- No fields
- Methods are `public` implicitly
- Cannot store data

---

## 3. Implementing an Interface

```csharp
class Player : IDamageable
{
    public int Health;

    public void TakeDamage(int amount)
    {
        Health -= amount;
    }
}
```

Rules:

- Must implement **all members**
- Visibility must be `public`
- No `override` keyword

---

## 4. Why Interfaces Exist

Interfaces solve:

- Tight coupling
- Lack of multiple inheritance
- Rigid class hierarchies

They allow:

- Behavior sharing without state
- Multiple capabilities
- Clean polymorphism

---

## 5. Interface Polymorphism

```csharp
void ApplyDamage(IDamageable target)
{
    target.TakeDamage(10);
}
```

Caller does not care if target is:

- Player
- Enemy
- Boss
- BreakableObject

That’s scalable design.

---

## 6. Multiple Inheritance (THE PROBLEM)

### What multiple inheritance means

> A class inherits from **more than one base class**.

Example (NOT allowed in C#):

```csharp
class C : A, B  // illegal in C#
{
}
```

Why this is dangerous → diamond problem.

---

## 7. The Diamond Problem

```
      A
     / \
    B   C
     \ /
      D
```

If:

- B and C both override A’s method
- D inherits from both

Which method should D use?

This creates:

- Ambiguity
- Undefined behavior
- Language complexity

---

## 8. Why C# FORBIDS Multiple Class Inheritance

Because:

- Ambiguous method resolution
- Complex constructor order
- Fragile base classes
- Hard-to-reason behavior

C# chose **clarity over power**.

---

## 9. How C# SOLVES This: Interfaces

C# allows:

```csharp
class Player : MonoBehaviour, IDamageable, IMovable
{
}
```

Rules:

- Only **one class**
- Unlimited interfaces

This avoids the diamond problem because:

- Interfaces have no state
- No constructors
- No inherited implementation (mostly)

---

## 10. Multiple Interface Implementation

```csharp
interface IMovable
{
    void Move();
}

interface IAttackable
{
    void Attack();
}
```

```csharp
class Enemy : IMovable, IAttackable
{
    public void Move() { }
    public void Attack() { }
}
```

Enemy has:

- Movement behavior
- Attack behavior

No ambiguity.

---

## 11. Diamond Problem with Interfaces? (C# 8+)

Before C# 8 → impossible
C# 8+ → possible with **default interface methods**

Example:

```csharp
interface A
{
    void Test() => Console.WriteLine("A");
}

interface B : A
{
}

interface C : A
{
}

class D : B, C
{
}
```

> This can reintroduce ambiguity.

C# resolves it by:

- Requiring explicit implementation

```csharp
class D : B, C
{
    void A.Test()
    {
        Console.WriteLine("Resolved manually");
    }
}
```

---

## 12. Explicit Interface Implementation (IMPORTANT)

Used when:

- Name conflicts
- You want to hide methods from public API

```csharp
interface IFlyable
{
    void Move();
}

interface IWalkable
{
    void Move();
}
```

```csharp
class Character : IFlyable, IWalkable
{
    void IFlyable.Move() { }
    void IWalkable.Move() { }
}
```

Usage:

```csharp
((IFlyable)character).Move();
```

---

## 13. Unity Best Practices

In Unity:

- Prefer **interfaces** for behavior
- Avoid deep abstract inheritance
- Compose using components

Example:

```csharp
interface IInteractable
{
    void Interact();
}
```

Then:

- Door
- NPC
- Chest

All implement it differently.

---

## 14. Performance Reality

- Interface calls → virtual dispatch
- Cost is minimal
- Avoid in tight math loops
- Fine in gameplay logic

Never prematurely optimize this.

---
