## LEVEL 1 — C# FOUNDATION

### MUST KNOW C#

- Variables & types
- Value vs Reference types
- `class` vs `struct`
- `null`
- `const` vs `readonly`
- `ref` / `out`
- Enums

### OOP (PRACTICAL)

- Classes & Objects
- Encapsulation
- Inheritance (when NOT to use)
- Interfaces (VERY important)
- Composition > Inheritance

> **Outcome:**
> You understand **what your scripts really are**.

---

## LEVEL 2 — UNITY CORE LIFECYCLE

### MonoBehaviour Lifecycle

- `Awake`
- `OnEnable`
- `Start`
- `Update`
- `FixedUpdate`
- `LateUpdate`
- `OnDisable`
- `OnDestroy`

### Execution Order

- Script Execution Order
- Why bugs happen due to timing

### Components

- `GameObject` vs `Component`
- `Transform`
- `GetComponent`
- `TryGetComponent`

> **Outcome:**
> You stop guessing.
> You know **WHEN code runs**.

---

## LEVEL 3 — INPUT, MOVEMENT & PHYSICS

### Input

- Old Input System
- New Input System (basics)
- Events vs polling

### Physics

- Rigidbody
- Colliders
- `OnCollisionEnter`
- `OnTriggerEnter`
- FixedUpdate vs Update

### Movement

- Frame-rate independence
- `Time.deltaTime`

> **Outcome:**
> You build predictable gameplay.

---

## LEVEL 4 — DATA & STATE

### Data Handling

- `SerializeField`
- Public vs private
- Inspector usage
- ScriptableObjects (VERY IMPORTANT)
- Enums for state

### Game State

- State machines (basic)
- Player state
- Game state

> **Outcome:**
> You stop hardcoding everything.

---

## LEVEL 5 — EVENTS & COMMUNICATION

### Communication

- Direct references
- Events
- Delegates
- UnityEvents

### Decoupling

- Why tight coupling kills projects
- Observer pattern (Unity version)

> **Outcome:**
> Your code becomes maintainable.

---

## LEVEL 6 — SCENE & FLOW MANAGEMENT

### Scene System

- Scene loading (sync/async)
- Additive scenes
- Scene transitions

### Managers

- GameManager
- UIManager
- AudioManager
- Singleton pattern (proper usage)

> **Outcome:**
> You understand **game flow**.

---

## LEVEL 7 — UI SYSTEM

### Unity UI

- Canvas
- RectTransform
- Anchors
- Buttons, sliders
- UI events

### UI Architecture

- UI ↔ Gameplay communication
- Avoid Update() in UI

> **Outcome:**
> Clean UI logic.

---

## LEVEL 8 — COROUTINES & ASYNC

### Coroutines

- `IEnumerator`
- `yield return`
- Common mistakes
- Coroutine lifetime

### Async Basics

- Async/Await (Unity-safe usage)
- When NOT to use async

> **Outcome:**
> Time-based logic becomes easy.

---

## LEVEL 9 — DEBUGGING & READING PRODUCTION CODE (CRITICAL)

### Debugging

- Logs strategy
- Breakpoints
- Call stack reading
- Bug reproduction

### Code Reading

- Entry points
- Data flow tracing
- Ignore noise
- Minimal understanding principle

> **Outcome:**
> You stop freezing.

---

## LEVEL 10 — PERFORMANCE

### Performance Basics

- GC allocations
- Update optimization
- Object pooling
- Avoiding LINQ in hot paths

### Profiling

- Unity Profiler
- Memory view
- CPU spikes

> **Outcome:**
> You write **safe production code**.

---

## LEVEL 11 — ARCHITECTURE

### Patterns

- MVC / MVP (Unity style)
- Service Locator
- Dependency Injection (basic)

### Project Structure

- Folder architecture
- Feature-based structure

> **Outcome:**
> You think like a senior.

---

## LEVEL 12 — POLISH & REALITY

### Reality Skills

- Refactoring safely
- Handling tech debt
- Communicating risks
- Estimation

### Professional Habits

- Small commits
- Meaningful logs
- Defensive coding

> **Outcome:**
> You become **reliable**.

---

## LEVEL M0 — MECHANICS THINKING

