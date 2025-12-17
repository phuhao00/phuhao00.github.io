# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Dota 2 custom game mod named "sword_gate_defense" built with X-Template - a TypeScript-based development framework for Dota 2 modding. The project uses:
- **Backend (VScripts)**: TypeScript → Lua via TypeScript-to-Lua (TSTL) in `game/scripts/src/`
- **Frontend (Panorama UI)**: TypeScript + React via Webpack in `content/panorama/src/`
- **Data Pipeline**: Excel → KV files → JSON via Gulp tasks

## Development Commands

### Essential Commands
```bash
# First time setup - creates symlinks to dota 2 beta/dota_addons/
yarn install

# Start development mode (keep running) - watches and auto-compiles all sources
yarn dev

# Launch Dota 2 with your map (recommended for testing)
yarn launch [map_name]    # e.g., yarn launch temp
yarn launch               # Tools mode only

# Production build (prompts for encryption mode)
yarn prod
```

### Individual Build Tasks
```bash
# Lint and fix code
yarn lint

# Excel → KV → JSON pipeline
gulp sheet_2_kv          # Excel to KV
gulp kv_2_js             # KV to JSON
gulp jssync              # Run both above

# Localization
gulp csv_to_localization  # CSV to addon_*.txt

# Images
gulp create_image_precache  # Generate image precache CSS
# or: yarn imgcache

# LESS compilation
gulp compile_less         # Compile LESS to CSS
yarn clessw              # Watch mode

# Format code
yarn formatcode          # Format both backend and frontend
yarn formatbcode         # Format backend only
yarn formatfcode         # Format frontend only

# API generation (from OpenAPI spec)
yarn api
```

### Build Components Running in Parallel
When you run `yarn dev`, these three watch tasks run in parallel:
1. `gulp dev` - Watches Excel→KV→JSON, CSV→localization, images→precache CSS, LESS→CSS
2. `webpack --watch` - Watches Panorama TypeScript/React → JavaScript bundles
3. `tstl --watch` - Watches VScripts TypeScript → Lua

## Project Architecture

### Three-Layer Build System

1. **VScripts (Server/Game Logic)**: `game/scripts/src/` → compiled to `game/scripts/vscripts/*.lua` via TSTL
2. **Panorama UI (Client)**: `content/panorama/src/` → compiled to `content/panorama/layout/custom_game/` via Webpack
3. **Data Layer**: `excels/*.xlsx` → `game/scripts/npc/*.kv` → JSON in both `game/scripts/src/json/` and `content/panorama/src/json/`

### Entry Points

- **Server**: `game/scripts/src/addon_game_mode.ts` exports `Activate()` and `Precache()` functions
- **Module Registration**: `game/scripts/src/index.ts` contains `ActivateModules()` which initializes all singleton systems on `GameRules`
- **Client**: Each UI panel has `layout.xml` that references a `.tsx` script (e.g., `content/panorama/src/hud/layout.xml` → `script.tsx`)

### Core Systems (Singleton Pattern)

All core systems are mounted as singletons on `GameRules`, initialized in `ActivateModules()`:

```typescript
// Accessed anywhere in server-side code
GameRules.EventBus         // Pub/sub event system
GameRules.Gameplay         // Game state machine
GameRules.CharacterSystem  // Player character management
GameRules.CustomAttrSystem // Custom attribute system with dependencies
GameRules.EnemySystem      // Enemy spawning and management
GameRules.XNetTable        // Enhanced NetTable wrapper for client-server sync
```

### Actor-Component Pattern

The project uses an Actor-Component pattern similar to ECS:

- **Actor**: Base class for game entities (`game/scripts/src/actor/Actor.ts`)
  - Examples: `PlayerCharacter`, `Enemy`
  - Manages a collection of components

- **Component**: Reusable functionality modules (`game/scripts/src/component/BaseComponent.ts`)
  - Examples: `PlayerAttrsComp`, `AbilityPoolComp`, `AbilityBarComp`
  - Lifecycle: `onAttach(owner)` → business logic → `onDetach()` for cleanup
  - Auto-manages event subscriptions via `registerEvent()` method

```typescript
// Example usage
export class PlayerCharacter extends Actor {
  constructor(playerId: PlayerID, name: string) {
    super();
    this.addComponent(new PlayerAttrsComp());
    this.addComponent(new AbilityPoolComp());
  }

  get Attrs(): PlayerAttrsComp {
    return this.getComponent(PlayerAttrsComp);
  }
}
```

### Event-Driven Architecture

**EventBus (Server-Side)** - Type-safe pub/sub for game logic:
- Define events in `game/scripts/src/event_bus/EventList.ts`
- Subscribe with priority support: `GameRules.EventBus.subscribe(eventName, handler, priority)`
- One-time handlers: return `true` to auto-unsubscribe
- Conditional subscription: `subscribeUntil(eventName, untilEvent, handler)`

