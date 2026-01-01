## 1. `if / else`

`if` is a **conditional jump**.
The condition is evaluated → execution jumps to one branch.

```csharp
if (health <= 0)
{
    Die();
}
else
{
    TakeDamage();
}
```

Rules the compiler enforces:

- Condition **must be `bool`**
- No implicit numeric truthiness (unlike C/C++)

```csharp
if (health) { }   // not allowed
```

---

### `else if` chain (priority matters)

```csharp
if (score >= 100)
{
    Rank = Rank.S;
}
else if (score >= 80)
{
    Rank = Rank.A;
}
else
{
    Rank = Rank.B;
}
```

Execution:

- First true condition wins
- Remaining checks are skipped

**Optimization tip:**
Order from **most likely** or **cheapest** condition first.

---

### Early-return pattern (production style)

Bad (nested hell):

```csharp
if (player != null)
{
    if (player.IsAlive)
    {
        Attack();
    }
}
```

Good:

```csharp
if (player == null) return;
if (!player.IsAlive) return;

Attack();
```

This improves:

- Readability
- Debugging
- Branch prediction

---

## 2. `switch`

### Classic `switch`

```csharp
switch (state)
{
    case GameState.Playing:
        UpdateGame();
        break;

    case GameState.Paused:
        ShowMenu();
        break;

    default:
        Debug.LogError("Unknown state");
        break;
}
```

Rules:

- `break` is mandatory (unless `return`)
- No fall-through (unlike C)

---

### Modern `switch` expressions (C# 8+)

Classic `switch` is a **statement**.
A switch **expression** is an **expression that returns a value**.

#### Exact syntax (memorize this pattern)

General form:

```csharp
result = expression switch
{
    pattern1 => value1,
    pattern2 => value2,
    _        => defaultValue
};
```

Parts explained:

- `expression` → value being tested
- `pattern` → what to match against
- `=>` → mapping operator (not lambda)
- `_` → wildcard / default case
- `;` → required (because expression)

---

#### No missing cases (compiler-enforced)

If you omit `_`:

```csharp
int damage = weaponType switch
{
    WeaponType.Sword => 10
};
```

> Compile-time error

This is huge in production:

- Refactoring enums becomes safer
- Compiler protects you

Classic `switch` gives **no warning**.

---

#### No accidental fall-through (by design)

Classic `switch`:

```csharp
case A:
    DoSomething();
case B:
    DoOther(); // bug
```

Switch expression:

- Each arm is isolated
- No execution flow
- Just value mapping

Fall-through is **impossible**.

#### Using switch expressions with enums (Unity-standard)

```csharp
PlayerState state;

float speed = state switch
{
    PlayerState.Idle => 0f,
    PlayerState.Run  => 5f,
    PlayerState.Jump => 3f,
    _ => 0f
};
```

This is **ideal for**:

- Player states
- Weapon types
- Animation states
- AI states

#### Switch expressions with conditions (patterns)

You are NOT limited to equality.

```csharp
int health = 75;

string status = health switch
{
    <= 0   => "Dead",
    < 30   => "Critical",
    < 70   => "Injured",
    _      => "Healthy"
};
```

This replaces ugly `if / else` chains.

#### Switch expressions with tuples (advanced but powerful)

```csharp
var damage = (weaponType, isCritical) switch
{
    (WeaponType.Sword, true)  => 20,
    (WeaponType.Sword, false) => 10,
    (WeaponType.Bow,   true)  => 14,
    (WeaponType.Bow,   false) => 7,
    _ => 0
};
```

Cleaner than nested logic.

---

#### When NOT to use switch expressions

- You need multiple statements
- You need side effects (A side effect is anything a piece of code does besides returning a value.)
- You need logging inside cases
- Logic is complex or stateful

Bad use:

```csharp
state switch
{
    PlayerState.Dead => Die(), // side effect
    _ => DoNothing()
};
```

Switch expressions are for **value selection**, not behavior execution.

---

#### Unity production rule

Use:

- `switch expression` → compute values
- `switch statement` → execute behavior

Example:

```csharp
float speed = state switch
{
    PlayerState.Run => 5f,
    PlayerState.Walk => 2f,
    _ => 0f
};

Move(speed);
```

---

> A switch expression is an expression that returns a value. It is exhaustive, side-effect-free, prevents fall-through bugs, and is safer than traditional switch statements for mapping input to output.

---

### Unity tip

Use `switch` for:

- Game states
- Animation states
- Input mapping

Avoid long `if/else` chains for enums.

---

## 3. Loops — Controlled Repetition

### `for` loop (index-based)

```csharp
for (int i = 0; i < enemies.Count; i++)
{
    enemies[i].UpdateAI();
}
```

Best used when:

- You need index
- You might modify collection
- Performance matters

Unity note:

- `for` is fastest
- Preferred in hot paths

In Unity terms, a **hot path** is anything that runs:

- 60+ times per second
- For dozens or hundreds of objects

Example hot paths:

- Player movement logic
- Enemy AI loops
- Physics-related updates
- Animation state updates
- Bullet updates

---

### `while` loop (condition-driven)

```csharp
while (health > 0)
{
    Regenerate();
}
```

Danger:

- Easy to create infinite loops

Always ask:

> “What guarantees this condition becomes false?”

---

### `foreach` loop (safe iteration)

```csharp
foreach (var enemy in enemies)
{
    enemy.UpdateAI();
}
```

Pros:

- Clean
- No index errors

Cons:

- Cannot modify collection
- Slight overhead (struct enumerators are fine)

Unity rule:

- Use `foreach` for **read-only iteration**
- Use `for` when removing items

---

## 4. Loop Control Keywords

### `break` — Exit the loop immediately

```csharp
foreach (var enemy in enemies)
{
    if (enemy.IsBoss)
        break;
}
```

Execution jumps **outside** the loop.

---

### `continue` — Skip current iteration

```csharp
foreach (var enemy in enemies)
{
    if (!enemy.IsAlive)
        continue;

    enemy.Attack();
}
```

Think of it as:

> “Ignore this one, move on.”

---

### `return` — Exit the method

```csharp
void Update()
{
    if (isPaused)
        return;

    Move();
}
```

Important:

- `return` exits **entire method**
- Not just loop

This is heavily used in Unity `Update()` for early exits.

---

## 5. Unity-Specific Control Flow Patterns

### Pattern: Guard Clauses (mandatory habit)

```csharp
void Update()
{
    if (!isActive) return;
    if (health <= 0) return;

    ProcessInput();
}
```

Cleaner than nested logic.

---

### Pattern: State-Driven Update

```csharp
void Update()
{
    switch (state)
    {
        case PlayerState.Idle:
            IdleUpdate();
            break;

        case PlayerState.Run:
            RunUpdate();
            break;
    }
}
```

This is **industry standard**.

---
