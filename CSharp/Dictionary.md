## 3️. `Dictionary<TKey, TValue>`

A `Dictionary` is **not** a list.
It is a **hash table**.

Think:

> “I want a value **by key**, not by position.”

---

## What is `Dictionary<TKey, TValue>`?

- Stores **key–value pairs**
- Keys are **unique**
- Order is **not guaranteed**
- Lookup by key is extremely fast
- Reference type
- Namespace: `System.Collections.Generic`

```csharp
Dictionary<int, string> players = new Dictionary<int, string>();
```

---

## How It Works Internally

Internally:

- Uses a **hash table**
- Each key is passed to `GetHashCode()`
- Hash → index in internal buckets array

Flow:

```
Key → HashCode → Bucket → Entry → Value
```

If you don’t understand hashing, you will misuse dictionaries.

---

## Adding Elements

```csharp
players.Add(1, "Alice");
players[2] = "Bob";
```

Difference:

```csharp
dict.Add(key, value); // throws exception if key exists
dict[key] = value;   // overwrites if key exists
```

---

## Accessing Elements

```csharp
string name = players[1]; // throws if key missing
```

Safe version (USE THIS):

```csharp
if (players.TryGetValue(1, out string name))
{
    Debug.Log(name);
}
```

`TryGetValue` avoids exceptions → faster + cleaner.

---

## Time Complexity (Average Case)

| Operation | Complexity |
| --------- | ---------- |
| Add       | O(1)       |
| Lookup    | O(1)       |
| Remove    | O(1)       |

Worst case: O(n)

---

## Hash Collisions

Two different keys → same hash code.

Dictionary handles this by:

- Chaining entries
- Comparing keys with `Equals()`

This is why **immutable keys** are important.

---

## Key Rules

### 1. Keys must be immutable

Bad:

```csharp
class Player
{
    public int id;
}
```

Changing `id` after insertion breaks dictionary.

Good keys:

- `int`
- `string`
- `enum`
- Structs with stable values

---

### 2. Override `GetHashCode` & `Equals`

```csharp
struct GridPos
{
    public int x, y;

    public override int GetHashCode()
        => HashCode.Combine(x, y);

    public override bool Equals(object obj)
        => obj is GridPos other && x == other.x && y == other.y;
}
```

Miss this → dictionary bugs that are painful to debug.

---

## Removing Elements

```csharp
dict.Remove(key);
```

Returns `true` if removed.

---

## Iteration

```csharp
foreach (var kvp in dict)
{
    Debug.Log(kvp.Key + " : " + kvp.Value);
}
```

Order is **not guaranteed**.
Never rely on it.

---

## Unity Use Cases

### Fast Lookups

```csharp
Dictionary<int, Enemy> enemyById;
```

### Object Pools

```csharp
Dictionary<GameObject, PoolData> poolMap;
```

### State Machines

```csharp
Dictionary<StateId, IState> states;
```

---

## Unity Performance Warnings

Using `Dictionary` in tight loops without reason
Using complex objects as keys
Rebuilding dictionaries every frame

If order matters → dictionary is wrong tool.

---

## Dictionary vs List

| Use Case          | Choose     |
| ----------------- | ---------- |
| Ordered data      | List       |
| Index-based       | List       |
| Key-based lookup  | Dictionary |
| Fast search by ID | Dictionary |

---