### Concepts

- What is a game mechanic?
- Input → Logic → Feedback loop
- Deterministic vs non-deterministic behavior
- Frame-based vs event-based logic
- State-driven mechanics

### Practice

- Button press → action → visual/audio feedback
- Toggle states (on/off)

**Outcome:**
You stop coding randomly. You design behavior.

---

## LEVEL M1 — MOVEMENT MECHANICS

### Mechanics

- Grid movement
- Free movement
- Acceleration & deceleration
- Jump mechanics
- Gravity tuning
- Dash

### Unity Concepts Used

- `Update`, `FixedUpdate`
- `Rigidbody`
- `Time.deltaTime`
- Physics materials

### Practice Mechanics

- Character controller
- Jump with coyote time
- Dash with cooldown

**Outcome:**
You can build **feel-good movement**.

---

## LEVEL M2 — INPUT MECHANICS

### Mechanics

- Single input → multiple actions
- Hold vs tap
- Buffered input
- Input priority
- Rebinding

### Unity Concepts Used

- New Input System
- Action maps
- Events

### Practice

- Jump buffering
- Attack buffering

**Outcome:**
Controls feel responsive.

---

## LEVEL M3 — COMBAT MECHANICS

### Mechanics

- Melee attack
- Ranged attack
- Hit detection
- Damage calculation
- Invincibility frames
- Knockback

### Unity Concepts Used

- Colliders
- Triggers
- Animation events
- ScriptableObjects

### Practice

- Sword slash
- Bullet hit
- Health system

**Outcome:**
You can build **combat systems**, not just attacks.

---

## LEVEL M4 — STATE MACHINES

### Mechanics

- Player states (Idle, Move, Attack, Dead)
- Enemy AI states
- Interruptions
- Transitions
- Priority rules

### Unity Concepts Used

- Enums
- Interfaces
- State pattern

### Practice

- Player state machine
- Enemy patrol/attack

**Outcome:**
Your mechanics stop conflicting.

---

## LEVEL M5 — PHYSICS-BASED MECHANICS

### Mechanics

- Push/pull
- Sliding
- Slopes
- Knockback physics
- Ragdoll basics

### Unity Concepts Used

- Rigidbody forces
- Constraints
- Layers & collision matrix

**Outcome:**
Natural, believable interactions.

---

## LEVEL M6 — TIME-BASED MECHANICS

### Mechanics

- Cooldowns
- Charge attacks
- Timers
- Slow motion
- Freeze frames

### Unity Concepts Used

- Coroutines
- Time scale
- DeltaTime

**Outcome:**
Polished, dramatic gameplay.

---

## LEVEL M7 — RESOURCE & PROGRESSION MECHANICS

### Mechanics

- Health
- Mana
- Stamina
- Ammo
- XP systems
- Skill upgrades

### Unity Concepts Used

- ScriptableObjects
- Events
- Save systems

**Outcome:**
Long-term engagement systems.

---

## LEVEL M8 — AI MECHANICS

### Mechanics

- Patrol
- Chase
- Attack
- Flee
- Alert systems

### Unity Concepts Used

- State machines
- NavMesh
- Raycasting

**Outcome:**
Enemies feel alive.

---

## LEVEL M9 — FEEDBACK MECHANICS

### Mechanics

- Screen shake
- Hit stop
- Particle effects
- Sound feedback
- Animation blending

### Unity Concepts Used

- Cinemachine
- Animator
- Particle system

**Outcome:**
Your game feels professional.

---

## LEVEL M10 — SYSTEMIC MECHANICS

### Mechanics

- Combo systems
- Buff/Debuff systems
- Status effects
- Chain reactions

### Unity Concepts Used

- Interfaces
- Event systems
- Data-driven design

**Outcome:**
Emergent gameplay.

---

## LEVEL M11 — PRODUCTION MECHANICS

### Mechanics

- Fail-safe logic
- Edge case handling
- Debug toggles
- Cheat systems
- Balancing tools

### Unity Concepts Used

- Editor scripting (basic)
- ScriptableObjects
- Conditional compilation

**Outcome:**
You survive production.

---

## LEVEL T0 — SPACE & TRANSFORM BASICS

