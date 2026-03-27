# s&box Engine Version Reference

## Current Version

- **Engine**: s&box (Source 2)
- **Runtime**: .NET 10
- **Language**: C# (managed game code), C++ (native engine core)
- **UI Framework**: Razor (PanelComponent with HTML/CSS)
- **Project Format**: `.sbproj` addon projects
- **Solution Format**: `.slnx` (custom format)

## Source Reference

- Engine source: `sbox-public/` (local clone of public s&box repo)
- Build commands: `./Bootstrap.bat` or individual `SboxBuild` steps
- Editor: `Sbox-Dev` (from `game/bin/`)

## Knowledge Gap Window

- Last verified: 2026-03-27
- LLM training cutoff: May 2025
- s&box is actively developed — APIs may have changed after the training cutoff
- Always verify API usage against `sbox-public/` source code when uncertain
- For the latest docs: https://sbox.game/ and https://sbox.game/api/

## Quick API Reference

### Key Namespaces

| Namespace | Purpose |
|-----------|---------|
| `Sandbox` | Core types (Component, GameObject, Scene) |
| `Sandbox.UI` | UI system (PanelComponent, ScreenPanel, WorldPanel) |
| `Sandbox.Services` | External services (leaderboards, storage) |
| `Sandbox.Engine` | Core engine (rendering, physics) |
| `Sandbox.Event` | Event system |

### Key Patterns

| Pattern | How |
|---------|-----|
| Spawn object | `Scene.CreateObject()` |
| Add component | `Components.Create<T>()` |
| Get component | `GetComponent<T>()` |
| Network sync | `[Sync]` attribute on properties |
| Server RPC | `[Broadcast]` attribute on methods |
| Check authority | `IsProxy` property |
| Game data | `[GameResource<T>]` attribute |
| UI panel | Extend `PanelComponent`, use .razor files |

## Breaking Changes

<!-- Add breaking API changes here as discovered -->
- None tracked yet

## Deprecated APIs

<!-- Add deprecated patterns here -->
- None tracked yet
