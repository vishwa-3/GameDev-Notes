## 1. Core Difference

> **Value types store data directly. Reference types store a reference (pointer) to data.**

---

## 2. What are Value Types?

Value types:

- Store the **actual data**
- Are **copied on assignment**
- Usually live on the **stack** (or inline in objects)
- Cannot be `null` (unless `Nullable<T>`)

### Examples

```csharp
int
float
bool
struct
enum
Vector3   // Unity struct
```

### Example: Copy behavior

```csharp
int a = 10;
int b = a;
b = 20;

Debug.Log(a); // 10
```

`a` and `b` are **independent copies**.

---

### Struct example (very important)

```csharp
struct Damage
{
    public int value;
}

Damage d1 = new Damage { value = 10 };
Damage d2 = d1;

d2.value = 20;

Debug.Log(d1.value); // 10
```

No shared state.

---

## 3. What are Reference Types?

Reference types:

- Store a **reference to an object**
- Assignment copies the **reference**, not the object
- Objects live on the **heap**
- Can be `null`

### Examples

```csharp
class
string
array
List<T>
MonoBehaviour
GameObject
```

### Example: Reference behavior

```csharp
class Player
{
    public int health;
}

Player p1 = new Player { health = 100 };
Player p2 = p1;

p2.health = 50;

Debug.Log(p1.health); // 50
```

Both variables point to the **same object**.

---

## 4. Memory Visualization (simple)

### Value type

```
a: [10]
b: [20]
```

### Reference type

```
p1 ─┐
    ├──> [ Player { health = 50 } ]
p2 ─┘
```

---

## 5. Passing to Methods

### Value type passed normally

```csharp
void Increase(int x)
{
    x++;
}

int a = 5;
Increase(a);

Debug.Log(a); // 5
```

Copy is passed.

---

### Reference type passed normally

```csharp
void Damage(Player p)
{
    p.health -= 10;
}

Damage(p1);
```

You modify the **same object**.

---

### Reassigning reference

```csharp
void Replace(Player p)
{
    p = new Player();
}

Replace(p1);
```

This does **not** affect `p1`.
The reference is copied, not replaced.

---

## 6. Unity-Specific Value vs Reference

### `Vector3` is a struct (value type)

```csharp
transform.position.x = 5; // does NOT work
```

Why?

- `position` returns a **copy**
- You modify the copy

Correct:

```csharp
Vector3 pos = transform.position;
pos.x = 5;
transform.position = pos;
```

This single rule trips up many Unity devs.

---

### `Transform` is a reference type

```csharp
Transform t = transform;
t.position = Vector3.zero;
```

Works because `Transform` is a class.

---

## 7. Performance Implications

### Value types

- No GC
- Fast allocation
- Large structs → expensive copies

### Reference types

- GC-managed
- Indirection cost
- Shared state risks

**Unity rule:**

- Small, immutable data → struct
- Large, mutable data → class

---

## 8. `string` – special case

`string` is:

- A reference type
- Immutable

```csharp
string a = "Hi";
string b = a;

b += "!";
```

A new string is created.
Original is untouched.

---

## 9. **Copying vs sharing data**

Copying data means **each variable owns its own data**. Sharing data means **multiple variables point to the same data**. In C#, the difference is determined by **value types vs reference types**, not by assignment syntax.

### Copying: value types (structs)

When you assign a value type, the **entire data is copied**.

```csharp
Vector3 a = new Vector3(1, 2, 3);
Vector3 b = a;     // copy
b.x = 10;
```

After this, `a.x` is still `1`. `b` is an independent copy. This is why `Vector3` is safe to pass around freely. No one can accidentally mutate your data unless you explicitly assign it back.

This behavior is critical for math correctness. A vector is just numbers. It should not have shared identity.

### Sharing: reference types (classes)

When you assign a reference type, **only the reference is copied**, not the object.

```csharp
Player a = new Player();
Player b = a;     // shared reference
b.Health = 0;
```

Now `a.Health` is also `0`. There is only one `Player` object in memory. Both variables point to it. This is intentional. Entities in a game have identity and shared state.

This is not “pass-by-reference magic”. It is simple pointer sharing.

### Interview-ready summary

“Value types copy data on assignment, giving isolation and safety. Reference types share data by copying references, enabling shared mutable state. Choosing correctly avoids bugs and performance issues, especially in Unity’s frame loop.”

---
