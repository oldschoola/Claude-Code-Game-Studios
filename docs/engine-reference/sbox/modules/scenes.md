# s&box Scenes Module

Last verified: 2026-03-27

## Key Types
- `Scene` — Scene container for GameObjects
- `Scene.Active` — The current active scene
- `GameObject` — Entity that holds Components
- `Scene.CreateObject()` — Spawn a new GameObject

## Patterns

**Spawning an object**:
```csharp
var obj = Scene.Active.CreateObject();
obj.AddComponent<MyComponent>();
obj.Position = new Vector3( 0, 100, 0 );
```

**Finding objects**:
```csharp
var player = Scene.Active.GetAllComponents<PlayerController>().FirstOrDefault();
```

**Destroying objects**:
```csharp
obj.Destroy(); // Safe destruction
```

## Notes
- GameObjects are destroyed when all components are removed
- Use `Simulate()` in components for per-frame updates
- `OnEnabled()` / `OnDisabled()` handle component lifecycle
