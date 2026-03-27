---
name: convert-gamemode
description: "Analyze and convert a Garry's Mod Lua gamemode to s&box C#. Orchestrates the 7-phase conversion workflow."
argument-hint: "<subcommand> [system] (e.g., 'analyze', 'plan', 'status', 'convert items')"
user-invocable: true
allowed-tools: Read, Glob, Grep, Write, Edit, Bash
---

# GMod -> s&box Gamemode Conversion

This skill guides the conversion of a Garry's Mod Lua gamemode to s&box C#.
It orchestrates the 7-phase workflow defined in `.claude/docs/lua-to-sbox-conversion.md`.

Reference that document for the full API mapping table and code examples.

---

## Workflow

### 1. Parse Arguments

**Format**: `/convert-gamemode <subcommand> [system]`

**Subcommands**:
- `analyze` — Scan the GMod Lua source and catalog all systems, files, and data (Phase 1)
- `plan` — Generate a prioritized conversion plan with system-by-system breakdown
- `status` — Check current conversion progress against the plan
- `convert <system>` — Convert a specific system from Lua to C#

**System names**: Any system found during analysis (e.g., items, vehicles, player,
  inventory, jobs, chat, ui, properties, networking, etc.)

**Examples**:
```
/convert-gamemode analyze
/convert-gamemode plan
/convert-gamemode status
/convert-gamemode convert items
/convert-gamemode convert networking
```

---

## Subcommand: analyze

### 1. Locate the GMod Source

Scan for the gamemode directory structure. Look for:
- `gamemode/` directory with `init.lua`, `sh_init.lua`
- `sh_*.lua`, `sv_*.lua`, `cl_*.lua` file pattern
- `vgui/` directory
- `entities/` directory
- `items/`, `vehicles/`, `npcs/`, `properties/`, `mixtures/`, `shops/` subdirectories
- `content/` directory (models, materials, sounds)

Common locations:
- `PERP3_old/gamemode/`
- `garrysmod/gamemode/`
- Root `gamemode/`

If multiple Lua directories exist, ask which to convert.

### 2. Catalog Everything

For each category, read and enumerate:

**Files** (list every .lua file with its size):
- `sh_*.lua` (shared) — count and list
- `sv_*.lua` (server) — count and list
- `cl_*.lua` (client) — count and list
- `vgui/*.lua` (UI panels) — count and list
- `sh_modules/*.lua` (shared modules) — count and list
- `sv_modules/*.lua` (server modules) — count and list
- `cl_modules/*.lua` (client modules) — count and list
- `items/*.lua` (item definitions) — count and list
- `vehicles/*.lua` (vehicle definitions) — count and list
- `npcs/*.lua` (NPC definitions) — count and list
- `properties/*.lua` (property definitions) — count and list
- `mixtures/*.lua` (crafting recipes) — count and list
- `shops/*.lua` (shop definitions) — count and list
- `entities/entities/*.lua` (scripted entities) — count and list
- `entities/weapons/*.lua` (SWEP definitions) — count and list

**Systems** (identify by reading key files):
- Read `sh_config.lua` for: teams, constants, game rules, prices, configuration
- Read `init.lua` for: includes, addon resources, max players
- Read `sh_player.lua` for: player methods, data properties, networking
- Read `sv_networking.lua` for: SQL queries, data persistence, net messages
- Read `cl_networking.lua` for: client-side networking, umsg hooks
- Scan `sv_*.lua` files for: server systems, hooks, timers, database operations
- Scan `vgui/*.lua` files for: UI panels, HUD elements, menus

**Data Counts**:
- Number of item definitions (individual ITEM = {} registrations)
- Number of vehicle definitions
- Number of property definitions
- Number of NPC definitions
- Number of crafting recipes
- Number of weapon definitions

**Networking Points**:
- Count `umsg.Start` / `usermessage.Hook` calls
- Count `net.Start` / `net.Receive` calls
- Count `tmysql.query` / SQL statements
- Identify all tables/columns used in SQL

### 3. Present Analysis Summary

```
=== GMod Gamemode Analysis ===

GAMEMODE: [name] (from info.txt or sh_config.lua)
SOURCE: [directory path]
TOTAL LUA FILES: [count]

FILE BREAKDOWN:
  Shared (sh_): [count]
  Server (sv_): [count]
  Client (cl): [count]
  UI (vgui): [count]
  Items: [count]
  Vehicles: [count]
  Properties: [count]
  NPCs: [count]
  Mixtures: [count]
  Shops: [count]
  Entities: [count]
  Weapons: [count]
  Modules: [count]

SYSTEMS IDENTIFIED:
  [list each system with file count and complexity estimate]

DATA DEFINITIONS:
  Items: [count]
  Vehicles: [count]
  Properties: [count]
  NPCs: [count]
  Recipes: [count]
  Weapons: [count]

NETWORKING:
  umsg/net messages: [count]
  SQL queries: [count]
  Database tables: [list]

UI PANELS:
  [list each panel]

ESTIMATED CONVERSION EFFORT:
  [rough estimate based on complexity]
```

