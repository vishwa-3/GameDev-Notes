## 9️. `IList<T>`

`IList<T>` is:

> A **mutable, ordered, index-based** collection contract

This is the **interface version of List-like behavior**.

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

---

## What is `IList<T>`?

```csharp
public interface IList<T> : ICollection<T>
{
    T this[int index] { get; set; }

    int IndexOf(T item);
    void Insert(int index, T item);
    void RemoveAt(int index);
}
```

This adds:

- Index access
- Order guarantee
- Insert/remove at index

---

## Key Guarantees

`IList<T>` guarantees:

- Elements have order
- Can access by index
- Can insert/remove by index

It does **not** guarantee:

- Fast inserts/removals
- Array-backed storage

Performance depends on implementation.

---

## Who Implements `IList<T>`?

- `List<T>`
- `T[]` (arrays implement `IList<T>` via runtime)
- `ReadOnlyCollection<T>`

Unity uses `List<T>` most often.

---

## Example Usage

### API Design

Bad:

```csharp
void SortEnemies(List<Enemy> enemies)
```

Better:

```csharp
void SortEnemies(IList<Enemy> enemies)
```

Why?

- Accepts List or array
- Caller controls concrete type
- Better abstraction

---

## Index-Based Operations

```csharp
enemies[0] = boss;
enemies.Insert(1, miniBoss);
enemies.RemoveAt(2);
```

Remember:

- Insert / RemoveAt → **O(n)** for List-backed implementations

---

## Unity Performance Warning

Using `IList<T>` in hot loops:

- Interface dispatch
- Possible hidden cost

Rule:

- Use interfaces at **boundaries**
- Use concrete types internally

---

## Common Mistakes

Assuming all IList implementations perform the same
Removing from middle in Update loops
Exposing List when IList is enough

---

## IList vs ICollection

| Feature    | ICollection | IList |
| ---------- | ----------- | ----- |
| Count      | yes         | yes   |
| Add/Remove | yes         | yes   |
| Indexing   | no          | yes   |
| Order      | no          | yes   |

---
