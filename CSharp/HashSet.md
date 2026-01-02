## 4️. `HashSet<T>`

A `HashSet<T>` is:

> A collection of **unique values** with **very fast lookup**

Internally, it is **the same hashing concept as Dictionary**, but **only keys, no values**.

Think of it as:

```
Dictionary<T, bool>
```

(Conceptually, not literally.)

---

## What is `HashSet<T>`?

- Stores **unique elements only**
- No duplicates allowed
- Order is **not guaranteed**
- Extremely fast `Contains`
- Reference type
- Namespace: `System.Collections.Generic`

```csharp
HashSet<int> ids = new HashSet<int>();
```

---

## Internal Behavior

- Uses hashing (`GetHashCode`)
- Uses buckets + collision handling
- Same rules as Dictionary keys

If you understand Dictionary, HashSet is easy.

---

## Adding Elements

```csharp
bool added = ids.Add(10);
```

- Returns `true` → element was new
- Returns `false` → element already existed

This return value is **useful**, don’t ignore it.

---

## Removing Elements

```csharp
ids.Remove(10);
```

Returns `true` if removed.

---

## Checking Existence

```csharp
if (ids.Contains(10))
{
    // O(1)
}
```

This is why HashSet exists.

---

## Time Complexity (Average)

| Operation | Complexity |
| --------- | ---------- |
| Add       | O(1)       |
| Remove    | O(1)       |
| Contains  | O(1)       |

Worst case: O(n) (same collision reasoning)

---

## HashSet vs List

### Membership Check

```csharp
list.Contains(x);    // O(n)
hashSet.Contains(x); // O(1)
```

If you do frequent `Contains` checks → **HashSet wins**.

---

## Common Use Cases

### 1. Prevent Duplicates

```csharp
HashSet<GameObject> activeEnemies;
```

No accidental double-adds.

---

### 2. Fast State Flags

```csharp
HashSet<StateId> activeStates;
```

---

### 3. Trigger Tracking

```csharp
HashSet<Collider> collidersInside;
```

Used in `OnTriggerEnter/Exit`.

---

## Set Operations

```csharp
hashSetA.UnionWith(hashSetB);        // A ∪ B
hashSetA.IntersectWith(hashSetB);    // A ∩ B
hashSetA.ExceptWith(hashSetB);       // A - B
```

You won’t do this daily in Unity, but interviewers love it.

---

## Rules for HashSet Keys

Same as Dictionary:

- Must be immutable
- Must have stable `GetHashCode`
- Avoid mutable structs/classes

If key changes → HashSet breaks silently.

---

## Unity Performance Advice

- Prefer `HashSet<T>` over `List<T>` for frequent lookups
- Avoid allocating new HashSets every frame
- Clear and reuse when possible

---
