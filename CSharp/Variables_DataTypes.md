## 1. Variables

A **variable** is a **named reference to a memory location** that stores a value of a **specific type**.
In C#, the **type is mandatory and enforced at compile time** (strong + static typing).

Key facts:

- The **type decides memory size**, valid operations, and runtime behavior.
- The **CLR verifies type safety** before execution.
- In Unity, variables inside `MonoBehaviour` are **serialized**, **reflected**, and sometimes **copied** between native and managed memory.

---

## 2. Built-in Value Types (Core Primitives)

### Primitive Data Types Table

| Type     | Size             | Default Value | Range / Notes                   | Unity Usage Tips                               |
| -------- | ---------------- | ------------- | ------------------------------- | ---------------------------------------------- |
| `int`    | 4 bytes          | `0`           | −2,147,483,648 to 2,147,483,647 | Use for counters, indices, IDs                 |
| `float`  | 4 bytes          | `0f`          | ~7 digits precision             | **Unity’s default numeric type**               |
| `double` | 8 bytes          | `0d`          | ~15–16 digits precision         | Avoid in Unity gameplay (slower + unnecessary) |
| `bool`   | 1 byte (logical) | `false`       | `true` / `false`                | Game state flags                               |
| `char`   | 2 bytes          | `'\0'`        | UTF-16 character                | Rare in Unity                                  |
| `string` | Reference type   | `null`        | Immutable text                  | UI, debugging, keys                            |

---

### `int`

#### 1. “Stored on stack when local, inside object when field”

This line is about **where memory lives**.

##### Case 1: Local variable (inside a method)

```csharp
void Example()
{
    int x = 10;
}
```

What happens internally:

- `x` is a **local variable**
- Memory is allocated on the **stack**
- Stack memory is:

  - Fast
  - Automatically destroyed when the method exits
  - Not managed by the GC

Think of the stack as:

> “Temporary working memory for the current function call”

When `Example()` finishes → `x` **ceases to exist instantly**.

---

##### Case 2: Field (inside a class/object)

```csharp
class Player
{
    public int health = 100;
}
```

What happens:

- `health` is a **field**
- It lives **inside the Player object**
- The Player object itself lives on the **heap**
- `health` exists as long as the Player object exists

Memory layout conceptually:

```
Heap:
[ Player Object ]
   └── health (int)
```

When the Player object is destroyed → `health` is destroyed.

---

##### Unity-specific note

```csharp
public class Player : MonoBehaviour
{
    public int health;
}
```

- `health` is stored in **managed memory**
- Unity mirrors it into **native C++ memory** for serialization & Inspector
- That’s why fields show up in the Inspector, locals don’t

---

#### 2. “Arithmetic is checked only if `checked` keyword is used”

This is about **integer overflow safety**.

##### Default behavior (unchecked)

```csharp
int x = int.MaxValue;
x = x + 1;
Debug.Log(x);
```

Output:

```
-2147483648
```

What happened?

- `int` has a fixed 32-bit size
- When it exceeds max value, it **wraps around**
- No exception
- Program continues silently

This is called **overflow wraparound**.

---

##### Checked arithmetic

```csharp
checked
{
    int x = int.MaxValue;
    x = x + 1;
}
```

What happens:

- CLR inserts overflow checks
- Runtime throws `OverflowException`
- Program stops (unless handled)

---

##### Why unchecked is default

Performance.

- Overflow checks cost CPU instructions
- Games and low-level systems prefer speed
- You opt-in to safety only where needed

---

##### Where `checked` is commonly used

- Financial calculations
- Score systems that must never overflow
- Debug builds

---

#### 3. “Overflow wraps silently by default”

This is the **consequence** of not using `checked`.

##### Example

```csharp
int bullets = int.MaxValue;
bullets++;
```

Result:

```
bullets = -2147483648
```

No warning.
No error.
No crash.

That’s what “wraps silently” means:

- Value wraps to the lowest representable value
- No notification

---

##### Visualizing wraparound

```
int range:

-2147483648  ....  0  ....  2147483647
        ↑                      ↓
        └──────── wrap ────────┘
```

---

### `float`

```csharp
float speed = 5.5f;
```

Why Unity uses `float` everywhere:

- GPU and physics engines are optimized for 32-bit floats
- Smaller memory footprint
- Faster SIMD (Single Instruction, Multiple Data) operations

**Precision Trap**

```csharp
float a = 0.1f + 0.2f; // NOT exactly 0.3
```

Never compare floats directly:

```csharp
Mathf.Abs(a - b) < 0.0001f
```

**Unity Rule:**

- Use `float` for positions, rotations, time
- Never use `double` unless doing financial or scientific math (rare in games)

---

### `bool`

```csharp
bool isJumping = false;
```

Use booleans for **state**, not logic abuse.
If you need more than 2 states → use `enum`.

---

### `string`

```csharp
string playerName = "Buddy";
```

Important truths:

