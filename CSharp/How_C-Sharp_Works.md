# 1. What C# _Really_ Is?

C# is **not** a language that talks directly to your CPU.
It is a **managed, statically typed language** that is designed to run **inside a runtime environment** provided by **.NET**.

That means:

- Your C# code **never becomes raw machine instructions immediately**
- It is first converted into a **CPU-independent format**
- Execution is **controlled and supervised** by a runtime

This design trades **raw control** for:

- Safety
- Portability
- Productivity
- Runtime services (GC, threading, security)

> **Key mental model**:
> C# code runs _inside_ something, not _on_ the hardware.

---

# 2. Why C# Cannot Run Alone

Languages like **C** or **C++** compile straight into **native machine code**.
C# does **not**.

C# depends on **.NET**, specifically the **CLR (Common Language Runtime)**.

Without the CLR:

- A C# program **cannot start**
- Even a simple `Console.WriteLine` is meaningless

So the real execution chain is:

```
Your Code → .NET Runtime → OS → Hardware
```

You never bypass the runtime.

---

# 3. The Three Major Layers You Are Always Using

Whenever you write C#, you are operating across **three layers**, even if you don’t see them.

### Layer 1: Your C# Code

This is what _you_ write:

- Classes
- Methods
- Variables
- Logic

This code is **high-level**, human-friendly, and safe.

---

### Layer 2: The .NET Runtime (CLR)

This is the **engine** that makes C# work.

The CLR:

- Loads your compiled code
- Verifies type safety
- Manages memory
- Converts IL to machine code
- Handles crashes and exceptions

Without the CLR, C# is just text.

---

### Layer 3: Operating System + Hardware

The CLR itself:

- Runs as a **native process**
- Calls the OS for memory, threads, files
- Ultimately uses CPU instructions

So **your code never touches the OS directly**.

---

# 4. What Happens When You Compile C#

When you press **Build**, this happens:

### Step 1: Syntax & Type Checking

The compiler:

- Validates syntax
- Verifies types
- Ensures rules like:

  - No calling methods that don’t exist
  - No assigning `string` to `int`

Errors here = **compile-time errors**.

---

### Step 2: C# → IL Conversion

Your `.cs` files are converted into **IL (Intermediate Language)**.

Important properties of IL:

- Not human-friendly
- Not machine code
- CPU-agnostic
- Designed for execution by CLR

This is why C# is **portable**.

---

### Step 3: Assembly Generation

The compiler bundles IL + metadata into an **assembly**.

An assembly contains:

- IL instructions
- Type definitions
- Method signatures
- References to other assemblies
- Version information

Assemblies are stored as:

- `.exe`
- `.dll`

---

# 5. What an EXE Really Means in C#

A C# `.exe` is **not a real executable** in the traditional sense.

It is:

- A container for IL
- A signal to Windows: _“Load CLR to run this”_

Execution flow when you double-click a C# exe:

1. Windows sees it’s a .NET app
2. Loads the CLR
3. CLR loads your assembly
4. CLR finds `Main()`
5. Execution begins

> The CPU never executes IL directly.

---

# 6. What a DLL Really Is?

A `.dll` in .NET is: (Dynamic Link Library)

- Compiled IL code
- Reusable logic
- No entry point

DLLs:

- Cannot start programs
- Must be loaded by another assembly

Unity uses DLLs extensively:

- Engine code
- Packages
- Plugins
- Your compiled game logic

Your scripts are ultimately compiled into **assemblies**, not individual files.

---

# 7. The CLR: The Actual Boss of Your Program

The **Common Language Runtime** controls execution from start to end.

It is responsible for:

### 1. Loading Assemblies

- Resolves dependencies
- Loads required DLLs
- Ensures version compatibility

---

### 2. JIT Compilation (Critical)

IL is **not executable**.

The CLR:

- Converts IL → native machine code
- Does this **method by method**
- Only compiles what is actually used

This is called **Just-In-Time compilation**.

Benefits:

- CPU-specific optimization
- Cross-platform support
- Smaller binaries

Cost:

- First call is slower (warm-up)

Unity performance issues often start here.

---

### 3. Garbage Collection (Memory Management)

C# uses **automatic memory management**.

Process:

- You allocate objects
- CLR tracks references
- Unused objects are collected

You do **not**:

- Free memory
- Delete objects manually

But:

- Excess allocations = GC spikes
- GC pauses = frame drops (Unity!)

Understanding GC is **mandatory** for production Unity code.

---

# 8. Managed vs Unmanaged Code (Why This Matters)

### Managed Code

- Runs under CLR
- Memory is automatic
- Safer
- Slight overhead

### Unmanaged Code

- Runs directly on OS
- Manual memory
- Faster but dangerous

C# is managed, but it **can cross the boundary**.

---

# 9. P/Invoke (C# Calling Native Code)

C# can call native C/C++ code using **Platform Invocation Services**.

Why this exists:

- OS APIs
- Legacy libraries
- Performance-critical native code

Mechanism:

- C# declares external methods
- CLR marshals data
- Native code executes

This is **advanced**, risky, and performance-sensitive.

Unity uses this internally.

---

# 10. Program Entry Point (Main)

Every C# program starts at **exactly one place**.

```csharp
static void Main(string[] args)
```

CLR behavior:

- Searches for `Main`
- Starts execution
- Owns lifetime
- Cleans up on exit

Unity is special: **Unity controls Main**, not you.

---

# 11. Build Output: What Actually Gets Produced

After compilation, you get:

- `.exe` → entry assembly
- `.dll` → logic assembly
- `.runtimeconfig.json` → runtime instructions

That JSON file tells the CLR:

- Which .NET version
- Runtime settings
- GC configuration

Delete it → program breaks.

---

# 12. End-to-End Execution (Final Mental Model)

```
You write C#
↓
Compiler checks types
↓
C# → IL
↓
Assembly created
↓
CLR loads assembly
↓
JIT compiles methods
↓
CPU executes native code
↓
GC manages memory
↓
Program exits
```

This is the **real pipeline**.

---
