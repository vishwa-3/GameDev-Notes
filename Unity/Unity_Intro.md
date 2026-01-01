## **Topic 1: Unity Hub**

### **What is Unity Hub?**

Unity Hub is the **central launcher + manager** for everything in Unity.

You **do NOT** open Unity Editor directly most of the time — you open **Unity Hub**.

---

### **What Unity Hub actually does**

Unity Hub has **4 core responsibilities**:

#### **1️. Manage Unity Editor Versions**

- Install multiple Unity versions (2021, 2022, 2023, Unity 6.x, etc.)
- Each project can use a **different editor version**
- Prevents version conflicts

---

#### **2️. Manage Projects**

- Shows **all your Unity projects**
- Create new projects
- Open existing projects
- Shows:

  - Project name
  - Editor version
  - Last modified date

---

#### **3️. Install Modules**

For each Unity version, you can add:

- Android Build Support
- iOS Build Support
- Windows / Mac / Linux builds
- WebGL
- Visual Studio

> Without correct modules, **you cannot build the game**.

---

#### **4️. Account & License Management**

- Sign in with Unity account
- Activate personal/pro licenses
- Manage seats (important in studios)

Even free users must activate a license.

---

### **Real-world studio fact**

**Never upgrade Unity version blindly**

- Older projects may break
- Plugins may fail
- Physics & rendering changes happen

That’s why Unity Hub exists — **controlled version management**.

---

## **Topic 2: LTS Versions (Why LTS Matters)**

### **What does LTS mean?**

**LTS = Long Term Support**

An **LTS version** is a Unity release that:

- Is **stable**
- Gets **bug fixes**
- Gets **security updates**
- **No new risky features**

Support duration: **2+ years**

---

### **Unity release types**

Unity releases come in **three types**:

1️. **Tech Stream (TS)**

- New features
- Experimental
- Less stable
- Short support

2️. **LTS**

- Feature-frozen
- Stable
- Long support
- Industry standard

3️. **Legacy**

- Old LTS versions
- Used for maintaining old projects

---

### **Why LTS is used in real projects**

In **studios, companies, and production**, they use **LTS only**.

Why?

#### Stability

- Fewer crashes
- Predictable behavior

#### Plugin Compatibility

- Assets from Asset Store tested on LTS
- Third-party SDKs (Ads, Analytics) support LTS first

#### Team Safety

- Multiple developers → fewer version issues
- CI/CD pipelines rely on stability

---

### **Why NOT use latest version always**

New Unity versions:

- Can break shaders
- Can break physics
- Can break rendering
- Can break mobile builds

> A game works in **2022 LTS**, breaks in **Unity 6.x Tech Stream**

---

## **Topic 3: Project Templates**

When you create a new project in Unity Hub, you must choose a **template**.
This decides **how your game renders, what packages are included, and how scenes behave**.

---

## **1️. 2D Template**

### **What it is**

Designed for:

- 2D games (platformers, top-down)

### **What Unity sets up**

- Orthographic camera
- 2D physics system:

  - `Rigidbody2D`
  - `Collider2D`

- Sprite system
- Pixel-perfect options

### **What you work with**

- Sprites
- Tilemaps
- Sorting Layers
- 2D lights (if URP 2D used)

### **Use this when**

- Pure 2D game
- Not needed for 3D environments

---

## **2️. 3D Template (Built-in Render Pipeline)**

### **What it is**

Classic Unity 3D setup using **Built-in Render Pipeline**.

### **What Unity sets up**

- Perspective camera
- 3D physics:

  - `Rigidbody`
  - `Collider`

- Standard shader
- Basic lighting

### **Pros**

- Simple
- Fast to start
- Tons of old tutorials support it

### **Cons**

- Less flexible
- Older rendering tech
- Not future-focused

### **Use this when**

- Learning core 3D concepts
- High-quality visuals or mobile optimization

---

## **3️. URP Template (Universal Render Pipeline)**

### **What it is**

Modern, optimized rendering pipeline.

### **What Unity sets up**

- Scriptable Render Pipeline (SRP)
- URP shaders
- Better lighting control
- Post-processing built-in

### **Why URP matters**

- Better performance (mobile-friendly)
- Scalable graphics
- Used in most new projects
- Works for **both 2D & 3D**

### **URP Variants**

- **2D URP** → 2D lighting
- **3D URP** → modern 3D visuals

---

## **Built-in vs URP**

| Feature        | Built-in  | URP                |
| -------------- | --------- | ------------------ |
| Performance    | Average   | Better             |
| Mobile         | Weak      | Strong             |
| Future support | Declining | Actively developed |
| Shader Graph   | no        | yes                |

---

## **Topic 4: Editor Layout**

