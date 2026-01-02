## 8️. `ICollection<T>`

`ICollection<T>` is:

> A **mutable collection contract**

It sits **between** `IEnumerable<T>` and concrete collections like `List<T>`.

---

## Hierarchy

```
IEnumerable<T>
   ↑
ICollection<T>
   ↑
IList<T>
   ↑
List<T>
```

This hierarchy is **interview-critical**.

---

## What is `ICollection<T>`?

It represents:

- A group of elements
- You can **add and remove**
- You can ask **how many**

```csharp
public interface ICollection<T> : IEnumerable<T>
{
    int Count { get; }
    bool IsReadOnly { get; }

    void Add(T item);
    bool Remove(T item);
    void Clear();
    bool Contains(T item);
    void CopyTo(T[] array, int index);
}
```

---

## Key Capabilities

`ICollection<T>` guarantees:

- Enumeration
- `Count`
- Add / Remove
- Clear
- Contains

It does **not** guarantee:

- Indexing
- Ordering
- Key-based access

---

## What Implements `ICollection<T>`?

- `List<T>`
- `HashSet<T>`
- `Queue<T>`
- `Stack<T>`
- `Dictionary<TKey, TValue>.Values`
- `Dictionary<TKey, TValue>.Keys`

---

## Why Use `ICollection<T>`?

### Design Reason (VERY IMPORTANT)

Bad:

```csharp
void Process(List<Enemy> enemies)
```

Better:

```csharp
void Process(ICollection<Enemy> enemies)
```

Why?

- Accepts List, HashSet, Queue, etc.
- Less coupling
- More reusable code

This is **clean architecture** thinking.

---

## Example: Generic Logic

```csharp
void ClearDead(ICollection<Enemy> enemies)
{
    enemies.RemoveWhere(e => e.IsDead); // works for HashSet
}
```

But careful:

- `RemoveWhere` exists only on `HashSet`
- Interface only exposes common behavior

---

## Read-only Collections

```csharp
bool readOnly = collection.IsReadOnly;
```

Some collections (like wrappers) prevent modification.

---

## Unity Usage

### Exposing mutable but abstract collections

```csharp
public void RegisterEnemies(ICollection<Enemy> enemies)
{
    _enemies = enemies;
}
```

Better than locking yourself to `List<T>`.

---

## Common Mistakes

Assuming order exists
Assuming index access
Casting blindly to `List<T>`

If you need index access → wrong interface.

---

## Performance Notes

- Interface calls slightly slower (negligible)
- Use abstraction at **boundaries**, not hot loops
- Concrete types inside performance-critical code

---
