---
paths:
  - "Code/**"
---

# Gameplay Code Rules

- ALL gameplay values MUST come from external config/data files, NEVER hardcoded
- Use delta time for ALL time-dependent calculations (frame-rate independence)
- NO direct references to UI code — use Sandbox.Event for cross-system communication
- Every gameplay system must implement a clear interface
- Use s&box Component pattern — extend `Component`, not standalone classes
- State machines must have explicit transition tables with documented states
- Write unit tests for all gameplay logic — separate logic from presentation
- Document which design doc each feature implements in code comments
- Use `[Sync]` attribute for any property that needs network replication
- Check `IsProxy` before executing authority-only logic (damage, spawning, state changes)
- Use `GameResource<T>` for data definitions loaded from asset files

## Examples

**Correct** (component pattern, data-driven, networked):

```csharp
public class HealthComponent : Component
{
    [Sync] public float CurrentHealth { get; set; }
    [Sync] public float MaxHealth { get; set; }

    public void TakeDamage( float amount )
    {
        if ( IsProxy ) return;
        CurrentHealth = Math.Max( 0, CurrentHealth - amount );
        if ( CurrentHealth <= 0 )
            OnKilled();
    }
}
```

**Incorrect** (hardcoded, no authority check):

```csharp
public void TakeDamage()
{
    CurrentHealth -= 25.0f; // VIOLATION: hardcoded value, no authority check, no parameter
}
```
