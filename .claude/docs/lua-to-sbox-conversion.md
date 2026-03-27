# Garry's Mod Lua -> s&box C# Conversion Guide

General-purpose reference for converting any Garry's Mod gamemode from Lua to
s&box (Source 2 + .NET 10, C#). Use this alongside the `sbox-specialist`
agent for engine-specific questions.

Last verified: 2026-03-27

---

## A. Architecture Mapping

### File Organization

| GMod Pattern | s&box Equivalent |
|---|---|
| `sh_` / `sv_` / `cl_` file split | Single C# file — use `if ( IsProxy )` for client-only branches |
| `vgui/` Derma panels | Razor `.razor` files with `PanelComponent` |
| `entities/` scripted entities | Components attached to GameObjects |
| `gamemode/init.lua` entry point | GameManager Component |
| `AddCSLuaFile()` | Not needed — s&box handles client distribution |
| `include()` / `file.FindInLua()` | `using` namespaces, `.sbproj` project references |
| `resource.AddFile()` | Assets in addon's `Assets/` directory (auto-mounted) |
| `GM.*` global table | GameManager Component or static class |

### Code Patterns

| GMod Pattern | s&box Equivalent |
|---|---|
| `hook.Add("Think", fn)` | `Simulate()` override in Component |
| `hook.Add("PlayerSpawn", fn)` | `OnStart()` in PlayerController / Pawn |
| `hook.Add("PlayerDeath", fn)` | `OnKilled()` in Component |
| `hook.Add("PlayerHurt", fn)` | `TakeDamage()` in Component |
| `hook.Add("EntityTakeDamage", fn)` | Component damage event or override |
| `hook.Add("Tick", fn)` | `Simulate()` (runs per-frame, same as Think) |
| `hook.Add("Initialize", fn)` | `OnEnabled()` in Component or `Scene.Loaded` event |
| `timer.Simple(delay, fn)` | Async: `Task.Delay(ms).ContinueWith()` or track `Time.Since` in `Simulate()` |
| `timer.Create(name, delay, reps, fn)` | Repeating timer via `Time.Since` accumulator in `Simulate()` |
| `Vector(x, y, z)` | `new Vector3( x, y, z )` |
| `Angle(p, y, r)` | `new Rotation( p, y, r )` |
| `Color(r, g, b, a)` | `new Color( r, g, b, a )` |
| `math.random(min, max)` | `Random.Shared.Int( min, max )` |
| `IsValid(ent)` | `ent.IsValid()` or null check |
| `util.TraceLine()` | `Scene.Trace.Ray()` |
| `util.TraceHull()` | `Scene.Trace.Box()` |

### Entity / Object System

| GMod Pattern | s&box Equivalent |
|---|---|
| `ents.Create("prop_physics")` | `Scene.CreateObject()` + `Components.Create<ModelRenderer>()` + `Components.Create<Rigidbody>()` |
| `Entity:SetModel("model.mdl")` | `GetComponent<ModelRenderer>().Model = "model.vmdl"` |
| `Entity:SetPos(vec)` | `GameObject.Position = vec` |
| `Entity:GetPos()` | `GameObject.Position` |
| `Entity:SetAngles(ang)` | `GameObject.Rotation = rot` |
| `Entity:GetOwner()` | `GameObject.Parent` |
| `Entity:GetClass()` | Not applicable — use `GetComponent<T>()` or type checking |
| `ents.FindByClass("classname")` | `Scene.Active.GetAllComponents<T>()` |
| `Entity:Remove()` | `GameObject.Destroy()` |
| `Entity:IsValid()` | `GameObject.IsValid` |
| `constraint.Weld(a, b)` | `Components.Create<Joint.Fixed>()` |
| `Entity:GetPhysicsObject()` | `GetComponent<Rigidbody>()` |
| `PhysObj:SetVelocity(vel)` | `Rigidbody.Velocity = vel` |

### Player

| GMod Pattern | s&box Equivalent |
|---|---|
| `Player:SteamID()` | `Steamworks.SteamId` |
| `Player:Nick()` / `Player:Name()` | `Player.DisplayName` or custom RP name property |
| `Player:Alive()` | Health component check or `!IsProxy` on server |
| `Player:Frags()` | Custom score property with `[Sync]` |
| `Player:GetEyeTrace()` | `Scene.Trace.Ray( eyePos, eyePos + eyeDir * distance )` |
| `Player:GetAimVector()` | Camera direction |
| `Player:Give("weapon_name")` | Weapon equip via component system |
| `Player:StripWeapon("weapon_name")` | Weapon holster via component system |
| `player.GetAll()` | `Scene.Active.GetAllComponents<PlayerController>()` |
| `Player:SetTeam(teamID)` | Custom job/role property with `[Sync]` |
| `Player:GetTeam()` | Custom job/role property |
| `FindMetaTable("Player")` | Extension methods or component properties |
| `Player:SetModel("model.mdl")` | Pawn model component property |
| `Player:SetWalkSpeed(speed)` | Pawn controller walk speed property |
| `Player:SetRunSpeed(speed)` | Pawn controller sprint speed property |

### Networking

| GMod Pattern | s&box Equivalent |
|---|---|
| `umsg.Start("name", ply)` + writes | `[Sync]` property (auto-replicates to clients) |
| `usermessage.Hook("name", fn)` | No hook needed — `[Sync]` properties update automatically |
| `net.Start("name")` + writes | `[Sync]` for state, `[Broadcast]` for events |
| `net.Receive("name", fn)` | No receive hook needed for `[Sync]`; `[Broadcast]` methods run on clients |
| Server -> client data sync | `[Sync]` on property |
| Server -> all clients event | `[Broadcast]` on method |
| Client -> server request | Regular method call (server checks `IsProxy`) |

### Data Storage

| GMod Pattern | s&box Equivalent |
|---|---|
| `tmysql.query(sql, callback)` | Network Storage v3: `CallEndpoint( "name", data )` |
| `mysqloo` | Network Storage v3 or local JSON |
| `sqlite` | Local JSON via `FileSystem.Data` |
| `file.Write("name.txt", data)` | `FileSystem.Data.WriteJson()` |
| `file.Read("name.txt")` | `FileSystem.Data.ReadJson()` |

### UI

| GMod Pattern | s&box Equivalent |
|---|---|
| `vgui.Create("DFrame")` | `PanelComponent` subclass in `.razor` file |
| `DButton`, `DLabel`, `DTextEntry` | HTML elements in Razor template |
| `Panel:Dock(DOCK_TOP)` | CSS `display: flex` layout |
| `Panel:SetSize(w, h)` | CSS `width` / `height` |
| `Panel:Center()` | CSS positioning (margin auto, etc.) |
| `surface.DrawText()` | Razor `<span>text</span>` |
| `draw.RoundedBox()` | CSS `border-radius` |
| `hook.Add("HUDPaint", fn)` | Razor `ScreenPanel` (always-visible HUD) |
| `vgui.Register("PanelName", PANEL, "DFrame")` | `PanelComponent` class in `.razor` file |

### Audio

| GMod Pattern | s&box Equivalent |
|---|---|
| `surface.PlaySound("sound.mp3")` | `Sound.Play( "sound.vsnd" )` |
| `Entity:EmitSound("sound.wav")` | Sound component on GameObject |
| `player.GetAll()[1]:EmitSound()` | Sound.Play with position, or `[Broadcast]` to trigger client-side playback |

---

## B. Common Gamemode Systems Conversion

### 1. Gamemode Setup

**GMod:** Global `GM` table with hooks, shared config, init.lua entry point.
**s&box:** A `GameManager` Component that lives in the scene.

```csharp
// Code/Game/GameManager.cs
[Title( "Game Manager" )]
public class GameManager : Component
{
    [Property] public string GameName { get; set; } = "My Gamemode";
    [Property] public int MaxPlayers { get; set; } = 32;

    protected override void OnStart()
    {
        // Initialize game systems
    }

    [Broadcast]
    public void OnGameEvent( string eventName, string data )
    {
        // All clients receive this
    }
}
```

### 2. Player Controller / Pawn

**GMod:** Player metatable methods, GM hooks for spawn/death.
**s&box:** Pawn component with lifecycle methods.

```csharp
// Code/Game/Player/Pawn.cs
public class MyPawn : Component
{
    [Sync] public string DisplayName { get; set; }
    [Sync] public int Health { get; set; } = 100;
    [Sync] public int Cash { get; set; } = 500;

    protected override void OnStart()
    {
        // Player spawned
    }

    public void TakeDamage( float amount )
    {
        if ( IsProxy ) return;
        Health = Math.Max( 0, Health - (int)amount );
        if ( Health <= 0 ) OnKilled();
    }

    [Broadcast]
    public void OnKilled()
    {
        // Runs on all clients — show death UI, etc.
    }
}
```

### 3. Teams / Jobs

**GMod:** `team.SetUp(TEAM_POLICE, "Police", Color(0, 0, 255))` + `Player:SetTeam()`
**s&box:** Enum + property on player with `[Sync]`.

```csharp
// Code/Game/Jobs/JobType.cs
public enum JobType
{
    Citizen = 0,
    Police = 1,
    Medic = 2,
    Fireman = 3,
    Mayor = 4,
}

// Then in Pawn:
[Sync] public JobType CurrentJob { get; set; } = JobType.Citizen;
```

### 4. Items / Inventory

**GMod:** `ITEM = {}` table with `ID`, `Name`, `Cost`, `OnUse`, `Equip`, `Holster`
functions, registered via `GM:RegisterItem(ITEM)`.

**s&box:** Data-driven item definitions using `[GameResource]` or a static
registry, with an `InventoryComponent` on the player.

```csharp
// Code/Game/Items/ItemDefinition.cs
[GameResource( "Items" )]
public class ItemDefinition
{
    [Property] public string ItemID { get; set; }
    [Property] public string Name { get; set; }
    [Property] public string Description { get; set; }
    [Property] public int Weight { get; set; }
    [Property] public int Cost { get; set; }
    [Property] public int MaxStack { get; set; } = 1;
    [Property] public string InventoryModel { get; set; }
    [Property] public string WorldModel { get; set; }
    [Property] public string EquipSlot { get; set; }
}

// Code/Game/Items/InventoryComponent.cs
public class InventoryComponent : Component
{
    // Dictionary<ItemDefinition, int> for slot-based or stack-based inventory
    // Sync relevant data to client
}
```

### 5. Vehicles

**GMod:** `VEHICLE = {}` table with `Model`, `PassengerSeats`, `Cost`,
registered via `GM:RegisterVehicle(VEHICLE)`.

**s&box:** Vehicle data definition + vehicle component.

```csharp
// Code/Game/Vehicles/VehicleDefinition.cs
[GameResource( "Vehicles" )]
public class VehicleDefinition
{
    [Property] public string VehicleID { get; set; }
    [Property] public string Name { get; set; }
    [Property] public string Model { get; set; }
    [Property] public int Cost { get; set; }
    [Property] public JobType RequiredJob { get; set; }
}
```

### 6. Data Persistence

**GMod:** Direct SQL queries via tmysql/mysqloo.
**s&box:** Network Storage v3 for multiplayer persistent data.

```csharp
// Server-side: Save player data
await CallEndpoint( "save_player", new { steamId, cash, inventory, skills } );

// Server-side: Load player data
var doc = await GetDocument( "players", steamId.ToString() );
int cash = doc["cash"];
```

### 7. Networking (Detailed)

**GMod:** `net.Start` / `net.WriteString` / `net.Send` / `net.Receive`
**s&box:** `[Sync]` for state replication, `[Broadcast]` for events.

```csharp
// STATE: Use [Sync] for properties that clients need to see
[Sync] public float Health { get; set; }
[Sync] public string PlayerName { get; set; }

// EVENT (server -> all clients): Use [Broadcast]
[Broadcast]
public void OnPlayerJoined( string name )
{
    // This runs on ALL clients automatically
}

// REQUEST (client -> server): Regular method, check authority
public void RequestTeamChange( JobType newJob )
{
    // Server runs this — validate and apply
    if ( IsProxy ) return;
    CurrentJob = newJob; // [Sync] propagates to all clients
}
```

### 8. UI / HUD

**GMod:** `vgui.Create("DFrame")`, `hook.Add("HUDPaint", ...)`, Derma skin system
**s&box:** Razor `.razor` files with `PanelComponent`.

```razor
@using Sandbox.UI;

<div class="hud-container">
    <div class="health-bar">
        <div class="health-fill" style="width: @(HealthPercent)%"></div>
        <span>@Health / @MaxHealth</span>
    </div>
    <div class="cash-display">$@Cash</div>
</div>

<style>
    .hud-container {
        position: absolute;
        top: 20px;
        left: 20px;
        color: white;
        font-family: Roboto;
        font-size: 16px;
    }
    .health-bar {
        width: 200px;
        height: 20px;
        background: rgba(0, 0, 0, 0.5);
        border-radius: 4px;
    }
    .health-fill {
        height: 100%;
        background: #e74c3c;
    }
    .cash-display {
        margin-top: 8px;
    }
</style>

@code {
    [Bind] public int Health { get; set; }
    public int MaxHealth { get; set; } = 100;
    public float HealthPercent => MaxHealth > 0 ? (float)Health / MaxHealth * 100 : 0;
    [Bind] public int Cash { get; set; }
}
```

### 9. NPCs / Shop Entities

**GMod:** Scripted entities with `ENT:AcceptInput`, `ENT:Use`, key-value setup.
**s&box:** `[Interact]` component attribute on GameObjects.

```csharp
// Code/Game/NPCs/ShopNPC.cs
public class ShopNPC : Component
{
    [Property] public string ShopName { get; set; }

    [Interact( "Buy items" )]
    private void OnInteract( GameObject player )
    {
        // Server-side: validate and sell item
        // Could also broadcast to open shop UI on client
    }
}
```

### 10. Doors / Properties

**GMod:** Door entity manipulation with `ent:SetDTBool` and `ent:Fire("unlock")`.
**s&box:** Door entity interaction via components, property ownership tracked in data.

```csharp
public class DoorComponent : Component
{
    [Sync] public string OwnerSteamId { get; set; }
    [Sync] public bool IsLocked { get; set; }
    [Sync] public string PropertyName { get; set; }

    [Interact( "Open / Close" )]
    private void OnInteract( GameObject player )
    {
        if ( IsProxy ) return;
        // Check ownership, lock status, toggle door
    }
}
```

### 11. Chat System

**GMod:** `GM:PlayerSay` hook, `chat.AddText` for display.
**s&box:** `[Broadcast]` RPC for chat messages, Razor panel for chat UI.

```csharp
// Server receives chat input, broadcasts to all
[Broadcast]
public void OnChatMessage( string playerName, string message, ChatType type )
{
    // All clients receive and display in their chat panel
}
```

### 12. Timers / Scheduled Tasks

**GMod:** `timer.Simple(delay, fn)`, `timer.Create(name, delay, reps, fn)`
**s&box:** Track time with `Time.Since` in `Simulate()`, or use `Task.Delay`.

```csharp
// Pattern: Repeating timer via Simulate()
private TimeSince _nextPayDay;

protected override void Simulate()
{
    if ( _nextPayDay > 60 ) // Every 60 seconds
    {
        _nextPayDay = 0;
        ProcessPayDay();
    }
}

// Pattern: One-shot delay
private async void DelayedAction()
{
    await Task.Delay( 5000 );
    // Do something after 5 seconds
}
```

### 13. Weapons / Tools

**GMod:** SWEP base with `SWEP:PrimaryAttack`, `SWEP:SecondaryAttack`.
**s&box:** Weapon component with `Simulate` for input handling.

```csharp
public class MyWeapon : Component
{
    [Property] public int Damage { get; set; } = 25;
    [Property] public float FireRate { get; set; } = 0.1f;

    private TimeSince _timeSinceLastShot;

    protected override void Simulate()
    {
        if ( Input.Down( "attack1" ) && _timeSinceLastShot > FireRate )
        {
            if ( IsProxy ) return;
            _timeSinceLastShot = 0;
            Fire();
        }
    }

    private void Fire()
    {
        // Raycast, apply damage, play sound, etc.
    }
}
```

### 14. Effects / Particles

**GMod:** `util.Effect("effect_name", data)`, `ParticleEmitter`.
**s&box:** Particle system components, or s&box particle editor tools.

Effects in s&box are primarily authored through the editor using Source 2's
particle system. For programmatic triggering, use the particle component API.

---

## C. Common Gotchas

1. **No `var` keyword** — s&box enforces explicit types (`string name`, not `var name`)
2. **No `this.` qualification** — `this.Health` is wrong, just use `Health`
3. **CSS limitations** — `display: grid`, `word-wrap`, `@import` and many other standard CSS properties do NOT work in s&box Razor UI
4. **`IsProxy` check** — Always check before modifying game state. Clients have proxy copies of server data.
5. **`[Sync]` is one-way** — Server to clients only. Clients cannot modify synced properties.
6. **No direct file I/O in game code** — Use `FileSystem.Data` APIs or Network Storage v3
7. **No SQL in game code** — Use Network Storage v3 for multiplayer persistence
8. **s&box uses tabs, not spaces** — Enforced by `.editorconfig`
9. **CRLF line endings** — Enforced by `.editorconfig`
10. **`Scene.CreateObject()` not `new GameObject()`** — Use the scene API
11. **Components, not inheritance** — Prefer composition (adding Components) over deep class hierarchies
12. **Razor UI must not own game state** — Display only, dispatch events to game layer

---

## D. Conversion Workflow (7 Phases)

### Phase 1: Analyze Existing Gamemode

Map out the complete gamemode structure:
- List all `sh_`/`sv_`/`cl_` files and what they do
- Identify all systems (inventory, jobs, vehicles, etc.)
- Catalog all data definitions (items, vehicles, properties, etc.)
- Map all UI panels (vgui files)
- Identify all SQL queries and data persistence
- Identify all networking (umsg/net/usermessage)
- Identify all entity definitions

### Phase 2: Set Up s&box Addon Project

Create the addon structure:
```
Code/
  Game/            # Core game code
    GameManager.cs
  Player/          # Pawn, PlayerController
  Items/           # Item definitions
  Vehicles/        # Vehicle definitions
  Jobs/            # Job system
  UI/              # Razor panels
  NPCs/            # NPC interactions
Assets/            # Models, sounds, materials
Editor/            # Editor tools
```

### Phase 3: Create Foundation Components

- GameManager (entry point, game configuration)
- Pawn / PlayerController (player spawn, death, movement)
- Basic networking infrastructure

### Phase 4: Convert Data Definitions

Convert all table-based definitions to C# classes:
- Item definitions -> `ItemDefinition` GameResource
- Vehicle definitions -> `VehicleDefinition` GameResource
- Property definitions -> `PropertyDefinition` GameResource
- Recipe/crafting definitions -> `RecipeDefinition` GameResource

### Phase 5: Convert Game Systems

- Jobs/teams system
- Inventory system
- Economy (cash/bank)
- Player data persistence (Network Storage v3)
- NPC interactions
- Property/door ownership
- Chat system

### Phase 6: Convert UI

Convert each vgui panel to a Razor PanelComponent:
- HUD -> ScreenPanel
- Menus/frames -> PanelComponent
- Inventory grid -> Flex layout in Razor
- Chat box -> ScreenPanel with text input

### Phase 7: Port Content and Test

- Convert Source models/materials to s&box format (.vmdl/.vmat/.vtex)
- Port sounds to .vsnd format
- Build and test each system incrementally
- Fix CSS compatibility issues in Razor UI
