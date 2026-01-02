## 6️. `Stack<T>`

A `Stack<T>` represents:

> **Last-In, First-Out (LIFO)** processing

Think:

- Last added → first removed
- Like undo/redo or function calls

---

## What is `Stack<T>`?

- LIFO collection
- No indexing
- Fast push & pop
- Reference type
- Namespace: `System.Collections.Generic`

```csharp
Stack<int> stack = new Stack<int>();
```

---

## Core Operations

### Push (Add to top)

```csharp
stack.Push(10);
stack.Push(20);
```

### Pop (Remove from top)

```csharp
int value = stack.Pop();
```

Throws exception if empty.

Safe usage:

```csharp
if (stack.Count > 0)
{
    int value = stack.Pop();
}
```

---

### Peek (Look at top)

```csharp
int top = stack.Peek();
```

Does NOT remove element.

---

## Time Complexity

| Operation | Complexity |
| --------- | ---------- |
| Push      | O(1)       |
| Pop       | O(1)       |
| Peek      | O(1)       |

Internally backed by an array, similar to `List<T>` but restricted access.

---

## Internal Behavior

- Uses array
- Grows when capacity exceeded
- No shifting, just index increment/decrement

Very efficient.

---

## Iteration Order

```csharp
foreach (var item in stack)
{
    Debug.Log(item);
}
```

Iteration goes **from top to bottom** (LIFO order).

This surprises people in interviews.

---

## Common Unity Use Cases

### 1. Undo / Redo Systems

```csharp
Stack<ICommand> undoStack;
Stack<ICommand> redoStack;
```

---

### 2. State Rollback

```csharp
Stack<PlayerState> stateHistory;
```

---

### 3. Depth-First Search (DFS)

```csharp
Stack<Node> dfsStack;
```

---

## Stack vs Queue

| Feature  | Stack              | Queue                 |
| -------- | ------------------ | --------------------- |
| Order    | LIFO               | FIFO                  |
| Use case | Undo, backtracking | Scheduling, pipelines |
| Removal  | Last               | First                 |

---

## Common Mistakes

Using Stack when order should be preserved
Expecting random access
Popping without checking Count

---

## Unity Performance Notes

- Very cheap operations
- Good for temporary logic
- Avoid recreating every frame

---
