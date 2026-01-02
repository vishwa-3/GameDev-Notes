## 5️. `Queue<T>`

A `Queue<T>` represents:

> **First-In, First-Out (FIFO)** processing

Think:

- First enters → first leaves
- Like a real queue

This is **not** for random access. It’s for **flow control**.

---

## What is `Queue<T>`?

- FIFO collection
- No indexing by position
- Fast enqueue & dequeue
- Reference type
- Namespace: `System.Collections.Generic`

```csharp
Queue<int> queue = new Queue<int>();
```

---

## Core Operations

### Enqueue (Add to end)

```csharp
queue.Enqueue(10);
queue.Enqueue(20);
```

### Dequeue (Remove from front)

```csharp
int value = queue.Dequeue();
```

Throws exception if empty.

Safe version:

```csharp
if (queue.Count > 0)
{
    int value = queue.Dequeue();
}
```

---

### Peek (Look at front)

```csharp
int next = queue.Peek();
```

Does NOT remove the element.

---

## Time Complexity

| Operation | Complexity |
| --------- | ---------- |
| Enqueue   | O(1)       |
| Dequeue   | O(1)       |
| Peek      | O(1)       |

Internally uses a **circular array**. No shifting like List.

---

## Internal Structure

- Backed by an array
- Uses head & tail indices
- Wraps around when reaching end

This avoids expensive element shifting.

---

## Iteration

```csharp
foreach (var item in queue)
{
    Debug.Log(item);
}
```

Order preserved **during iteration**, but you should **not modify** during iteration.

---

## Common Unity Use Cases

### 1. Action Processing

```csharp
Queue<ICommand> commandQueue;
```

Process one command per frame.

---

### 2. Event Pipelines

```csharp
Queue<GameEvent> eventQueue;
```

---

### 3. AI / Turn Systems

```csharp
Queue<Unit> turnOrder;
```

---

## Queue vs List

| Scenario               | Correct |
| ---------------------- | ------- |
| FIFO behavior          | Queue   |
| Random access          | List    |
| Frequent front removal | Queue   |
| Order matters + index  | List    |

Never remove from index `0` in a List when FIFO is needed. That’s **O(n)** and dumb.

---

## Common Mistakes

Using List and `RemoveAt(0)` instead of Queue
Calling `Dequeue` without checking Count
Expecting index access

---

## Unity Performance Advice

- Queue is allocation-safe after creation
- Reuse queues instead of recreating
- Avoid per-frame construction

---
