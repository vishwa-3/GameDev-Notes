## 10. `IDictionary<TKey, TValue>`

`IDictionary<TKey, TValue>` is:

> The **contract for key–value collections**

It is the **abstraction behind `Dictionary<TKey, TValue>`**.

---

## Hierarchy (IMPORTANT)

```
IEnumerable<KeyValuePair<TKey, TValue>>
          ↑
IDictionary<TKey, TValue>
          ↑
Dictionary<TKey, TValue>
```

---

## What is `IDictionary<TKey, TValue>`?

```csharp
public interface IDictionary<TKey, TValue>
    : ICollection<KeyValuePair<TKey, TValue>>
{
    TValue this[TKey key] { get; set; }

    ICollection<TKey> Keys { get; }
    ICollection<TValue> Values { get; }

    void Add(TKey key, TValue value);
    bool Remove(TKey key);
    bool ContainsKey(TKey key);
    bool TryGetValue(TKey key, out TValue value);
}
```

This defines **what a dictionary can do**, not how.

---

## Key Capabilities

`IDictionary` guarantees:

- Key-based access
- Unique keys
- Add / Remove by key
- Safe lookup (`TryGetValue`)
- Access to Keys & Values collections

It does **not** guarantee:

- Order
- Hashing (implementation detail)

---

## Why Use `IDictionary` Instead of `Dictionary`?

### API Design (PRO LEVEL)

Bad:

```csharp
void LoadData(Dictionary<int, Enemy> enemies)
```

Better:

```csharp
void LoadData(IDictionary<int, Enemy> enemies)
```

Why?

- Accepts Dictionary or custom implementations
- Less coupling
- More testable code

This is **clean architecture**, not theory.

---

## Indexer vs Methods

```csharp
dict[key] = value;      // add or overwrite
dict.Add(key, value);  // throws if key exists
```

Indexer is part of `IDictionary`.

---

## Keys & Values Collections

```csharp
ICollection<TKey> keys = dict.Keys;
ICollection<TValue> values = dict.Values;
```

Notes:

- Not snapshots
- Reflect dictionary changes
- No ordering guarantee

---

## Iteration

```csharp
foreach (KeyValuePair<TKey, TValue> kvp in dict)
{
    Debug.Log(kvp.Key + " => " + kvp.Value);
}
```

Order is undefined unless using ordered implementation.

---

## Unity Use Cases

### Service Locators

```csharp
IDictionary<Type, IService> services;
```

### State Machines

```csharp
IDictionary<StateId, IState> states;
```

### Caches

```csharp
IDictionary<int, GameObject> cache;
```

---

## Common Mistakes

Exposing Dictionary when IDictionary is enough
Assuming iteration order
Using indexer without key existence check

---

## Performance Notes

- Interface dispatch cost is negligible
- Use IDictionary at boundaries
- Use Dictionary internally in hot paths

---
