# Directory Structure

```text
/
├── CLAUDE.md                    # Master configuration
├── .sbproj                      # s&box addon project file
├── .claude/                     # Agent definitions, skills, hooks, rules, docs
├── Code/                        # Main game C# source code
│   ├── Game/                    # Game-specific code (gamemode, entities, components)
│   ├── UI/                      # Razor UI panels and components
│   ├── Networking/              # Network-specific code (if separated)
│   └── AI/                      # AI and behavior code (if separated)
├── Libraries/                   # Third-party libraries (do not modify)
├── Editor/                      # Editor panels and tools (Hammer extensions, custom editors)
├── Assets/                      # Game assets
│   ├── Models/                  # 3D models (.vmdl)
│   ├── Materials/               # Materials (.vmat)
│   ├── Textures/                # Textures (.vtex)
│   ├── Sounds/                  # Sound files (.vsnd)
│   ├── Animations/              # Animation files (.vanim)
│   ├── Shaders/                 # Custom shaders
│   ├── Data/                    # Data files (JSON configs, tables)
│   └── Maps/                    # Map files (.vmap)
├── design/                      # Game design documents (gdd, narrative, levels, balance)
├── docs/                        # Technical documentation (architecture, api, postmortems)
│   └── engine-reference/        # Curated engine API snapshots (version-pinned)
│       └── sbox/                # s&box reference docs
├── tests/                       # Test suites (unit, integration, performance, playtest)
├── prototypes/                  # Throwaway prototypes (isolated from Code/)
├── production/                  # Production management (sprints, milestones, releases)
│   ├── session-state/           # Ephemeral session state (active.md — gitignored)
│   └── session-logs/            # Session audit trail (gitignored)
└── sbox-public/                 # s&box engine source reference (read-only)
```
