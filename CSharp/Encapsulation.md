## 1. What Encapsulation Really Means

> Encapsulation = **restrict direct access to data and expose behavior instead**

You do **not** let other code freely change your internal state.

Bad mental model:

> “Encapsulation means making things private.”

Correct mental model:

> “Encapsulation means protecting invariants.”

---

## 2. The Problem Without Encapsulation

```csharp
public class Player
{
    public int health;
}
```

Anywhere in the code:

```csharp
player.health = -999;
player.health = int.MaxValue;
```

Now your object is in an **invalid state**.

---

## 3. Basic Encapsulation

```csharp
public class Player
{
    private int health;

    public int Health
    {
        get => health;
        private set => health = Mathf.Clamp(value, 0, 100);
    }
}
```

Now:

- Health is always valid
- All changes go through one place
- Rules are enforced automatically

---

## 4. Unity-Approved Encapsulation Pattern

You still want Inspector access **without breaking encapsulation**.

```csharp
public class Player : MonoBehaviour
{
    [SerializeField] private int maxHealth = 100;
    [SerializeField] private int health;

    public int Health => health;

    public void TakeDamage(int amount)
    {
        health = Mathf.Max(0, health - amount);
    }
}
```

This is **production-standard Unity code**.

---

## 5. Encapsulation Through Behavior

Bad:

```csharp
player.health -= 10;
```

Good:

```csharp
player.TakeDamage(10);
```

Why?

- You control logic
- You can add effects later
- No breaking changes

---

## 6. Encapsulation Prevents Side Effects

Without encapsulation:

- Anyone can mutate data
- Bugs are non-local
- Changes are unpredictable

With encapsulation:

- Only the owner mutates state
- Changes are traceable
- Safer refactors

---

## 7. Encapsulation and Invariants

An **invariant** is a rule that must always be true.

Examples:

- Health ≥ 0
- Ammo ≤ maxAmmo
- Cooldown ≥ 0

Encapsulation enforces invariants.

```csharp
public void Heal(int amount)
{
    if (amount <= 0) return;
    health = Mathf.Min(maxHealth, health + amount);
}
```

---

## 8. Properties vs Methods

### Use properties when:

- No side effects
- Simple access

```csharp
public int Health => health;
```

### Use methods when:

- Logic is involved
- Side effects exist

```csharp
public void TakeDamage(int amount)
```

Never hide heavy logic inside getters.

---

> “What is encapsulation?”

> Encapsulation is the practice of restricting direct access to an object’s internal state and exposing behavior that preserves invariants.

---

## 1. Private Field + Public Getter (Read-only)

**Most common and safest variant**

```csharp
private int health;
public int Health => health;
```

Use when:

- External code must read
- Only the class may modify

This is the **default choice** in real projects.

---

## 2. Private Field + Public Getter + Private Setter

```csharp
private int health;

public int Health
{
    get => health;
    private set => health = Mathf.Max(0, value);
}
```

Use when:

- Value must be validated
- Updates come only from internal methods

This protects invariants cleanly.

---

## 3. Private Field + Public Methods (Behavior-only)

**Strong encapsulation**

```csharp
private int health;

public void TakeDamage(int amount)
{
    if (amount <= 0) return;
    health -= amount;
}

public void Heal(int amount)
{
    health += amount;
}
```

Use when:

- State changes have logic
- Side effects exist (FX, events)

Preferred in gameplay code.

---

## 4. Serialized Private Field (Unity Pattern)

Inspector access **without breaking encapsulation**.

```csharp
[SerializeField] private int maxHealth = 100;
public int MaxHealth => maxHealth;
```

Use when:

- Designers need to tweak values
- Code must remain protected

This is the **Unity gold standard**.

---

## 5. Internal Access (Assembly-level Encapsulation)

```csharp
internal class DamageSystem { }
```

Use when:

- Code should be hidden from other assemblies
- Large modular projects

Rare in Unity unless using assemblies.

---

## 6. Readonly Fields

```csharp
private readonly int maxHealth;
```

Use when:

- Value never changes after construction
- Strong immutability required

Excellent for correctness.

---

## 7. Immutable Encapsulation (Best for Data)

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

Use when:

- Data should never change
- Avoids side effects entirely

---

## 8. Property with Validation Logic

```csharp
private float speed;

public float Speed
{
    get => speed;
    set => speed = Mathf.Clamp(value, 0f, 10f);
}
```

Use when:

- Validation is cheap
- No side effects

Never put heavy logic here.

---

## 9. Encapsulation via Interfaces

```csharp
public interface IDamageable
{
    void TakeDamage(int amount);
}
```

Consumers depend on behavior, not data.

Use in scalable systems.

---
