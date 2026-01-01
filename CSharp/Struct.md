## 1. What a `struct` Really Is

A `struct` is a **value type**.

That means:

- It stores **data directly**
- Assignment **copies the data**
- No shared identity
- No inheritance (except from `ValueType`)
- No garbage collection

If you remember only one thing:

> A struct represents **a value**, not an entity.

---

## 2. Basic Struct Syntax

```csharp
public struct Damage
{
    public int amount;
}
```

Usage:

```csharp
Damage d1 = new Damage { amount = 10 };
Damage d2 = d1;

d2.amount = 20;
// d1.amount is still 10
```

---

## 3. Memory Behavior

### Where structs live

This is subtle and important.

Structs are stored:

- On the **stack** when local
- **Inline** inside objects when fields
- On the **heap** if part of a heap object

“Structs are always on stack” → wrong
“Structs are stored inline” → correct

Example:

```csharp
class Player
{
    public Damage damage; // stored inside Player object
}
```

No extra allocation.

---

## 4. Passing Structs to Methods

### Default (by value)

```csharp
void Modify(Damage d)
{
    d.amount = 50;
}
```

Original is untouched.

---

### By reference (`ref`)

```csharp
void Modify(ref Damage d)
{
    d.amount = 50;
}
```

Original is modified.

Use `ref` carefully. It breaks value semantics.

---

## 5. Immutability

Structs should be **immutable** when possible.

Bad:

```csharp
public struct Stats
{
    public int hp;
    public int attack;
}
```

Good:

```csharp
public readonly struct Stats
{
    public int Hp { get; }
    public int Attack { get; }

    public Stats(int hp, int attack)
    {
        Hp = hp;
        Attack = attack;
    }
}
```

Why?

- Prevents accidental copies causing bugs
- Safer in multithreading
- Easier reasoning

---

## 6. Constructors in Structs

Rules:

- Must initialize **all fields**
- Cannot have parameterless constructor (until C# 10 with limitations)

```csharp
public struct Range
{
    public int Min;
    public int Max;

    public Range(int min, int max)
    {
        Min = min;
        Max = max;
    }
}
```

---

## 7. Default Value Behavior

```csharp
Damage d = default;
```

Equivalent to:

```csharp
Damage d = new Damage { amount = 0 };
```

All fields are zeroed.

---

## 8. Boxing (Danger Zone)

### What is boxing?

```csharp
struct Damage { public int value; }

Damage d = new Damage { value = 10 };
object o = d; // boxing
```

- Allocates on heap
- Copies struct
- Causes GC

Unboxing:

```csharp
Damage d2 = (Damage)o;
```

Avoid boxing in hot paths.

---

## 9. Interfaces and Structs

Structs can implement interfaces.

```csharp
public struct Damage : IDamageable
{
    public int Value;
    public void Apply() { }
}
```

Calling through interface may cause boxing.

---

## 10. When to Use Struct vs Class

| Use Struct     | Use Class          |
| -------------- | ------------------ |
| Small data     | Large data         |
| Immutable      | Mutable            |
| No identity    | Has identity       |
| Math types     | Entities           |
| No inheritance | Needs polymorphism |

Unity examples:

- `Vector3` → struct
- `Player` → class

---

## 11. Common Mistakes (Memorize)

1. Large structs (> 32 bytes)
2. Mutable public fields
3. Passing structs by value unintentionally
4. Using structs as entities
5. Forgetting copy behavior

---

## 12. Performance Rules (Production)

- Keep structs small
- Prefer `readonly struct`
- Avoid interface calls on structs
- Avoid boxing
- Use `in` for large readonly structs

Example:

```csharp
void Process(in Damage d)
{
    // no copy, read-only
}
```

---