### Concepts

- World Space vs Local Space
- Position, Rotation, Scale
- `Transform` component
- Parent–child relationship
- Pivot vs Center

### Must Understand

```csharp
transform.position
transform.localPosition
transform.rotation
transform.localRotation
transform.localScale
```

**Outcome:**
You stop being confused when objects move “wrong”.

---

## LEVEL T1 — VECTORS

If vectors are weak, everything breaks.

### Concepts

- `Vector2` / `Vector3`
- Direction vs position
- Vector magnitude
- Normalized vectors
- Distance between points

### Core APIs

```csharp
Vector3.Distance(a, b)
Vector3.Normalize()
Vector3.forward
```

### Mental Model

> Vector = direction + strength

**Outcome:**
Movement logic becomes simple.

---

## LEVEL T2 — TRANSLATION & MOVEMENT

### Methods

- `Transform.Translate()`
- `transform.position +=`
- Rigidbody movement vs Transform movement

### Frame Independence

```csharp
position += direction * speed * Time.deltaTime;
```

### Common Mistakes

❌ Moving in Update without deltaTime
❌ Mixing Rigidbody + Transform

**Outcome:**
Smooth, predictable movement.

---

## LEVEL T3 — INTERPOLATION

### Concepts

- What interpolation means
- Why Lerp is NOT time
- Alpha (t) value

### APIs

```csharp
Vector3.Lerp(a, b, t)
Quaternion.Lerp(a, b, t)
Mathf.Lerp(a, b, t)
```

### CRITICAL RULE

> `t` must be **controlled**, not hardcoded

### Common Bug

```csharp
transform.position = Vector3.Lerp(pos, target, 0.1f); // ❌
```

**Outcome:**
No more “stuck halfway” bugs.

---

## LEVEL T4 — ROTATION FUNDAMENTALS

### Euler Angles

- X, Y, Z rotations
- Degrees vs radians
- Gimbal lock (WHY it happens)

### APIs

```csharp
transform.eulerAngles
transform.Rotate()
```

**Outcome:**
You understand why rotation breaks.

---

## LEVEL T5 — QUATERNIONS

### Truth

> Unity uses **quaternions internally**.

### Why Quaternions

- No gimbal lock
- Smooth interpolation

### APIs

```csharp
transform.rotation
Quaternion.identity
Quaternion.LookRotation()
Quaternion.Slerp()
```

### Rule

- Don’t manually edit quaternion values
- Use Unity helpers

**Outcome:**
Rotation stops being scary.

---

## LEVEL T6 — LOOK & AIM MECHANICS

### Mechanics

- Face target
- Smooth rotation
- Turrets
- Camera follow

### APIs

```csharp
Quaternion.LookRotation()
Vector3.RotateTowards()
```

**Outcome:**
Professional camera & enemy behavior.

---

## LEVEL T7 — SMOOTHING & DAMPING

### Concepts

- Smooth vs instant
- Damping
- Ease-in / ease-out

### APIs

```csharp
Vector3.SmoothDamp()
Mathf.SmoothDamp()
```

### Use Case

- Camera movement
- UI animation

**Outcome:**
Game feel improves massively.

---

## LEVEL T8 — DOT & CROSS PRODUCT

### Dot Product

- Angle between vectors
- Field of view
- AI vision cones

### Cross Product

- Perpendicular direction
- Orientation

**Outcome:**
Smart AI & advanced mechanics.

---

## LEVEL T9 — RAYCASTING & SPACE QUERIES

### Concepts

- Ray direction
- Ray origin
- Hit info

### APIs

```csharp
Physics.Raycast()
```

**Outcome:**
Interaction systems.

---

## LEVEL T10 — PRODUCTION PITFALLS

### Must Know

- Floating point precision
- Comparing vectors
- Using `==` wrongly
- Update vs FixedUpdate

**Outcome:**
Fewer bugs in production.

---

# GAME MATH — ONLY THE IMPORTANT STUFF

## 1️. VECTORS

> **This is 50% of game math.**

### You MUST understand

- Vector = **direction + magnitude**
- Difference between **position** and **direction**
- Normalization (unit direction)
- Distance between two points

### Used for