## **Main Unity Editor Windows**

### **1️. Scene View**

- Where you **build** the game
- Move, rotate, scale objects
- Edit level, place lights, cameras

---

### **2️. Game View**

- Shows what the **player sees**
- Uses the active camera
- Affected by resolution & aspect ratio

---

### **3️. Hierarchy**

- List of all GameObjects in the scene
- Parent-child relationships

Example:

```
Player
 ├─ Camera
 └─ Weapon
```

---

### **4️. Inspector**

- Shows properties of selected object
- Where you:

  - Change Transform values
  - Add Components
  - Edit scripts

---

### **5️. Project Window**

- Shows assets:

  - Scripts
  - Materials
  - Prefabs
  - Scenes

---

### **6️. Console**

- Displays:

  - Errors
  - Warnings
  - Logs

---

## **What is a Layout exactly?**

A **layout** is:

- Arrangement of all windows
- Saved as a preset

Unity gives defaults:

- Default
- 2 by 3
- Tall
- Wide

---

## **Topic 5: Scene View vs Game View**

## **Scene View**

### **What it is**

- The **editor’s workspace**
- Used to **design and build** your game

### **What you do here**

- Place GameObjects
- Move / Rotate / Scale objects
- Adjust lights, cameras
- Edit level geometry

### **Key points**

- Shows **editor gizmos**
- Not affected by resolution
- Free camera movement (WASD + mouse)

> Scene View is for **developers**, not players.

---

## **Game View**

### **What it is**

- What the **player actually sees**
- Renders from the **active Camera**

### **What affects Game View**

- Camera position & rotation
- Camera settings (FOV, clipping)
- Screen resolution & aspect ratio
- Post-processing

> Game View is the **final output**.

---

## **Critical differences**

| Scene View   | Game View         |
| ------------ | ----------------- |
| Editor-only  | Player-facing     |
| Free camera  | Camera-controlled |
| Shows gizmos | No gizmos         |
| Design mode  | Runtime output    |

---

_Why does my object appear in Scene View but not Game View?_

Correct reasons:

- Camera not pointing at object
- Object outside camera frustum
- Object on ignored layer
- Disabled renderer

---

## **Topic 6: Play Mode vs Edit Mode**

## **Edit Mode**

### **What it is**

- Default state of Unity
- Used to **set up** the game

### **What you do**

- Place GameObjects
- Change values in Inspector
- Add components
- Write scripts

### **Important rule**

> Changes made in Edit Mode are **permanent**

---

## **Play Mode**

### **What it is**

- Simulation of the game
- Runs your scripts

### **What happens**

- `Start()` runs once
- `Update()` runs every frame
- Physics starts
- Input works

Unity enters Play Mode when you press `play button`.

---

## **Advanced note (important later)**

Some scripts can run in Edit Mode:

```csharp
[ExecuteAlways]
```

But beginners should **avoid this** initially.

---

## **Interview question**

_Why do changes revert after exiting Play Mode?_

Correct answer:

> Because Play Mode runs a temporary simulation and Unity reloads the scene to its original Edit Mode state after stopping.

---

## **Topic 7: Console Usage (Errors, Warnings, Logs)**

## **What is the Console?**

The Console shows messages from Unity and your scripts:

- Errors
- Warnings
- Logs

Open it:

- `Window → General → Console`
- Or shortcut: **Ctrl + Shift + C**

---

## **1️. Errors**

### **What they mean**

- Code is broken
- Game will NOT run correctly
- Must be fixed

### **Examples**

- Syntax error
- NullReferenceException
- Missing script

### **Rule**

**Never press Play with red errors**

---

## **2️. Warnings**

### **What they mean**

- Game runs
- But something is risky or inefficient

### **Examples**

- Unused variables
- Deprecated APIs
- Missing references

### **Rule**

Fix warnings early, or they become bugs later.

---

## **3️. Logs**

### **What they are**

- Debug messages you print

Example:

```csharp
Debug.Log("Player Jumped");
```

Used for:

- Debugging logic
- Checking values
- Understanding flow

---

## **Reading error messages correctly (CRITICAL SKILL)**

Unity errors show:

1. Error type
2. Description
3. Script name
4. Line number

Example:

```
NullReferenceException
PlayerMovement.cs:25
```

> Double-click → jumps to the line.

---

## **Clear Console options**

- **Clear** → removes messages
- **Collapse** → groups identical logs
- **Error Pause** → auto-pauses on error (useful)

---

## **Topic 1: Debugger Setup (VS Code + Unity)**

Debugging = **pausing the game and inspecting reality**.
If you don’t debug, you’re blind.

---

## **What debugging actually does**

Debugger lets you:

