# Technical Preferences

s&box game development project — configured for addon-based game development
on Source 2 with C# and .NET 10.

## Engine & Language

- **Engine**: s&box (Source 2, .NET 10)
- **Language**: C# (managed game code) — engine core is native C++
- **Rendering**: Source 2 renderer (forward+, deferred options via materials)
- **Physics**: Source 2 physics (Bullet-based, VPhysics)
- **UI Framework**: Razor (PanelComponent with HTML/CSS)
- **Networking**: Built-in replication (`[Sync]`, `[Broadcast]`, `IsProxy`)

## Naming Conventions

(Enforced by `.editorconfig` in sbox-public)

- **Classes/Structs**: PascalCase (`PlayerController`, `WeaponComponent`)
- **Interfaces**: IPascalCase (`IInteractable`, `IDamageable`)
- **Methods**: PascalCase (`TakeDamage()`, `OnKilled()`)
- **Properties**: PascalCase (`Health`, `MaxSpeed`)
- **Private fields**: `_camelCase` (`_health`, `_moveSpeed`)
- **Private static fields**: `s_camelCase` (`s_instance`, `s_maxCount`)
- **Public fields**: PascalCase
- **Parameters/locals**: camelCase
- **Enums**: PascalCase
- **Constants**: PascalCase (both private and public)
- **Files**: PascalCase matching the type name (`PlayerController.cs`)
- **Scenes/Prefabs**: Use s&box `.scn` scene files
- **Events/Delegates**: PascalCase with `EventHandler` suffix

## Code Style

(From sbox-public `.editorconfig`)

- **Indentation**: Tabs (size 4) for C#, spaces (size 2) for JSON/XML
- **Line endings**: CRLF
- **No `var`**: Explicit types required (`csharp_style_var_elsewhere = false`)
- **No `this.` qualification** on members
- **Expression-bodied**: Properties and accessors yes, methods and constructors no
- **File-scoped namespaces** preferred
- **CA2000** (Dispose before losing scope) is an error
- **Static local functions** are a warning

## Performance Budgets

- **Target Framerate**: 60 FPS (minimum), 144+ FPS (target)
- **Frame Budget**: 16.6ms (60 FPS), 6.9ms (144 FPS)
- **Draw Calls**: [TO BE CONFIGURED per scene complexity]
- **Memory Ceiling**: [TO BE CONFIGURED per platform]
- **Network Tick Rate**: Server-authoritative, s&box default

## Testing

- **Framework**: MSTest
- **Unit Tests**: s&box engine tests (`engine/Sandbox.Test.Unit/`)
- **Game-Level Tests**: Addon-based (`game/unittest/`)
- **Linting**: `dotnet format` (enforced by CI)
- **Minimum Coverage**: [TO BE CONFIGURED per project]

## Forbidden Patterns

- No `var` keyword — use explicit types
- No `this.` qualification on member access
- No hardcoded gameplay values — use config files or constants
- No `GameObject.Find()` in hot paths — cache references
- No direct file I/O in game code — use s&box filesystem APIs
- No blocking operations on the game thread
- No modifying third-party libraries without approval
- Standard CSS properties like `word-wrap`, `overflow-wrap`, `display:grid`, `@import` do NOT work in s&box Razor UI — check CSS compatibility before use

## Allowed Libraries / Addons

- s&box SDK APIs (Sandbox.System, Sandbox.Engine, etc.)
- Razor UI (PanelComponent, ScreenPanel, WorldPanel)
- .NET 10 BCL (System.*, Microsoft.*)
- Network Storage v3 (via sbox.cool endpoints)
- [Add approved third-party NuGet packages here as approved]

## Architecture Decisions Log

<!-- Quick reference linking to full ADRs in docs/architecture/ -->
- [No ADRs yet — use /architecture-decision to create one]
