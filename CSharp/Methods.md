## 1. What a Method Really Is

A **method** is:

- A named block of executable code
- With defined **inputs (parameters)**
- Optional **output (return value)**
- A clear **responsibility**

Think in terms of:

> “What does this method _do_ and _what does it return_?”

---

## 2. Parameters (Inputs to a Method)

### Basic syntax

```csharp
int Add(int a, int b)
{
    return a + b;
}
```

- `a`, `b` → parameters
- Strongly typed
- Passed **by value** by default

---

### Parameter passing (critical)

#### By value (default)

```csharp
void Increase(int x)
{
    x++;
}

int a = 5;
Increase(a);
// a is still 5
```

A copy is passed.

---

#### By reference (`ref`)

```csharp
void Increase(ref int x)
{
    x++;
}

int a = 5;
Increase(ref a);
// a is now 6
```

Use sparingly. Makes code harder to reason about.

---

#### `out` parameters (for returning multiple values)

```csharp
bool TryDivide(int a, int b, out int result)
{
    if (b == 0)
    {
        result = 0;
        return false;
    }

    result = a / b;
    return true;
}
```

Used heavily in .NET (`TryParse`).

---

### Optional parameters

```csharp
void Move(float speed = 5f)
{
}
```

Good for APIs, dangerous if abused.

---

## 3. Return Values (Outputs)

### Void vs returning data

```csharp
void Jump()
{
    // side effect only
}

float GetSpeed()
{
    return speed;
}
```

Rule:

- Use `void` for **actions**
- Return values for **queries**

This is a key design principle.

---

### Early returns (clean design)

```csharp
int CalculateDamage(int baseDamage)
{
    if (baseDamage <= 0)
        return 0;

    return baseDamage * 2;
}
```

Avoid deep nesting.

---

### Returning multiple values (modern C#)

```csharp
(int min, int max) GetRange()
{
    return (0, 100);
}
```

Preferred over `out` in modern code.

---

## 4. Method Overloading (Same name, different signatures)

### What overloading really means

Multiple methods with:

- Same name
- Different parameter lists

```csharp
void Damage(int amount) { }
void Damage(int amount, bool isCritical) { }
```

Compiler decides **at compile time** which one to call.

---

### Overloading rules (important)

You can overload by:

- Parameter count
- Parameter types
- Parameter order

You CANNOT overload by:

- Return type alone

Invalid:

```csharp
int GetValue() { }
float GetValue() { }
```

---

### Real Unity-style overloading

```csharp
void Move(Vector3 direction)
{
    Move(direction, speed);
}

void Move(Vector3 direction, float speed)
{
    transform.position += direction * speed * Time.deltaTime;
}
```

This avoids duplication and improves API usability.

---