- Movement
- AI direction
- Knockback
- Camera follow
- Projectile paths

### Unity APIs

```csharp
Vector3 direction = (target - current).normalized;
float distance = Vector3.Distance(a, b);
```

If vectors are weak, everything feels hard.

---

## 2️. DELTA TIME

> **Why movement doesn’t break on fast PCs**

### Concept

- Frames ≠ time
- Time-based movement

### Used for

- Movement
- Rotation
- Timers
- Cooldowns

```csharp
position += direction * speed * Time.deltaTime;
```

---

## 3️. INTERPOLATION

> **Smooth motion & transitions**

### You need

- What Lerp means
- `t` range (0 → 1)
- Why Lerp is NOT time

### Used for

- Smooth movement
- Camera follow
- UI animation
- Health bars

```csharp
value = Mathf.Lerp(start, end, t);
```

---

## 4️. ROTATION BASICS

> You don’t need quaternion math — just **usage**.

### You MUST know

- Euler angles = human readable
- Quaternions = engine safe
- Never manually modify quaternions

### Used for

- Facing target
- Camera rotation
- Turrets

```csharp
Quaternion.LookRotation(direction);
Quaternion.Slerp(a, b, t);
```

---

## 5️. DOT PRODUCT

> **Very powerful, very practical**

### You need

- What dot product tells
- Value range (-1 → 1)

### Used for

- AI vision cones
- “Is enemy in front?”
- Angle checks

```csharp
float dot = Vector3.Dot(forward, directionToTarget);
```

---

## 6️. CROSS PRODUCT

### Used for

- Perpendicular direction
- Orientation
- Some physics effects

You don’t need deep theory.

---

## 7️. DISTANCE & RANGE CHECKS

> Simple but everywhere

### Used for

- Attack range
- Trigger logic
- AI behavior

```csharp
if (distance < attackRange)
```

---

## 8️. RAYCASTING

> **Not math-heavy, but math-based**

### Used for

- Shooting
- Line of sight
- Ground check
- Interaction

```csharp
Physics.Raycast(origin, direction, out hit, range);
```

---

## 9️. CLAMPING & REMAPPING

> Prevents bugs

### Used for

- Health
- Mana
- UI sliders

```csharp
Mathf.Clamp(value, min, max);
```

---

## 10. SMOOTHING & DAMPING

> Game feel polish

### Used for

- Camera smoothing
- UI transitions

```csharp
Vector3.SmoothDamp();
```

---

# DSA ROADMAP FOR GAME DEVELOPERS (PRACTICAL)

## 1️. ARRAYS & LISTS (MOST USED)

> **Used everywhere, every day**

### Learn

- Arrays
- `List<T>`
- Indexing
- Iteration
- Insert/remove cost

### Used for

- Enemies list
- Bullets
- Inventory
- Update loops
- Object pools

### Unity examples

```csharp
List<GameObject> enemies;
```

---

## 2️. DICTIONARIES / HASH MAPS (CRITICAL)

> **Second most important**

### Learn

- Key → value lookup
- Existence checks
- Iteration

### Used for

- ID → object mapping
- State lookups
- Caches
- Settings
- Cooldown timers

```csharp
Dictionary<int, EnemyData>
```

---

## 3️. STACKS (GAME STATES)

> Simple but powerful

### Learn

- Push / Pop

### Used for

- Undo
- State transitions
- UI navigation
- Pause menus

---

## 4️. QUEUES (EVENT FLOW)

> Event-driven logic

### Learn

- Enqueue / Dequeue

### Used for

- Turn systems
- AI actions
- Animation queues
- Task scheduling

---

## 5️. SETS (FAST LOOKUPS)

> Prevent duplicates

### Learn

- Add / Remove / Contains

### Used for

- Active enemies
- Triggered objects
- Buffs/debuffs
- Collision tracking

---

## 6️. BASIC SEARCHING (LINEAR + SIMPLE OPTIMIZATION)

### Learn

- Linear search
- Early exit
- Filtering

### Used for

- Finding nearest enemy
- Target selection
- Range checks

> You don’t need binary search often in gameplay.

---

## 7️. SORTING (LIGHT USE)

> Only basics

### Learn

- Built-in sorting
- Custom comparers