**CustomGameEvents** - Client-server communication:
- Declare in `game/scripts/src/shared/gameevents.d.ts`
- Client → Server: `GameEvents.SendCustomGameEventToServer(eventName, data)`
- Server listens: `CustomGameEventManager.RegisterListener(eventName, handler)`

### XNetTable System

Enhanced network table that bypasses Valve's 2MB NetTable limit using chunked event transmission:
- Server: `GameRules.XNetTable.SetTableValue(table, key, value)`
- Server (player-specific): `GameRules.XNetTable.SetPlayerTableValue(playerId, table, key, value)`
- Client: `useXNetTableKey(table, key)` React hook
- Definitions: `game/scripts/src/shared/x-net-table.d.ts`

**When to use**: Large data (>100KB), low update frequency (<1/sec). For small, high-frequency data, use native `CustomNetTables` instead.

### Custom Attributes System

Located in `game/scripts/src/system/custom_attr/`. Supports dependency chains:

- **Independent Attributes**: `CustomAttr_Independent` - Set values directly
- **Dependent Attributes**: `CustomAttr_Dependency` - Auto-recalculate when dependencies change

```typescript
// Example: HealthPointFinal depends on BaseHealthPoint
class HealthPointFinal extends CustomAttr_Dependency {
  protected calculateValue(): void {
    this.resultValue = base * (1 + addPer * 0.01) * mulPer;
  }
}
```

Attribute modifiers support duration, source tracking, and force override.

## Directory Structure

### Server-Side (`game/scripts/src/`)
```
├── actor/              # Game entities (Actor base class, PlayerCharacter, Enemy)
├── component/          # Component system (BaseComponent, various component implementations)
├── system/             # Global systems (Debug, CharacterSystem, CustomAttrSystem, etc.)
├── event_bus/          # Event system (EventBus, EventList)
├── game/               # Game logic (GamePlay, GameConfig, state machine, game modes)
├── abilities/          # Custom abilities
├── modifiers/          # Custom modifiers
├── shared/             # Frontend-backend shared types (gameevents.d.ts, x-net-table.d.ts)
├── utils/              # Utility functions (precache, tstl-utils, dota_ts_adapter)
├── config/             # Configuration (KVData access)
├── global/             # Global functions and declarations
├── index.ts            # ActivateModules entry point
└── addon_game_mode.ts  # Dota 2 main entry point
```

### Client-Side (`content/panorama/src/`)
```
├── hud/                # Main game UI
├── loading-screen/     # Loading screen
├── end_screen/         # End screen
├── mgr/                # Frontend managers
├── view/               # View components
├── agent/              # Agent pattern implementations
├── hooks/              # React hooks (useXNetTable, useKeyboard, useStateIfMounted)
├── utils/              # Utilities (event-bus, keybinding, sequential-actions)
├── def/                # Type definitions
├── shared/             # Symlink to game/scripts/src/shared/
└── json/               # KV-generated JSON (auto-generated)
```

## Code Conventions

### TypeScript Naming
- **Classes/Interfaces**: PascalCase (`PlayerCharacter`, `ICustomState`)
- **Enums**: PascalCase for enum and members (`GameStateType.Init`)
- **Functions/Methods**: camelCase (`calculateDamage`, `getValue`)
- **Private members**: `_` prefix (`_eventMap`)
- **Protected members**: No prefix (`protected owner`)
- **React Components**: PascalCase (`BlessAbilityUI`)
- **Hooks**: `use` prefix + camelCase (`useXNetTableKey`)

### File Organization
- Group related functionality in subdirectories
- One class per file (exception: small related utilities)
- Use index files to re-export public APIs where appropriate

## TypeScript to Lua (TSTL) Constraints

### Avoid These Features
- `async/await` - Use callbacks instead
- Destructuring in function parameters - Destructure in function body
- `for...of` loops - Prefer `pairs()` or `ipairs()` for performance

### Decorators
- `@reloadable` - Enables hot-reloading during development
- `@registerModifier()` - Registers modifier with Dota 2 engine

## Component Lifecycle Management

### Critical: Always Clean Up

**Components must implement `onDetach()` properly**:
```typescript
onDetach(): void {
  // 1. Clean up EventBus subscriptions (auto-managed via registerEvent)
  this.eventRecord.forEach((name, id) =>
    GameRules.EventBus.unsubscribeByID(name, id)
  );

  // 2. Clean up CustomGameEvent listeners
  this.customGameEventListeners.forEach(id =>
    CustomGameEventManager.UnregisterListener(id)
  );

  // 3. Clean up timers
  if (this.timer) Timers.RemoveTimer(this.timer);

  // 4. Clear references
  this.eventRecord.clear();
}
```

**Actors must implement `onDestroy()`** to clean up all components and resources.

## Data Pipeline