- Pause execution
- Inspect variables
- Step through code line-by-line
- Understand WHY something happens

`Debug.Log` is weak. Debugger is powerful.

---

## **Prerequisites (don’t skip)**

Make sure:

- Unity installed with **Visual Studio Code Editor** module
- VS Code has **C# Dev Kit**
- Unity is set to VS Code as external editor

If any of these fail → debugger won’t attach.

---

## **How Unity debugging works (conceptual)**

Flow:

```
Unity Editor -> runs game
VS Code -> attaches debugger
Debugger -> controls execution
```

You do **NOT** run Unity from VS Code.

---

## **Step-by-step: Attach debugger**

### **1️. Open project via Unity**

- Open Unity Hub
- Open your project
- Open any C# script (opens VS Code)

---

### **2️. Open Run & Debug in VS Code**

- Left sidebar → ▶🐞 icon
- Select **“Attach to Unity”**

If you don’t see it:

- Press `Ctrl + Shift + P`
- Type: `Debug: Select and Start Debugging`
- Choose **Attach to Unity**

---

### **3️. Press Play in Unity**

- VS Code debugger attaches automatically
- Status bar turns orange

Now debugging is active.

---

## **If Attach to Unity is missing**

Fix checklist:

- C# Dev Kit installed
- `.csproj` files generated
- Restart both Unity & VS Code
- Reopen project from Unity

---

## **Unity Editor Debugging options**

In Unity:

```
Edit → Preferences → General
Script Debugging → ENABLE
```

This allows breakpoints to work.

---

## **Topic 2: Breakpoints**

Breakpoints are how you **stop time** and inspect your game.

---

## **What is a breakpoint?**

A **breakpoint** is a marker that tells the debugger:

> “Pause execution here when this line runs.”

Game freezes. You examine everything.

---

## **How to set a breakpoint in VS Code**

- Click on the **left gutter** (next to line numbers)
- A **red dot** appears

That’s it.

Example:

```csharp
void Update()
{
    MovePlayer();   // ← breakpoint here
}
```

---

## **When does breakpoint hit?**

A breakpoint triggers **only if**:

- Script is attached to an active GameObject
- Method is actually executed
- Debugger is attached
- You are in Play Mode

If any of these fail → breakpoint won’t hit.

---

## **Best places to put breakpoints**

- Inside `Start()`
- Inside `Update()`
- Inside `OnTriggerEnter`
- Before `if` conditions
- Where values change unexpectedly

---

## **What happens when breakpoint hits**

- Unity **pauses**
- VS Code highlights the line
- You can inspect variables
- Game window freezes

This is GOOD.

---

## **Debugger controls**

When paused, use:

- **Continue (F5)** → resume
- **Step Over (F10)** → next line
- **Step Into (F11)** → go inside method
- **Step Out (Shift+F11)** → exit method

---

## **Watching variables**

Hover mouse over variables → see values
Or add to **Watch panel**.

Example:

```csharp
float speed;
Vector3 direction;
```

---

## **Topic 3: Call Stack**

## **What is the Call Stack?**

The **call stack** shows:

> **Which methods were called, and in what order, to reach the current line.**

It’s the **execution path**.

---

## **Simple mental model**

Think of it like a **stack of plates**:

- Method A calls Method B
- Method B calls Method C
- Unity pauses inside Method C

Stack (top → bottom):

```
MethodC()
MethodB()
MethodA()
```

Top = current execution.

---

## **Unity example**

```csharp
void Update()
{
    Move();
}

void Move()
{
    ApplyForce();
}

void ApplyForce()
{
    Debug.Log("Moving");
}
```

Breakpoint inside `ApplyForce()`.

Call Stack shows:

```
ApplyForce()
Move()
Update()
```

This tells you **how you got here**.

---

## **Why Call Stack is CRITICAL**

It answers:

- Who called this method?
- From where did this execute?
- Why is this running?

Especially important for:

- NullReferenceException
- Unexpected behavior
- Complex logic

---

## **Where to see Call Stack in VS Code**

When breakpoint hits:

- Open **Call Stack** panel (left side)
- Click any frame → jump to that method

You can move **up and down execution history**.

---

## **Unity-specific insight**

Unity calls:

- `Update()`
- `FixedUpdate()`
- `OnCollisionEnter()`

You **do not call them manually**, but they still appear in the stack.

---

## **Real debugging workflow**

1. Game pauses
2. Check **current line**
3. Check **variable values**
4. Check **Call Stack**
5. Identify wrong caller

This is how pros debug.

---

## **Interview question**

_How do you debug a NullReferenceException in Unity?_

Correct answer:

> I use breakpoints, inspect variables, and check the call stack to find where the null reference originated.

---
