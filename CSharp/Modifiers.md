## 1. Why modifiers exist

Modifiers control:

1. **Visibility** – who can access the member
2. **Ownership** – does it belong to an instance or the type?
3. **Lifetime** – per object or per application
4. **Safety** – preventing misuse

If everything is `public`, **your design has failed**.

---

## 2. `public`

### Meaning

Accessible from **anywhere**:

- Any class
- Any assembly
- Any system

```csharp
public int Health;
```

### When `public` is correct

- Public API
- Contracts
- Interfaces
- Data meant to be consumed externally

### When `public` is WRONG

- Internal state
- Mutable fields
- Implementation details

Bad:

```csharp
public int currentHealth;
```

Anyone can break invariants.

---

## 3. `private` (DEFAULT — memorize this)

### Meaning

Accessible **only inside the same class**.

```csharp
private int health;
```

### Why `private` should be your default

- Enforces encapsulation
- Prevents accidental misuse
- Allows refactoring safely
- Reduces cognitive load

**Rule:**

> Everything is `private` unless proven otherwise.

---

## 4. `protected`

### Meaning

Accessible:

- Inside the class
- Inside derived classes only

```csharp
protected int speed;
```

### Correct use case

- Base classes
- Template methods
- Shared internal behavior

### Danger in Unity

Deep inheritance + `protected` = fragile hierarchy.

Overuse example:

```csharp
protected int health;
protected float speed;
protected Weapon weapon;
```

This leads to **inheritance coupling**.

**Use sparingly.**

---

## 5. `internal`

### Meaning

Accessible **only within the same assembly** (`.dll`).

```csharp
internal class DamageSystem {}
```

### What is an assembly?

- A compiled unit (`Assembly-CSharp.dll`)
- Unity compiles scripts into assemblies

### Why `internal` exists

- Hide implementation details
- Expose only public API
- Protect systems from external misuse

### Unity use case

- Framework code
- Tools
- Core systems not meant for gameplay scripts

---

## 6. `internal` + `public` difference

| Modifier  | Same class | Same assembly | Other assembly |
| --------- | ---------- | ------------- | -------------- |
| private   | yes        | no            | no             |
| protected | yes        | no            | no             |
| internal  | yes        | yes           | no             |
| public    | yes        | yes           | yes            |

---

## 7. `static`

### What `static` really means

> Belongs to the **type**, not the instance.

```csharp
public static int EnemyCount;
```

- One copy for entire application
- Shared across all objects
- Exists once

> static is a modifier, but not an access modifier.

---

## 8. Instance vs static

### Instance field

```csharp
public int health;
```

- Each object has its own value
- Lives with the object

### Static field

```csharp
public static int totalEnemies;
```

- Shared across all objects
- Lives for app lifetime

---

## 9. Static in memory terms

- Instance → heap per object
- Static → type-level memory
- Static exists even if **no instance exists**

This is why static is dangerous.

---

## 10. Correct uses of `static`

### Constants

```csharp
public static readonly int MaxPlayers = 4;
```

### Stateless utility methods

```csharp
public static class MathUtils
{
    public static float Clamp01(float value) {}
}
```

### Global configuration (read-only)

```csharp
public static class GameConfig
{
    public static float Gravity = 9.8f;
}
```

---

## 11. WRONG uses of `static` (Unity beginners do this)

Static gameplay state

```csharp
public static int playerHealth;
```

Static MonoBehaviour logic

```csharp
public static Player Instance;
```

This leads to:

- Hidden dependencies
- Impossible testing
- Order-of-initialization bugs
- Scene reload issues

---

## 12. `static` vs Singleton

### Static

- No lifecycle
- No control
- Always alive

### Singleton

- Controlled instance
- Explicit initialization
- Can be replaced or mocked

Prefer:

- Dependency injection
- Explicit references

Avoid:

- Global statics

---

## 13. `static` methods cannot access instance data

This WILL NOT compile:

```csharp
static void TakeDamage()
{
    health -= 10;
}
```

Why?

- Static has no `this`
- No instance context

---

## 14. Modifier combinations (real usage)

```csharp
[SerializeField] private int health;

public int Health { get; private set; }

protected virtual void Die() {}

internal static class DamageUtils {}
```

Understand every word here:

- `private` → encapsulation
- `public` → controlled access
- `protected` → inheritance
- `internal` → assembly scope
- `static` → type-level

---

## 15. Unity-specific rules (critical)

### Unity Inspector

- Only **fields**
- `public` OR `[SerializeField] private`

### Best practice

```csharp
[SerializeField] private float speed;
public float Speed => speed;
```

---