### Excel to KV to JSON
1. Create Excel files in `excels/` directory
2. Sheets starting with `__` or `Sheet[1-3]` are ignored
3. Row 2 contains key names
4. Use `#Loc{key}` for localization strings
5. Run `gulp sheet_2_kv` to generate KV files in `game/scripts/npc/`
6. Run `gulp kv_2_js` to generate JSON in both backend and frontend json directories

### Localization Workflow
1. Excel `#Loc{key}` → `game/resource/kv_generated.csv`
2. `gulp csv_to_localization` → `game/resource/addon_*.txt`
3. Access in code: `$.Localize('#key')` (Panorama) or `LocalizeString('#key')` (Lua)

## Panorama UI

### React Patterns
- Use `react-panorama-x` library for Panorama integration
- XML layout files import TSX: `<include src="./script.tsx" />`
- Access shared types via symlink at `content/panorama/src/shared/`

### Performance
- Use `hittest="false"` on non-interactive panels
- Use `React.memo`, `useCallback`, `useMemo` to prevent unnecessary re-renders
- Minimize deep panel nesting

### LESS/CSS
- Valve uses custom CSS properties (e.g., `flow-children`, `horizontal-align`)
- Keyframes require quoted names: `@keyframes 'animation-name'`
- Use `s2r://` protocol for asset references

## Production Build

### Encryption Configuration
Edit `scripts/addon.config.ts`:
- `addon_name` - Must match dota_addons folder name (lowercase, underscores only)
- `encrypt_files` - Glob patterns for which files to encrypt
- `exclude_files` - Files not copied to publish folder
- **Important**: Don't encrypt modifiers/abilities that need to run on client

### Getting Encryption Keys
1. Upload unencrypted version to Steam Workshop
2. Use console command `get_key_v3 [version]` in-game
3. Fill keys in `scripts/addon.config.ts`
4. Run `yarn prod` with encryption enabled

## Common Pitfalls

### Symlink Issues
- `game/` and `content/` are symlinks created by `yarn install`
- If installation fails, run as administrator or check dota_addons folder permissions

### KV File Access in Panorama
- **Never** use `$.LoadKeyValues()` - Panorama cannot read KV files
- **Always** use the generated JSON files from `gulp kv_2_js`

### Image Precaching
- Run `yarn imgcache` to generate image precache CSS
- Include in XML: `<include src="s2r://panorama/images/image_precache0.css" />`

### Modifier Client Execution
- Modifiers with visual effects must be accessible to clients
- Exclude modifiers folder from encryption in `scripts/addon.config.ts`

### Event Subscription Leaks
- Always use `registerEvent()` in Components for auto-cleanup
- Or manually save subscription IDs and unsubscribe in cleanup

### Hot Reload
- Add `@reloadable` decorator to classes that should hot-reload
- Keep `yarn dev` running for watch mode
- Check vconsole2 for Lua syntax errors

## Configuration Files

### `scripts/addon.config.ts`
Project name and encryption settings.

### TypeScript Compilation
- **VScripts**: `game/scripts/tsconfig.json` - TSTL with normalized enums
- **Panorama**: `content/panorama/tsconfig.json` - Standard React
- **Scripts**: Root `tsconfig.json` - Node.js build scripts

### Webpack
- `content/panorama/webpack.dev.js` - Development build
- `content/panorama/webpack.prod.js` - Production build

## Debugging

### Launch Options
```bash
yarn launch map_name  # Launch with specific map
yarn launch           # Tools mode only
yarn launch do        # Play regular Dota 2
```

### Debug Tools
- Console command system: `game/scripts/src/system/Debug.ts`
- `vconsole2.exe` launches automatically for Lua debugging
- Use `DebugPrint()` for server logs
- Use `console.log()` for Panorama logs

## Additional Project Context

### Project Name
The addon name is configured in `scripts/addon.config.ts` as `sword_gate_defense`. This must match the folder name in dota_addons and follow the pattern: lowercase letters, numbers, and underscores only.

### State Machine
Game uses a state machine pattern in `game/scripts/src/game/state/`:
- States: Init → PreGame → InProgress → Settlement → End
- Each state implements `ICustomState` with `onEnter()` and `onExit()` methods
- Current state: `GameRules.Gameplay.gameState.getState()`

### Documentation Files in Repository
- `README.md` - Chinese documentation
- `README.EN.MD` - English documentation
- `README.RU.MD` - Russian documentation
- `.github/copilot-instructions.md` - Detailed technical documentation (most comprehensive)
- Various `.md` files in root - Implementation notes for specific features

### External Resources
- **Panorama UI Docs**: https://developer.valvesoftware.com/wiki/Dota_2_Workshop_Tools/Panorama
- **TypeScript to Lua**: https://typescripttolua.github.io/
- **ModDota KV Reference**: https://moddota.com/abilities/item-keyvalues/
- **RobinCode (Chinese)**: https://robincode.cn/
