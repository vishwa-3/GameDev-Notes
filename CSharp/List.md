## 2️. `List<T>`

`List<T>` exists because **arrays are too rigid** for most real-world logic.

Think of it as:

> A **resizable array** with safety and convenience.

Internally, `List<T>` **uses an array**. That detail matters for performance and interviews.

---

## What is `List<T>`?

- Generic collection (`List<int>`, `List<GameObject>`)
- **Dynamic size**
- Maintains order
- Reference type
- Located in `System.Collections.Generic`

```csharp
List<int> numbers = new List<int>();
```

---

## Internal Structure

Internally:

```text
List<T>
 ├── T[] _items   ← internal array
 ├── int _size    ← number of elements used
 └── int _version ← modification tracking
```

When capacity is exceeded:

1. New array is allocated (usually **double size**)
2. Old elements are copied
3. Old array is discarded → **GC pressure**

This is why misuse of `List<T>` kills Unity performance.

---

## Adding Elements

```csharp
List<int> list = new List<int>();
list.Add(10);
list.Add(20);
```

### Time Complexity

| Operation       | Complexity |
| --------------- | ---------- |
| Add (amortized) | O(1)       |
| Add (resize)    | O(n)       |
| Insert at index | O(n)       |
| Remove          | O(n)       |

---

## Capacity vs Count

```csharp
list.Count    // number of actual elements
list.Capacity // size of internal array
```

Example:

```csharp
List<int> list = new List<int>(100);
```

Why do this?

- Avoid repeated resizing
- Reduce allocations
- Critical in Unity loops

---

## Accessing Elements

```csharp
int value = list[0]; // O(1)
```

Same speed as arrays **when no resizing happens**.

---

## Removing Elements

```csharp
list.Remove(10);     // removes first occurrence
list.RemoveAt(0);    // removes by index
list.Clear();        // count = 0 (capacity unchanged)
```

Removal shifts elements → O(n)

---

## Iteration

### `for` (Unity-safe)

```csharp
for (int i = 0; i < list.Count; i++)
{
    Debug.Log(list[i]);
}
```

### `foreach` (alloc risk in old Unity)

```csharp
foreach (var item in list)
{
    Debug.Log(item);
}
```

Modern Unity mostly fixed this, but **for is safer** in hot paths.

---

## Preallocating Capacity

Bad:

```csharp
List<Vector3> points = new List<Vector3>();
for (int i = 0; i < 1000; i++)
    points.Add(Vector3.zero);
```

Good:

```csharp
List<Vector3> points = new List<Vector3>(1000);
```

This avoids **multiple array copies**.

---

## `List<T>` in Unity Inspector

By default:

```csharp
[SerializeField]
private List<GameObject> enemies;
```

Unity:

- Serializes lists like arrays
- Allows resizing in Inspector
- Preferred over arrays for designer-driven content

---

## When to Use `List<T>`

Use `List<T>` when:

- Size changes at runtime
- Order matters
- Frequent additions/removals
- You need index access

Do **NOT** use when:

- Fixed-size, performance-critical data
- You know size upfront and it never changes → use array

---
