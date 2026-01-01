## **Stack**

The stack is **automatic, scoped memory**. It is extremely fast. Memory is allocated and freed simply by moving a pointer. No GC, no bookkeeping, no fragmentation. Stack memory is tied to **method execution**. When a method is called, a stack frame is created. When the method returns, the entire frame is destroyed instantly.

What lives on the stack?
– Local variables of **value types**
– Copies of value-type parameters
– References (pointers) to heap objects
– Return addresses and call metadata

Example:

```csharp
void Move()
{
    int speed = 5;
    Vector3 dir = Vector3.forward;
    Player p = playerRef;
}
```

Here, `speed` and `dir` are stored directly in the stack frame. `p` is just a reference on the stack; the actual `Player` object is somewhere else called **Heap**.

---

## **Heap**

The heap is **long-lived, shared memory**. Allocation is slower. Deallocation is non-deterministic and handled by the **Garbage Collector**. Objects live here until nothing references them anymore. This is where performance problems come from if you allocate carelessly.

What lives on the heap?
– All **class instances**
– Arrays
– Strings
– Boxing allocations
– Captured variables in closures

Example:

```csharp
Player player = new Player();
```

`player` (the reference) is on the stack.
The `Player` object itself is on the heap.

---

## Key difference: lifetime control

Stack lifetime is **deterministic**. You know exactly when memory dies: end of scope.
Heap lifetime is **non-deterministic**. Memory is freed when GC runs, not when you want.

This is why GC spikes cause frame drops in Unity.

Value types vs reference types (the mental model).
Value type = the data itself. Copy means copy the data.
Reference type = a pointer to data. Copy means copy the pointer.

```csharp
Vector3 a = new Vector3(1,2,3);
Vector3 b = a;
b.x = 10; // a unchanged
```

```csharp
Player a = new Player();
Player b = a;
b.Health = 0; // a.Health also 0
```

This behavior directly comes from stack vs heap semantics.

---

## Passing to methods

C# is always **pass-by-value**. The confusion comes from _what_ is being copied.

```csharp
void Modify(Vector3 v) { v.x = 5; }
void Modify(Player p) { p.Health = 0; }
```

In the first case, the vector data is copied. No effect outside.
In the second case, the **reference** is copied, but both references point to the same heap object.

---

## Unity-specific performance reality

Every frame, Unity runs thousands of operations. Stack allocations are basically free. Heap allocations are dangerous. This is why:
– `Vector3`, `Quaternion`, `Color` are structs
– `Transform`, `GameObject`, `MonoBehaviour` are classes
– Allocating in `Update()` is a red flag
– Boxing a struct silently allocates on the heap

Example of hidden heap allocation:

```csharp
void Update()
{
    object o = transform.position; // boxing → heap alloc
}
```

---

## Interview-ready one-liner

“The stack provides fast, deterministic memory for scoped value data, while the heap stores long-lived objects managed by GC. In Unity, avoiding heap allocations in hot paths is critical to prevent GC spikes.”

---