### 4. Ask to Proceed

"Analysis complete. Shall I generate a conversion plan? (`/convert-gamemode plan`)"

---

## Subcommand: plan

### 1. Review Analysis

Read any existing analysis output or re-run a quick scan if needed.

### 2. Prioritize Systems

Priority order (high to low):
1. **Foundation** — GameManager, Pawn, PlayerController (required for everything else)
2. **Data Persistence** — Player data loading/saving (required for multiplayer)
3. **Networking** — Core sync infrastructure (required for multiplayer)
4. **Teams/Jobs** — Role system (many systems depend on job checks)
5. **Items/Inventory** — Central to most gamemodes
6. **Economy** — Cash, banking, trading
7. **Player UI** — HUD (players need feedback to test other systems)
8. **Vehicles** — If applicable
9. **Properties/Doors** — If applicable
10. **Chat** — Basic communication
11. **NPCs/Shops** — Interactable world elements
12. **Crafting/Mixtures** — If applicable
13. **Advanced Systems** — Organizations, weather, time, etc.
14. **Polish** — Scoreboard, settings, admin tools

### 3. Generate Conversion Plan

For each system, specify:
- Source Lua files to read
- Target C# files to create
- Conversion complexity (LOW/MEDIUM/HIGH)
- Dependencies on other systems
- Key conversion challenges

### 4. Present Plan and Get Approval

"Here's the recommended conversion plan, prioritized by dependency order.
Shall I begin with Phase 1 (Foundation)?"

---

## Subcommand: status

### 1. Check Conversion Progress

Look for existing s&box C# files in `Code/` and compare against the plan:
- Which systems have been started?
- Which are in progress?
- Which haven't been started?
- Are there any blockers?

### 2. Present Status Report

```
=== Conversion Status ===

PHASE 1 (Foundation): [NOT STARTED / IN PROGRESS / COMPLETE]
PHASE 2 (Project Setup): [status]
PHASE 3 (Core Components): [status]
PHASE 4 (Data Definitions): [status]
PHASE 5 (Game Systems): [status]
PHASE 6 (UI): [status]
PHASE 7 (Content): [status]

SYSTEMS:
  GameManager:     [status] (Code/Game/GameManager.cs)
  Player/Pawn:      [status] (Code/Game/Player/Pawn.cs)
  Jobs:            [status] (Code/Game/Jobs/)
  Items:            [status] (X/Y definitions converted)
  Vehicles:         [status]
  ...

BLOCKERS:
  [any current issues]
```

---

## Subcommand: convert <system>

### 1. Read Source Files

Read the GMod Lua source files for the specified system. Use the conversion
guide (`.claude/docs/lua-to-sbox-conversion.md`) to map patterns.

### 2. Identify Conversion Challenges

For each Lua file in the system:
- What GMod APIs are used?
- Are there s&box equivalents?
- What needs architectural redesign (e.g., SQL -> Network Storage v3)?
- What's a direct 1:1 conversion vs. needs rethinking?

### 3. Propose C# Architecture

Show the proposed file structure and class design:
```
Target files for [system]:
  Code/Game/[System]/[System]Component.cs
  Code/Game/[System]/[System]Definition.cs
  Code/Game/[System]/[System]UI.razor (if UI needed)
```

### 4. Write C# Code

Convert the Lua code to s&box C#, following these rules:
- Use `Component` pattern, not inheritance
- Use `[Sync]` for networked properties, `[Broadcast]` for RPCs
- Check `IsProxy` before authority-only logic
- Use `GameResource` for data definitions
- Use Razor PanelComponent for UI (never built-in widgets)
- Follow `.editorconfig` conventions (no var, no this., tabs)
- Reference the API mapping table in the conversion guide

### 5. Test Incrementally

After writing each system:
- Verify it compiles (`dotnet build`)
- Verify networking works in s&box editor
- Check CSS compatibility for any Razor UI
- Test the feature end-to-end

---

## Tips for Agents

- Always read the conversion guide before converting: `.claude/docs/lua-to-sbox-conversion.md`
- For complex systems, delegate to `lua-conversion-specialist` agent
- For s&box-specific API questions, consult `sbox-specialist` agent
- For UI conversions, consult `sbox-razor-specialist` agent
- For networking, consult `sbox-networking-specialist` agent
- Copy the conversion checklist template and fill it in: `.claude/docs/templates/lua-to-sbox-conversion.md`
