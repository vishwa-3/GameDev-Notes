## 1. What problem are we solving?

Before “composition” or “inheritance”, understand **why they exist**.

### Real problem in software

You want to:

- Reuse code
- Avoid rewriting the same logic
- Build complex behavior from simpler parts
- Change behavior without breaking everything

Two main approaches:

1. **Inheritance** → “is-a” relationship
2. **Composition** → “has-a” relationship

---

## 2. Inheritance

### What is inheritance?

A child class **inherits** behavior from a parent class.

```csharp
class Animal
{
    public void Move()
    {
        Debug.Log("Moving");
    }
}

class Dog : Animal
{
    public void Bark()
    {
        Debug.Log("Bark");
    }
}
```

Here:

- `Dog` **is an** `Animal`
- `Dog` automatically gets `Move()`

This feels good at first.

---

## 3. Why inheritance breaks in real games

Let’s model a **game character**.

### Attempt 1: Inheritance approach

```csharp
class Character
{
    public int health;
    public void Move() {}
}

class Player : Character
{
    public void Shoot() {}
}

class Enemy : Character
{
    public void Attack() {}
}
```

Now the problems start.

### New requirements appear (always happens in real projects):

- Some enemies can shoot
- Some players cannot shoot
- Some characters can fly
- Some characters can swim
- Some can do all of these

### You try inheritance again

```csharp
class FlyingEnemy : Enemy {}
class ShootingEnemy : Enemy {}
class FlyingShootingEnemy : Enemy {}
```

This becomes **class explosion**.

### Problems with inheritance

- Tight coupling
- Deep class hierarchies
- Hard to modify behavior
- One change affects many classes
- Violates **Open/Closed Principle**
- Impossible to mix behaviors freely

  **Inheritance locks behavior at compile time**

---

## 4. Core idea of Composition

### Composition means:

> Build objects by **combining small behaviors**, not inheriting them.

Think in **abilities**, not in **types**.

---

## 5. Composition explained with a real-world analogy

### Human example

You are not:

- `WalkingHuman`
- `TalkingHuman`
- `SleepingHuman`

Instead, you **have**:

- Ability to walk
- Ability to talk
- Ability to sleep

Lose a leg → walking ability gone
Lose voice → talking ability gone

You are **composed of abilities**.

---

## 6. Unity is a composition engine (why this statement exists)

### Unity does NOT want inheritance

Unity wants:

- Small scripts
- One responsibility per script
- Mix & match behaviors

### Core Unity rule

> **One MonoBehaviour = one behavior**

You don’t build:

```csharp
class FlyingShootingEnemyWithAIAndHealthAndSound : Enemy
```

You build:

- Health component
- Movement component
- Shooting component
- AI component
- Sound component

---

## 7. What is a Component in Unity?

### A component:

- Is a **MonoBehaviour**
- Is attached to a **GameObject**
- Adds one specific behavior

```csharp
public class Health : MonoBehaviour
{
    public int maxHealth = 100;
    private int currentHealth;

    void Awake()
    {
        currentHealth = maxHealth;
    }

    public void TakeDamage(int damage)
    {
        currentHealth -= damage;
    }
}
```

This script:

- Knows **only about health**
- Nothing else

This is **good design**.

---

## 8. Composition example (step-by-step)

### Example: Enemy that can move and shoot

#### 1️. Movement component

```csharp
public class Movement : MonoBehaviour
{
    public float speed = 5f;

    void Update()
    {
        transform.Translate(Vector3.forward * speed * Time.deltaTime);
    }
}
```

#### 2️. Shooting component

```csharp
public class Shooter : MonoBehaviour
{
    public GameObject bulletPrefab;

    public void Shoot()
    {
        Instantiate(bulletPrefab, transform.position, transform.rotation);
    }
}
```

#### 3️. Health component

```csharp
public class Health : MonoBehaviour
{
    public int health = 100;

    public void TakeDamage(int damage)
    {
        health -= damage;
        if (health <= 0)
            Destroy(gameObject);
    }
}
```

---

### How do you create different enemies now?

| Enemy Type    | Components                       |
| ------------- | -------------------------------- |
| Basic Enemy   | Health + Movement                |
| Shooter Enemy | Health + Movement + Shooter      |
| Turret        | Health + Shooter                 |
| Boss          | Health + Movement + Shooter + AI |

**No new classes needed**

This is composition.

---

## 9. How components talk to each other

### Getting another component

```csharp
Health health;

void Awake()
{
    health = GetComponent<Health>();
}
```

### Using it

```csharp
health.TakeDamage(10);
```

This is **dependency through composition**, not inheritance.

---

## 10. Why composition scales in production

### Benefits

- Loose coupling
- Easy to test
- Easy to reuse
- Easy to remove behavior
- Designer-friendly (add/remove components in Inspector)
- No deep inheritance trees

### Industry rule

> **Prefer composition. Use inheritance only for true “is-a” relationships**

---

## 11. When inheritance is still okay in Unity

Use inheritance **sparingly**:

### Good cases

- Abstract base classes
- Shared contracts
- Framework-level structure

```csharp
public abstract class Weapon : MonoBehaviour
{
    public abstract void Fire();
}
```

```csharp
public class Gun : Weapon
{
    public override void Fire() {}
}
```

Even here:

- Weapons are still components
- Inheritance is shallow

---

## 12. Interview-level explanation

If asked:

> **Why does Unity favor composition over inheritance?**

Answer:

- Unity uses a component-based architecture
- Composition allows flexible behavior reuse
- Avoids deep inheritance hierarchies
- Encourages single responsibility
- Behavior can be changed at runtime
- Works naturally with GameObjects

---
