## 1. What is a FIELD?

A **field** is a variable that **stores data directly** inside a class or struct.

```csharp
public class Player
{
    public int health;
}
```

- Memory is allocated for it
- It holds raw state
- It has **no control**
- Anyone with access can read/write freely

### Field characteristics

- Direct data access
- No validation
- No side effects
- No future flexibility

### Field lifecycle

- Created when object is created
- Destroyed when object is destroyed
- Lives in memory as part of the object

---

## 2. Why fields alone are dangerous

```csharp
player.health = -999;
```

No rules. No protection. No logic.

### Problems

- Invalid states
- Hard-to-track bugs
- No hooks for events
- Cannot enforce invariants

In production code, **uncontrolled fields are a liability**.

---

## 3. What is a PROPERTY?

A **property** is a controlled access point to data.

```csharp
public class Player
{
    private int health;

    public int Health
    {
        get { return health; }
        set { health = value; }
    }
}
```

Important:

- Property is **not a variable**
- It is **syntax sugar for methods**
- `get` and `set` are functions

The compiler turns this into:

```csharp
int get_Health();
void set_Health(int value);
```

---

## 4. Why properties exist

Properties allow:

- Validation
- Encapsulation
- Lazy logic
- Events
- Debugging hooks
- Future changes without API breakage

Example:

```csharp
set
{
    health = Mathf.Clamp(value, 0, maxHealth);
}
```

Now invalid states are impossible.

---

## 5. Fields vs Properties

| Aspect        | Field | Property                    |
| ------------- | ----- | --------------------------- |
| Stores data   | Yes   | Usually (via backing field) |
| Validation    | No    | Yes                         |
| Encapsulation | No    | Yes                         |
| Logic         | No    | Yes                         |
| Safe API      | No    | Yes                         |
| Future-proof  | No    | Yes                         |

**Rule:**

> Fields store state. Properties expose state.

---

## 6. Backing field

A **backing field** is the actual variable behind a property.

```csharp
private int health;

public int Health
{
    get => health;
    set => health = value;
}
```

- Field = storage
- Property = gatekeeper

Never expose the backing field publicly.

---

## 7. Auto-properties

When no extra logic is needed:

```csharp
public int Health { get; set; }
```

Compiler generates:

```csharp
private int <Health>k__BackingField;
```

### Auto-property rules

- Clean
- Short
- No validation inside
- Still encapsulated

---

## 8. Auto-property with access restriction

Very common in production:

```csharp
public int Health { get; private set; }
```

- Readable by everyone
- Writable only by the class

Used when:

- State should not be modified externally
- Logic must control changes

---

## 9. Read-only properties

### Option 1: No setter

```csharp
public int MaxHealth => 100;
```

Computed, immutable.

---

### Option 2: Private setter

```csharp
public int Health { get; private set; }
```

- Mutable internally
- Immutable externally

---

### Option 3: Readonly backing field

```csharp
private readonly int id;

public int Id => id;
```

Set once (constructor / Awake).

---

## 10. Unity-specific behavior

### Unity Inspector does NOT serialize properties

This will NOT appear in Inspector:

```csharp
public int Health { get; set; }
```

Unity serializes **fields only**.

---

## 11. Correct Unity pattern (industry standard)

```csharp
[SerializeField] private int maxHealth = 100;

public int MaxHealth => maxHealth;
```

- Field for serialization
- Property for access

This is the **correct pattern**.

---

## 12. Validation pattern in Unity

```csharp
[SerializeField] private int health;

public int Health
{
    get => health;
    private set
    {
        health = Mathf.Clamp(value, 0, maxHealth);
        if (health == 0)
            Die();
    }
}
```

Now:

- Inspector edits field
- Code uses property
- State is safe

---

## 13. Never do this in Unity

```csharp
public int health;
```

Why?

- Anyone can modify it
- No validation
- No future control
- Breaks encapsulation

---

## 14. Fields vs Properties in performance

Important truth:

- Property access ≈ field access
- JIT inlines trivial getters/setters
- No measurable cost

So never avoid properties for performance.

---

## 15. Clean production example (Unity)

```csharp
public class Health : MonoBehaviour
{
    [SerializeField] private int maxHealth = 100;
    [SerializeField] private int currentHealth;

    public int MaxHealth => maxHealth;
    public int CurrentHealth => currentHealth;

    public void TakeDamage(int damage)
    {
        currentHealth = Mathf.Clamp(currentHealth - damage, 0, maxHealth);
    }
}
```

This is **correct, safe, scalable**.

---
