## 1. What a Class Really Is

A **class** is a **blueprint**.

It defines:

- Data (fields / properties)
- Behavior (methods)
- Identity
- Lifetime

A class itself is **not usable** until you create an **object** from it.

```csharp
class Player
{
    public int health;
    public void Damage(int amount) { health -= amount; }
}
```

---

## 2. What an Object Is

An **object** is:

- A runtime instance of a class
- Allocated on the **heap**
- Accessed via a **reference**

```csharp
Player player = new Player();
```

- `player` → reference
- `new Player()` → actual object in memory

---

## 3. Object Lifetime

### Normal C# class

- Created with `new`
- Lives on heap
- Collected by GC when no references exist

```csharp
Player p = new Player();
p = null; // eligible for GC
```

You **do not control destruction timing**.

---

### Unity objects

```csharp
GameObject
MonoBehaviour
ScriptableObject
```

- Managed by Unity engine
- Created via Unity APIs
- Destroyed using `Destroy()`

```csharp
Destroy(gameObject);
```

Never use `new` for `MonoBehaviour`.

---

## 4. Fields vs Properties

### Fields (raw data)

```csharp
public int health;
```

Fast, but unsafe.

---

### Properties (controlled access)

```csharp
public int Health
{
    get => health;
    private set => health = Mathf.Max(0, value);
}
```

Production rule:

- Fields → private
- Properties → public

---

## 5. Constructors

```csharp
class Weapon
{
    public int damage;

    public Weapon(int damage)
    {
        this.damage = damage;
    }
}
```

Used for:

- Valid state creation
- Dependency injection

Not used for `MonoBehaviour`.

---

## 6. Methods Belong to Classes

Methods operate on object state.

```csharp
class Enemy
{
    int hp;

    public void TakeDamage(int amount)
    {
        hp -= amount;
    }
}
```

Avoid static methods when behavior depends on instance state.

---

## 7. Common Unity Class Patterns

### Component pattern

```csharp
class PlayerMovement : MonoBehaviour
```

Each class has **one responsibility**.

---

### Data vs Behavior split

```csharp
class PlayerStats { }          // data
class PlayerCombat { }        // behavior
```

---