- `string` is **immutable**
- Every modification creates a **new object**
- Heavy GC pressure in Update loops

Bad:

```csharp
void Update()
{
    text += Time.deltaTime; // GC spike
}
```

Good:

```csharp
StringBuilder sb = new StringBuilder();
```

**Unity Tip:**
Avoid string concatenation inside `Update`, `LateUpdate`, `FixedUpdate`.

---

## 3. `var` — Compiler Inference, Not Dynamic Typing

### What `var` REALLY Does

```csharp
var x = 10;       // int
var y = 5.5f;     // float
var z = "Hello";  // string
```

- Type is resolved **at compile time**
- No runtime cost
- Still strongly typed

### When to Use `var`

| Use Case                     | Recommendation       | Example                                                | Why                                        |
| ---------------------------- | -------------------- | ------------------------------------------------------ | ------------------------------------------ |
| Obvious RHS type             | Use `var`            | `var count = 10;`                                      | RHS clearly shows `int`, no ambiguity      |
| Obvious Unity type           | Use `var`            | `var rb = GetComponent<Rigidbody>();`                  | Method name guarantees the type            |
| LINQ queries                 | Required             | `var aliveEnemies = enemies.Where(e => e.IsAlive);`    | Anonymous types have no explicit name      |
| Long generic types           | Improves readability | `var dict = new Dictionary<int, List<PlayerStats>>();` | Explicit type is noisy and adds no value   |
| Loop variables               | Use `var`            | `for (var i = 0; i < 10; i++)`                         | Type is obvious, cleaner                   |
| Public fields                | Avoid                | `public int health;`                                   | API clarity matters                        |
| Unity serialized fields      | Avoid                | `[SerializeField] private float speed;`                | Inspector + serialization need clarity     |
| Ambiguous values             | Avoid                | `var data = Load();`                                   | Type unclear without jumping to definition |
| Method return not obvious    | Avoid                | `var result = GetValue();`                             | Hurts readability and review               |
| Numeric literals with intent | Avoid                | `float damage = 10f;`                                  | `var damage = 10;` would infer `int`       |
| Interfaces as intent         | Avoid                | `IEnemy enemy = new Zombie();`                         | Shows abstraction explicitly               |

**Interview Rule:**
If the type is not obvious in one glance → **don’t use `var`**.

`var` does not affect GC
`var` does not affect serialization
`var` does not make code slower

---

## What `null` actually means?

`null` means **no object**. Not “empty”. Not “default”. It literally means _there is nothing at that reference_. The variable exists, but it points to nowhere.

This is only possible for **reference types** and **nullable value types**.

```csharp
Player p = null;
```

`p` is a reference variable. It does not point to any `Player` object on the heap.

### Why value types cannot be null (by default)

Value types store **data directly**, not references. There is no “nothing” to point to.

```csharp
int x = null;       // Wrong
Vector3 v = null;   // Wrong
```

They always have a value, even if it’s a default one (`0`, `(0,0,0)`).

---

### Nullable value types.

C# allows value types to _opt-in_ to nullability using `Nullable<T>` or `T?`.

```csharp
int? score = null;
Vector3? pos = null;
```

Internally, this is a struct with:
– the value
– a boolean flag (`HasValue`)

This is not free. It adds checks and should not be used casually in hot paths.

---

### NullReferenceException

This is the most common runtime crash in Unity.

```csharp
p.Health = 0; // error - if p is null
```

You are not accessing data. You are dereferencing nothing. The runtime stops you.

Unity makes this worse with destroyed objects.

Unity’s fake null (critical).
In Unity, this is **true**:

```csharp
GameObject go = GetComponent<GameObject>();
Destroy(go);

if (go == null) { ... } // true
```

But internally, `go` is **not actually null**. Unity overrides the `==` operator to return true when the native object is destroyed.

This leads to traps:

```csharp
if (go != null)
{
    go.transform.position = Vector3.zero; // MissingReferenceException
}
```

Best practice:
– Treat destroyed Unity objects as null
– Avoid caching references without lifecycle awareness
– Use `OnDestroy` to clean up references

---

### Null checks: correct patterns

Basic:

```csharp
if (player == null) return;
```

Null-conditional operator:

```csharp
player?.TakeDamage(10);
```

Null-coalescing:

```csharp
int health = player?.Health ?? 0;
```

Do not overuse these. They can hide logic errors.

### Design-level thinking

Null is a **design smell** when overused.

Bad design:

```csharp
Weapon currentWeapon; // may be null
```

Better:

```csharp
Weapon currentWeapon = Weapon.None;
```

Or use state objects, or explicit initialization.

In production code:
– Prefer valid default objects
– Prefer explicit states over “maybe null”
– Use null only to represent true absence

### Interview-ready summary.

“`null` represents the absence of an object reference. Only reference types and nullable value types can be null. Dereferencing null causes runtime exceptions, and in Unity, destroyed objects behave like null via overloaded equality. Good design minimizes null usage and makes object lifetimes explicit.”

---
