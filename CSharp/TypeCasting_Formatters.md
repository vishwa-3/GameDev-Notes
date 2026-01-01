## TYPE CASTING

1. **Implicit casting** (compiler guarantees safety)
2. **Explicit casting** (you accept risk)

This is enforced at **compile time**, not runtime (except for reference types).

---

### Implicit Casting (Safe, Automatic)

Occurs when **no data loss is possible**.

#### Example

```csharp
int a = 10;
float b = a;
```

Why allowed:

- `float` can represent all `int` values
- No precision loss (at this step)

#### Numeric Promotion Table (Common)

| From    | To       | Allowed | Why              |
| ------- | -------- | ------- | ---------------- |
| `int`   | `long`   | Yes     | Larger range     |
| `int`   | `float`  | Yes     | Wider type       |
| `float` | `double` | Yes     | Higher precision |
| `char`  | `int`    | Yes     | Unicode value    |

Compiler does this **silently and safely**.

---

### Explicit Casting (Potential Data Loss)

Required when **data might be lost**.

#### Example

```csharp
float x = 9.9f;
int y = (int)x;
```

Result:

```
y = 9
```

Important:

- **Truncation**, not rounding
- Compiler forces explicit cast to protect you

#### More examples

```csharp
int a = (int)2.9f;     // 2
byte b = (byte)300;    // 44 (wraparound!)
```

---

### Checked vs Unchecked Casting

```csharp
checked
{
    byte b = (byte)300; // OverflowException
}
```

Without `checked`:

```csharp
byte b = (byte)300; // wraps to 44
```

Use `checked` in:

- Financial logic
- Save systems
- Score systems

---

### Casting vs Parsing

#### Casting (memory-level)

```csharp
int a = (int)2.5f;
```

✔ Same type system
✔ No validation
✔ No strings involved

---

#### Parsing (text → value)

```csharp
int a = int.Parse("123");     // throws if invalid
int b = int.TryParse("123", out var result);
```

Parsing:

- Converts **text**
- Can fail
- Involves validation

**Production rule:**
Never trust user input → use `TryParse`.

---

### Reference Type Casting (Runtime-checked)

```csharp
Animal a = new Dog();
Dog d = (Dog)a; // OK
```

Wrong cast:

```csharp
Cat c = (Cat)a; // Runtime exception
```

Safer pattern:

```csharp
if (a is Dog d)
{
    d.Bark();
}
```

---

### 7. Unity-Specific Casting

```csharp
GetComponent<Rigidbody>();
GetComponent<Collider>();
```

Unity returns specific types using **generic constraints**, not runtime casting. That’s why it’s safe.

Avoid this:

```csharp
(Component)GetComponent(typeof(Rigidbody));
```

Use generics instead.

---

## FORMATTERS (STRING & NUMBER FORMATTING)

Formatting controls:

- Precision
- Readability
- Localization
- UI correctness

---

### String Interpolation

```csharp
int score = 150;
float time = 12.3456f;

string text = $"Score: {score}, Time: {time:F2}";
```

Readable, safe, compile-time checked.

---

### Numeric Format Specifiers (Core Table)

| Specifier | Meaning            | Example    | Output  |
| --------- | ------------------ | ---------- | ------- |
| `F2`      | Fixed-point        | `{x:F2}`   | `3.14`  |
| `N0`      | Number with commas | `{x:N0}`   | `1,000` |
| `P`       | Percentage         | `{x:P}`    | `50 %`  |
| `0.00`    | Custom format      | `{x:0.00}` | `3.10`  |
| `00`      | Padding            | `{x:00}`   | `05`    |

---

### Unity UI Formatting Examples

```csharp
healthText.text = $"HP: {health}";
ammoText.text   = $"Ammo: {ammo:00}";
timeText.text   = $"Time: {time:F1}s";
```

These are **production patterns**.

---

### Avoid This in Update (GC Trap)

```csharp
void Update()
{
    text.text = "Score: " + score;
}
```

Better:

```csharp
text.text = $"Score: {score}";
```

Best (advanced):

```csharp
StringBuilder sb = new StringBuilder(32);
sb.Append("Score: ").Append(score);
text.text = sb.ToString();
```

---

### Culture-Aware Formatting (Advanced)

```csharp
float price = 1234.56f;
price.ToString("C", CultureInfo.InvariantCulture);
```

Important for:

- Shops
- Localization
- Multiplayer

---
