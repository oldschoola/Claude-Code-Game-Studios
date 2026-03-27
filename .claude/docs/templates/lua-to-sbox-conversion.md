# GMod -> s&box Conversion Checklist

Template for converting any Garry's Mod Lua gamemode to s&box C#.
Copy this file, fill it in for your gamemode, and update it as you progress.

---

## 1. Pre-Conversion Analysis

### Gamemode Info
- **Gamemode Name**:
- **GMod Version**: (e.g., PERP3, DarkRP, TTT, Sandbox)
- **Target s&box Addon Name**:
- **Multiplayer?**: Yes / No (determines if Network Storage v3 is needed)

### Systems Inventory
For each system, note the source files and complexity.

| System | Source Files (Lua) | Lines (approx) | Complexity | Priority |
|--------|-------------------|----------------|------------|----------|
| Player Controller | `sh_player.lua`, `sv_player.lua`, `cl_player.lua` | | | HIGH |
| Teams/Jobs | `sh_config.lua`, `sv_modules/job_*.lua` | | | HIGH |
| Items/Inventory | `sh_items.lua`, `sv_items.lua`, `cl_items.lua`, `items/*.lua` | | | HIGH |
| Vehicles | `sh_vehicles.lua`, `sv_vehicles.lua`, `vehicles/*.lua` | | | MEDIUM |
| Data Persistence | `sv_networking.lua` (SQL queries) | | | HIGH |
| Networking | `sv_networking.lua`, `cl_networking.lua` | | | HIGH |
| UI/HUD | `vgui/*.lua` | | | MEDIUM |
| Properties/Doors | `sh_property.lua`, `properties/*.lua` | | | MEDIUM |
| NPCs/Shops | `sh_npcs.lua`, `npcs/*.lua` | | | LOW |
| Chat | `sv_chat.lua`, `cl_chat.lua` | | | MEDIUM |
| Crafting/Mixtures | `sh_mixtures.lua`, `mixtures/*.lua` | | | LOW |
| Weather/Time | `sv_modules/system_weather.lua`, `sh_modules/system_time*.lua` | | | LOW |
| Organizations/Gangs | `sv_organizations.lua` | | | LOW |
| Trade/Market | `sv_trade.lua`, `cl_trade.lua` | | | LOW |
| Weapons | `entities/weapons/` | | | MEDIUM |
| Scoreboard | `cl_scoreboards.lua`, `scoreboards/` | | | LOW |
| Admin System | `sv_modules/system_admin.lua` | | | LOW |

### Data Definitions Count
- **Items**: ____
- **Vehicles**: ____
- **Properties**: ____
- **NPCs**: ____
- **Recipes/Crafting**: ____
- **Weapons**: ____
- **Jobs**: ____

### Networking Points
- **umsg/usermessage calls** (count): ____
- **net.Start/Receive calls** (count): ____
- **SQL tables used**: ____

### UI Panels Count
- **vgui panels**: ____
- **HUD elements**: ____

---

## 2. File Mapping Template

Map each source file to its target C# location.

| Source Lua File | Target C# File | Notes |
|----------------|---------------|-------|
| `gamemode/init.lua` | `Code/Game/GameManager.cs` | Entry point |
| `gamemode/sh_config.lua` | `Code/Game/GameConfig.cs` | Constants, team defs |
| `gamemode/sh_player.lua` | `Code/Game/Player/PlayerShared.cs` | Shared player methods |
| `gamemode/sv_player.lua` | `Code/Game/Player/PlayerController.cs` | Server player logic |
| `gamemode/cl_player.lua` | `Code/Game/Player/PlayerClient.cs` | Client-only player UI |
| `gamemode/sh_items.lua` | `Code/Game/Items/ItemRegistry.cs` | Item registration |
| `gamemode/sv_items.lua` | `Code/Game/Items/ItemSystem.cs` | Item server logic |
| `gamemode/items/*.lua` | `Code/Game/Items/Definitions/*.cs` | Individual item defs |
| `gamemode/vgui/*.lua` | `Code/UI/*.razor` | UI panels |
| `gamemode/sv_networking.lua` | `Code/Game/Networking/*.cs` | Sync/Broadcast |
| `gamemode/entities/*.lua` | `Code/Game/Entities/*.cs` | Scripted entities |
| ... | ... | ... |

---

## 3. Phase Progress Tracker

### Phase 1: Analysis
- [ ] Catalog all source files
- [ ] Map all systems
- [ ] Count all data definitions
- [ ] Identify all SQL queries
- [ ] Identify all networking calls
- [ ] List all UI panels
- [ ] Identify external dependencies (URLs, web APIs)

### Phase 2: Project Setup
- [ ] Create `.sbproj` addon project
- [ ] Create directory structure (`Code/`, `Assets/`, `Editor/`)
- [ ] Verify s&box editor can load the addon
- [ ] Set up basic GameManager component
- [ ] Set up basic Pawn component

### Phase 3: Foundation
- [ ] GameManager component working
- [ ] Player spawn/despawn working
- [ ] Basic movement and camera
- [ ] Basic `[Sync]` property replication verified
- [ ] Basic `[Broadcast]` RPC verified

### Phase 4: Data Definitions
- [ ] Item definitions converted (____ / ____)
- [ ] Vehicle definitions converted (____ / ____)
- [ ] Property definitions converted (____ / ____)
- [ ] Recipe definitions converted (____ / ____)
- [ ] Job/team definitions converted

### Phase 5: Game Systems
- [ ] Jobs/team assignment system
- [ ] Inventory system (pickup, drop, equip)
- [ ] Economy (cash, bank)
- [ ] Player data persistence (Network Storage v3)
- [ ] NPC interaction system
- [ ] Property/door ownership
- [ ] Chat system
- [ ] Vehicle spawning/driving
- [ ] Crafting/mixing

### Phase 6: UI
- [ ] HUD (health, cash, job display)
- [ ] Inventory UI
- [ ] Shop/trade UI
- [ ] Scoreboard
- [ ] Chat UI
- [ ] Vehicle UI
- [ ] Property UI
- [ ] Settings/menu UI

### Phase 7: Content & Polish
- [ ] Models converted to .vmdl
- [ ] Materials converted to .vmat
- [ ] Sounds converted to .vsnd
- [ ] Maps ported or recreated
- [ ] CSS compatibility issues resolved
- [ ] Performance tested
- [ ] Multiplayer tested

---

## 4. Common Gotchas Checklist

Before finishing each system, verify:

- [ ] No `var` keyword used (use explicit types)
- [ ] No `this.` qualification on members
- [ ] Tabs (not spaces) for C# indentation
- [ ] CRLF line endings
- [ ] `[Sync]` properties are only for data clients need to observe
- [ ] Server-authoritative: `IsProxy` checked before mutations
- [ ] UI does NOT own game state (display only, dispatch events)
- [ ] CSS properties checked for s&box compatibility
- [ ] No hardcoded gameplay values (use config/data)
- [ ] All user-facing text goes through localization
- [ ] No direct SQL queries (use Network Storage v3)
- [ ] File-scoped namespaces used
- [ ] No `util.Effect` / old GMod patterns remaining
