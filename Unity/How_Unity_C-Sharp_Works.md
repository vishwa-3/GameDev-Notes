# 1. What Unity _Really_ Is (Architecturally)

Unity is **not a C# engine**.

Unity is a **native C++ engine** that embeds a **.NET runtime** to execute your C# scripts.

Key fact you must lock in:

> **Unity runs C++ code first. Your C# runs _inside_ Unity.**

So the real stack looks like this:

```
Your C# Scripts
↓
Managed Runtime (Mono or IL2CPP)
↓
Unity Native Engine (C++)
↓
Operating System
↓
Hardware (CPU / GPU / RAM)
```

Your scripts do **not** control the program.
Unity does.

---

# 2. Why Unity Uses C# at All

Unity chose C# because it offers:

- Fast iteration
- Strong tooling
- Managed memory
- Safe scripting for designers
- Cross-platform support

But performance-critical systems remain in **C++**:

- Rendering
- Physics
- Animation
- Audio
- Input
- Job system core

Your C# code is **glue + gameplay logic**, not engine logic.

---

# 3. Unity’s Two Execution Backends (Critical Concept)

Unity can run your C# code in **two completely different ways**:

## 1️. Mono (JIT-based)

## 2️. IL2CPP (AOT (Ahead-Of-Time) - based)

These are **not interchangeable details**. They affect performance, debugging, memory, and builds.

---

# 4. Mono Backend (Editor + Some Platforms)

### What Mono Is

Mono is:

- An open-source .NET runtime
- Uses **JIT compilation**
- Similar to standard CLR behavior

### How Mono Executes Your Code

```
C# Scripts
↓
Compiled to IL
↓
Mono Runtime loads assemblies
↓
JIT compiles methods
↓
CPU executes native code
```

### Why Unity Uses Mono in Editor

- Fast compile times
- Hot reload
- Better debugging
- Reflection-friendly

### Downsides of Mono

- JIT overhead
- Slower startup
- GC spikes
- Platform restrictions (iOS disallows JIT)

Mono is **development-friendly**, not deployment-perfect.

---

# 5. IL2CPP Backend (Production Builds)

IL2CPP changes everything.

### What IL2CPP Actually Does

IL2CPP means:

```
C# → IL → C++ → Native Binary
```

Step-by-step:

1. C# scripts compile to IL
2. IL is converted to **C++ source code**
3. C++ is compiled by a platform compiler
4. Final output is **pure native code**

There is **no managed runtime executing IL at runtime**.

---

### Why IL2CPP Exists

- iOS forbids JIT
- Better performance predictability
- Harder to reverse engineer
- Better platform compliance

IL2CPP is **mandatory** for:

- iOS
- Consoles
- Many mobile builds

---

### Consequences of IL2CPP (Very Important)

- No runtime code generation
- Reflection is limited
- `System.Reflection.Emit` is not supported
- Generic code may be stripped
- Debugging is harder
- Build times are longer

This is why bad architecture breaks in IL2CPP builds.

---

# 6. How Unity Compiles Your Scripts (Internally)

Unity does **not** compile scripts one by one.

It groups scripts into **assemblies**.

### Default Assemblies

| Assembly Name                | Purpose           |
| ---------------------------- | ----------------- |
| `Assembly-CSharp.dll`        | Your game scripts |
| `Assembly-CSharp-Editor.dll` | Editor-only code  |
| `UnityEngine.dll`            | Engine API        |
| `UnityEditor.dll`            | Editor API        |

These are **real .NET assemblies**.

---

### Assembly Definition Files (`.asmdef`)

`.asmdef` lets you:

- Split code into modules
- Control dependencies
- Reduce compile times
- Enforce architecture boundaries

Production Unity projects **always** use asmdefs.

If you don’t, your project **will not scale**.

---

# 7. How MonoBehaviour Actually Works

`MonoBehaviour` is **not magic**.

It is:

- A managed C# object
- Backed by a **native C++ object**

When you write:

```csharp
public class Player : MonoBehaviour
```

Unity:

1. Creates a C++ engine object
2. Creates a C# wrapper
3. Links them via an internal handle

Destroying one incorrectly causes bugs.

This is why:

- `new MonoBehaviour()` is illegal
- Unity controls object lifetime

---

# 8. The Script Lifecycle (Real Execution Order)

Unity does **not** call your methods arbitrarily.

Execution is driven by the **engine loop**.

Simplified engine loop:

```
Initialize Engine
↓
Load Scene
↓
Awake()
↓
OnEnable()
↓
Start()
↓
[Loop Begins]
    Update()
    FixedUpdate()
    LateUpdate()
    Rendering
[Loop Ends]
↓
OnDisable()
↓
OnDestroy()
```

Your code is **callbacks**, not control flow.

You react. Unity drives.

---

# 9. Memory Management in Unity (Where Devs Fail)

Unity has **two memory worlds**:

## Managed Memory

- C# objects
- Garbage collected
- Controlled by Mono / IL2CPP

## Native Memory

- Textures
- Meshes
- Audio
- Engine systems
- NOT managed by GC

This means:

```csharp
Destroy(texture);
```

≠ memory freed immediately

You must understand:

- Allocation patterns
- GC pressure
- Object pooling
- Struct vs class usage

Frame drops come from **ignorance here**, not GPU.

---

# 10. Why `Update()` Is Dangerous

`Update()` runs:

- Every frame
- On the main thread
- Under strict time limits

Bad practices:

- Allocating memory
- LINQ
- `new` in loops
- Boxing
- String concatenation

These cause:

- GC spikes
- Stutters
- Mobile crashes

Professional Unity code treats `Update()` as **radioactive**.

---

# 11. Managed ↔ Native Boundary Cost

Every time you call:

```csharp
transform.position
```

You cross:

- C# → C++

This boundary has a **cost**.

Unity optimizes heavily, but:

- Excessive calls hurt
- Caching references matters
- Batch operations are faster

This is why ECS and Jobs exist.

---

# 12. Why Unity Uses Jobs, Burst, ECS

Classic MonoBehaviour:

- Single-threaded
- GC-heavy
- Cache-unfriendly

Modern Unity:

- Jobs = multithreading
- Burst = native compilation
- ECS = data-oriented design

These bypass many managed limitations.

You cannot avoid this knowledge long-term.

---

# 13. Unity Is Deterministic Only If You Make It

Unity does **not** guarantee:

- Order of execution (unless specified)
- Floating-point determinism
- GC timing

Production code:

- Controls script execution order
- Avoids hidden allocations
- Uses fixed-step logic carefully

---

# 14. Final Mental Model (Lock This In)

> Unity is a **C++ engine**
> C# is a **managed scripting layer**
> Unity owns the loop
> You write reactions
> Performance lives at boundaries

---
