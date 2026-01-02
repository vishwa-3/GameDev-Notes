> we move from **concrete collections** to **abstractions**.

---

## 7️. `IEnumerable<T>`

`IEnumerable<T>` is **not a collection**.
It is a **contract for iteration**.

If you misunderstand this, you will write inefficient LINQ and bad APIs.

---

## What is `IEnumerable<T>`?

It means:

> “This thing can be **iterated** using `foreach`.”

That’s it. Nothing more.

```csharp
public interface IEnumerable<T>
{
    IEnumerator<T> GetEnumerator();
}
```

---

## Key Characteristics

- Read-only iteration
- No indexing
- No Count guarantee
- Deferred execution (often)
- Very lightweight abstraction

---

## `foreach` Works Because of `IEnumerable`

```csharp
foreach (var item in collection)
{
}
```

This only requires `IEnumerable<T>`.

Arrays, List, Dictionary, HashSet — all implement it.

---

## Deferred Execution (CRITICAL)

Many `IEnumerable<T>` sequences **do not execute immediately**.

```csharp
IEnumerable<int> evens =
    numbers.Where(n => n % 2 == 0);
```

Nothing runs yet.

Execution happens **when iterated**:

```csharp
foreach (var n in evens)
{
}
```

---

## Multiple Enumeration Danger

```csharp
IEnumerable<int> seq = GetNumbers();

foreach (var x in seq) { }
foreach (var x in seq) { } // runs again
```

If `GetNumbers()` is expensive → performance bug.

Fix:

```csharp
List<int> cached = seq.ToList();
```

---

## What You CANNOT Do

```csharp
enumerable[0];
enumerable.Add(x);
enumerable.Count;  // (unless LINQ)
```

If you need these → wrong abstraction.

---

## Why Return `IEnumerable<T>`?

Bad API:

```csharp
public List<Enemy> GetEnemies()
```

Good API:

```csharp
public IEnumerable<Enemy> GetEnemies()
```

Why?

- Caller cannot modify internal collection
- Implementation can change later
- Enforces read-only access

This is **professional-level design**.

---

## `yield return`

Creates `IEnumerable` lazily.

```csharp
IEnumerable<int> GetNumbers()
{
    for (int i = 0; i < 5; i++)
        yield return i;
}
```

No list created. Values produced **on demand**.

Unity warning:
Avoid `yield return` in hot paths.

---

## Unity Usage

### Read-only exposure

```csharp
public IEnumerable<Enemy> ActiveEnemies => enemies;
```

### LINQ queries (careful!)

```csharp
var nearby = enemies.Where(e => e.IsNear);
```

LINQ allocates. Use carefully in Update loops.

---

## Common Mistakes

Assuming `IEnumerable` has data stored
Re-enumerating expensive sequences
Using LINQ every frame in Unity

---
