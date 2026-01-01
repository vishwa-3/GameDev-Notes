## 1. `this` — the current instance

### What `this` means

`this` is a **reference to the current object instance**.

```csharp
public class Player
{
    public int health;

    public void TakeDamage(int damage)
    {
        this.health -= damage;
    }
}
```

Here:

- `this` = _the Player object on which the method was called_
- It disambiguates **instance state** from local variables

---

## 2. Why `this` exists

### Case 1: Name collision

```csharp
public Player(int health)
{
    health = health;   // wrong
}
```

Both refer to the parameter.

Correct:

```csharp
public Player(int health)
{
    this.health = health;
}
```

Rule:

> Use `this` when instance members are shadowed.

---

## 3. `this` is NOT optional in logic

This:

```csharp
health -= 10;
```

Is compiled as:

```csharp
this.health -= 10;
```

So:

- `this` is implicit
- Use it only when clarity is needed

---

## 4. `this` in method chaining

```csharp
public class Builder
{
    public Builder SetHealth(int value)
    {
        health = value;
        return this;
    }
}
```

Used for fluent APIs.

---

## 5. `this` in constructors

### Constructor chaining

```csharp
public class Weapon
{
    public int damage;

    public Weapon() : this(10) {}

    public Weapon(int damage)
    {
        this.damage = damage;
    }
}
```

Rules:

- `this()` calls another constructor
- Must be first line
- Prevents duplication

---

## 6. `this` with extension methods

Inside an extension method:

```csharp
public static void Reset(this Transform t)
{
    t.position = Vector3.zero;
}
```

Here:

- `this` is syntax to attach behavior
- Not the same as instance `this`
- Still important to understand

---

## 7. `base` — parent class reference

### What `base` means

`base` refers to the **immediate parent class instance**.

```csharp
class Enemy
{
    protected int health;
}

class Boss : Enemy
{
    void Start()
    {
        base.health = 200;
    }
}
```

---

## 8. Why `base` exists

### Case 1: Access overridden methods

```csharp
class Enemy
{
    public virtual void Die()
    {
        Debug.Log("Enemy died");
    }
}

class Boss : Enemy
{
    public override void Die()
    {
        base.Die();   // call parent logic
        Debug.Log("Boss explosion");
    }
}
```

Without `base`:

- Parent behavior is skipped
- Bugs appear silently

---

## 9. `base` in constructors

Parent constructor always runs **first**.

```csharp
class Enemy
{
    public Enemy(int health) {}
}

class Boss : Enemy
{
    public Boss() : base(500) {}
}
```

Rules:

- `base()` must be first
- If not specified, default constructor is called
- Mandatory if parent has no parameterless constructor

---

## 10. When NOT to use `base`

If inheritance is shallow and behavior should be replaced:

```csharp
public override void Move()
{
    // completely new logic
}
```

Calling `base` blindly causes duplicated behavior.

---

## 11. `new` — object creation

### Primary meaning

Allocates memory and creates an instance.

```csharp
Player player = new Player();
```

Steps:

1. Memory allocated
2. Fields initialized
3. Constructor executed
4. Reference returned

---

## 12. `new` and heap allocation

- Reference types → heap
- `new` creates object
- Variable holds reference, not object

```csharp
Player p1 = new Player();
Player p2 = p1;
```

Both point to same object.

---

## 13. `new` vs `new` (keyword overload)

Important: `new` has **multiple meanings**.

---

## 14. `new` as method hiding

```csharp
class Enemy
{
    public void Attack() {}
}

class Boss : Enemy
{
    public new void Attack() {}
}
```

This:

- Hides parent method
- Does NOT override
- Is resolved at compile time

Dangerous. Avoid unless intentional.

---

## 15. `new` vs `override`

| Keyword  | Polymorphism | Runtime dispatch |
| -------- | ------------ | ---------------- |
| override | Yes          | Yes              |
| new      | No           | No               |

Use `override` for behavior change.
Use `new` only to **hide**, not extend.

---

## 16. `new` in generics constraint

```csharp
where T : new()
```

Means:

- Type must have public parameterless constructor
- Allows `new T()`

---

## 17. Unity-specific rules

### `new` with MonoBehaviour

```csharp
Player p = new Player(); // WRONG
```

Unity controls lifecycle.

Correct:

```csharp
Instantiate(prefab);
```

Or:

```csharp
gameObject.AddComponent<Player>();
```

---
