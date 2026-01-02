## 1️. Arrays (`T[]`)

Arrays are the **lowest-level collection** you’ll use in C#. Everything else (`List`, `Dictionary`, etc.) is built _on top of concepts introduced here_.

---

## What is an Array?

An array is:

- A **fixed-size**, **contiguous block of memory**
- Holds **elements of the same type**
- Indexed (zero-based)
- Reference type in C#

```csharp
int[] numbers = new int[5];
```

Memory is allocated **once**. Size **cannot change**.

---

## Key Characteristics

### 1. Fixed Size (Critical)

```csharp
int[] arr = new int[3];
arr[0] = 10;
arr[1] = 20;
arr[2] = 30;
// arr[3] runtime error
```

You **cannot**:

- Add
- Remove
- Resize

This is why `List<T>` exists.

---

### 2. Zero-Based Indexing

```csharp
arr[0]  // first element
arr[arr.Length - 1] // last element
```

Accessing invalid index → **IndexOutOfRangeException**

---

### 3. Arrays Are Reference Types

```csharp
int[] a = { 1, 2, 3 };
int[] b = a;

b[0] = 99;
Console.WriteLine(a[0]); // 99
```

Both variables point to **same memory**.

This is a **classic interview trap**.

---

### 4. Contiguous Memory (Performance Impact)

Because elements are stored side-by-side:

- Very fast access: **O(1)**
- CPU cache friendly
- Ideal for math-heavy logic (Unity, physics, rendering)

This is why Unity loves arrays internally.

---

## Creating Arrays

### 1. Fixed Size

```csharp
int[] scores = new int[10];
```

### 2. Inline Initialization

```csharp
int[] scores = { 10, 20, 30 };
```

### 3. With Explicit Type

```csharp
int[] scores = new int[] { 10, 20, 30 };
```

---

## Iterating Arrays

### `for` (preferred in performance code)

```csharp
for (int i = 0; i < arr.Length; i++)
{
    Console.WriteLine(arr[i]);
}
```

### `foreach` (cleaner, slightly slower)

```csharp
foreach (int value in arr)
{
    Console.WriteLine(value);
}
```

In **Unity Update loops**, prefer `for`.

---

## Multidimensional Arrays

### 2D Array

```csharp
int[,] grid = new int[3, 3];
grid[0, 0] = 1;
```

Memory is still contiguous, but access is slower than 1D arrays.

Unity **rarely** uses multidimensional arrays.
Use **1D array + index math** instead.

---

## Jagged Arrays (Array of Arrays)

```csharp
int[][] jagged = new int[3][];
jagged[0] = new int[2];
jagged[1] = new int[5];
```

- Each inner array is separate
- Not contiguous
- Flexible sizes

Used when rows differ in size.

---

## Time & Space Complexity

| Operation         | Complexity    |
| ----------------- | ------------- |
| Access by index   | O(1)          |
| Search (unsorted) | O(n)          |
| Insert/Delete     | Not supported |
| Memory            | Fixed         |

---

## Unity-Specific Usage

### Serialized Array in Inspector

```csharp
[SerializeField]
private GameObject[] enemies;
```

Why arrays here?

- Inspector-friendly
- Known size
- No runtime resizing → less GC pressure

---

### Performance-Critical Systems

Unity uses arrays internally for:

- Mesh vertices
- Physics buffers
- Jobs & Burst
- NativeArray (low-level array)

Arrays = **deterministic memory**, no surprises.

---