### Used for

- Leaderboards
- Priority systems
- AI target priority

---

## 8️. GRAPHS (VERY BASIC)

> Optional but useful

### Learn

- Nodes & edges
- Adjacency list concept

### Used for

- NavMesh understanding
- Pathfinding concepts
- Level connections

> You don’t need to implement A\* from scratch unless asked.

---

## 9️. RECURSION (LIMITED)

> Use carefully

### Learn

- Base case
- Stack overflow risks

### Used for

- Hierarchy traversal
- Tree-like data
- Menu systems

---

## 10. OBJECT POOLING (GAME-SPECIFIC DSA)

> This is **huge** in games

### Learn

- Reuse objects
- Avoid allocations

### Used for

- Bullets
- Particles
- Enemies

---

# C# ROADMAP FOR GAME DEVELOPMENT (UNITY-FOCUSED)

## Topics To Cover Later

- complie time vs run time
- Dynamic
- ref in out
- TryParse
- lambda
- tuple
- memory
- cache
- Null-coalescing
- GC
- readonly
- constryuctor chaining
- upcasting downcasting
- Singleton
- create object in unity?
- build in function list for collections
- Hashcode, collision
- LinQ

---

Learn after core.

- `SortedDictionary<TKey, TValue>`
- `SortedList<TKey, TValue>`
- `ReadOnlyCollection<T>`
- `ConcurrentDictionary<TKey, TValue>`
- `ConcurrentQueue<T>`
- `BlockingCollection<T>`
- `ImmutableList<T>`
- `ImmutableDictionary<TKey, TValue>`

---

Important if you go deeper into Unity systems.

- `NativeArray<T>`
- `Span<T>`
- `ReadOnlySpan<T>`
- `ArraySegment<T>`
- Object pooling patterns (not a collection, but critical)

---

Know they exist. Don’t spend time mastering.

- `LinkedList<T>`
- `ObservableCollection<T>`
- `BitArray`
- `SortedSet<T>`

---

## LEVEL C5 — DELEGATES, EVENTS & ACTIONS (CORE UNITY PATTERN)

### Concepts

- Delegates
- `Action`
- `Func`
- Events

---

## LEVEL C6 — ENUMS & STATE MANAGEMENT

### Enums

- Defining states
- Flags enum (basic)

### State Handling

- Switch-based states
- State pattern (basic)

---

## LEVEL C7 — GENERICS (POWERFUL BUT SIMPLE)

### Must Know

- Generic classes
- Generic methods
- Constraints (`where T : class`)

### Use Cases

- Object pools
- Managers
- Reusable systems

---

## LEVEL C8 — COROUTINES & TIME (UNITY-SPECIFIC)

### Coroutines

- `IEnumerator`
- `yield return`
- `WaitForSeconds`
- Coroutine lifetime

### Timing

- Cooldowns
- Delays
- Sequences

---

## LEVEL C9 — ASYNC / AWAIT (LIMITED BUT USEFUL)

### Use Cases

- File IO
- Loading data
- Network calls (if any)

### Rules

- Don’t use async in Update
- Use for background tasks

---

## LEVEL C10 — MEMORY, GC & PERFORMANCE (PRODUCTION LEVEL)

### Must Know

- Garbage collection basics
- Allocations
- Boxing / unboxing
- String allocations

### Practices

- Object pooling
- Avoid LINQ in hot paths

---

## LEVEL C11 — SCRIPTABLEOBJECTS & DATA-DRIVEN DESIGN

### Must Know

- ScriptableObject lifecycle
- Data vs behavior separation
- Sharing data safely

### Use Cases

- Weapon data
- Enemy stats
- Config files

---

## LEVEL C12 — ERROR HANDLING & SAFETY

### Must Know

- Exceptions
- `try / catch`
- Defensive programming

### Unity Reality

- When to fail
- When to log

---

## LEVEL C13 — TOOLS & QUALITY

### Must Know

- Namespaces
- Partial classes
- Attributes
- Custom inspectors (basic)

---

## LEVEL C14 — PRODUCTION HABITS (SENIOR THINKING)

### Practices

- Writing readable code
- Guard clauses
- Fail-safe logic
- Refactoring safely

---
