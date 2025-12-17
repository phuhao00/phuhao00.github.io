# WARP.md

This file provides guidance to WARP (warp.dev) when working with code in this repository.

## Project Overview

**sword_gate_defense** is a Dota 2 custom game built with X-Template - a TypeScript-based framework for Dota 2 modding. The project uses a three-layer architecture:

1. **Backend (VScripts)**: TypeScript → Lua via TSTL in `game/scripts/src/`
2. **Frontend (Panorama UI)**: TypeScript + React in `content/panorama/src/`
3. **Data Pipeline**: Excel → KV → JSON for game data

## Development Commands

### Initial Setup
```powershell
# Install dependencies and create symlinks to dota_addons folder
yarn install

# Configure project name (if not already done)
# Edit scripts/addon.config.ts and set addon_name to your project name
```

### Daily Development
```powershell
# Start development mode (watches and auto-compiles all sources)
# Keep this running during development
yarn dev

# Launch Dota 2 with your map for testing
yarn launch temp          # Replace 'temp' with your map name
yarn launch               # Tools mode only

# Alternative: dev without map launching
yarn devm
```

### Build Components
When `yarn dev` runs, it starts three parallel watchers:
- **Gulp**: Watches Excel→KV→JSON, CSV→localization, images→precache CSS, LESS→CSS
- **Webpack**: Watches Panorama TypeScript/React → JavaScript bundles
- **TSTL**: Watches VScripts TypeScript → Lua

### Production Build
```powershell
# Build for production with encryption options
yarn prod

# The script will prompt for encryption mode:
# - test: Local testing (tools mode)
# - release-test: Test workshop upload
# - release: Production workshop upload
```

### Other Useful Commands
```powershell
# Generate API client from OpenAPI spec
yarn api

# Generate image precache CSS
yarn imgcache

# Manually compile LESS files
yarn cless

# Convert CSV to localization files
yarn csv

# Format code
yarn formatcode
```

## Architecture

### Actor-Component Pattern (ECS-like)

The project uses an Actor-Component architecture similar to ECS:

- **Actor**: Base class for game entities (PlayerCharacter, Enemy, etc.)
- **Component**: Reusable functionality modules that attach to Actors
- **System**: Singleton managers mounted on GameRules

**Example Flow**:
```typescript
// Actor with components
PlayerCharacter
  ├─ PlayerAttrsComp      // Custom attributes
  ├─ AbilityPoolComp      // Available abilities
  └─ AbilityBarComp       // Equipped abilities

// Accessing components
character.getComponent(PlayerAttrsComp).BaseHealthPoint.getValue()
character.Attrs.BaseHealthPoint.getValue()  // Via convenience getter
```

### Singleton Systems

All core systems are singletons on `GameRules`, initialized in `ActivateModules()`:

- `GameRules.EventBus` - Priority-based pub/sub event system
- `GameRules.Gameplay` - Game state machine
- `GameRules.CharacterSystem` - Player character management
- `GameRules.CustomAttrSystem` - Custom attribute system with dependencies
- `GameRules.EnemySystem` - Enemy spawning and management
- `GameRules.XNetTable` - Enhanced NetTable for large data sync

### Entry Points

**Server (VScripts)**:
- `game/scripts/src/addon_game_mode.ts` exports `Activate()` and `Precache()`
- `game/scripts/src/index.ts` contains `ActivateModules()` which initializes all systems

**Client (Panorama)**:
- Each UI panel: `layout.xml` → references `.tsx` script
- Example: `content/panorama/src/hud/layout.xml` → `script.tsx`

### Event System

**EventBus (Server-Side)** - Type-safe pub/sub with priority:
```typescript
// Subscribe with priority
GameRules.EventBus.subscribe('GameStartEvent', data => {
  // Handle event
}, EventOrder.High);

// Publish
GameRules.EventBus.publish('GameStartEvent', { diff: 1 });

// One-time subscription (returns true to auto-unsubscribe)
GameRules.EventBus.subscribe('GameStartEvent', data => {
  console.log('Only once');
  return true;
});
```

**CustomGameEvents** - Client-server communication:
```typescript
// Client → Server
GameEvents.SendCustomGameEventToServer('c2s_test_event', { key: 'F' });

// Server: Listen
CustomGameEventManager.RegisterListener('c2s_test_event', (_, data) => {
  print(data.key);
});
```

### Custom Attributes System

Attributes use a dependency calculation chain:
- **Independent Attributes**: Direct values (BaseHealthPoint, BaseAttack)
- **Dependency Attributes**: Calculated from other attributes (HealthPointFinal)

```typescript
// Dependency attribute auto-recalculates when dependencies change
class HealthPointFinal extends CustomAttr_Dependency {
  protected calculateValue(): void {
    const char = this.owner.Attrs;
    this.resultValue = 
      char.BaseHealthPoint.getValue() * 
      (1 + char.HealthPointAddPer.getValue() * 0.01) * 
      char.HealthPointMulPer.getValue();
  }
}
```

### Game State Machine

States in `game/scripts/src/game/state/`:
- `InitState` - Game initialization
- `PreGameState` - Before game starts
- `InProgressState` - Active gameplay
- `SettlementState` - End of round
- `EndState` - Game over

Each state implements:
- `onEnter()` - Setup when entering state
- `onExit()` - Cleanup when leaving state

### XNetTable System

Enhanced network table that bypasses Valve's 2MB limit using chunked transmission:

```typescript
// Server: Set value for all clients
GameRules.XNetTable.SetTableValue('game', 'gameState', GameStateType.InProgress);

// Server: Set player-specific value
GameRules.XNetTable.SetPlayerTableValue(playerId, 'character', 'choices', data);

// Client: Subscribe (React hook)
const gameState = useXNetTableKey('game', 'gameState');
```

**Note**: For small, high-frequency data use native `CustomNetTables` to avoid frame time impact.

### Devour Condition System

9 condition types for ability evolution:
1. `COUNT_KILL_MONSTER` - Kill N monsters
2. `COUNT_SECOND` - Hold for N seconds (0 = instant)
3. `SELECT_UNCONSUMED` - Have specific ability (not devoured)
4. `SELECT_CONSUMED` - Have specific ability (devoured)
5. `COST_GOLD` - Spend N gold
6. `COST_WOOD` - Spend N wood
7. `COST_MERIT` - Spend N merit
8. `ANY_SELECT_UNCONSUMED_ANY_AUTOPHAGY` - Have any + devoured any from list
9. `ALL_ALLIANCE` - Complete alliance (have all abilities with same AllianceTag)

Configured in KV via `DevourConditions` field. See `DEVOUR_CONDITION_EXAMPLES.md` for details.

## Directory Structure

### Backend (`game/scripts/src/`)
```
actor/              # Game entities (Actor base class, PlayerCharacter, Enemy)
component/          # Reusable components (PlayerAttrsComp, AbilityBarComp)
  ├─ ability/       # Ability-related components
  └─ ...
system/             # Singleton systems (mounted on GameRules)
  ├─ character/     # CharacterSystem
  ├─ custom_attr/   # CustomAttrSystem, attribute classes
  └─ Debug.ts       # Console command system
event_bus/          # EventBus and EventList definitions
game/               # Core game logic
  ├─ state/         # State machine states
  └─ mode/          # Game modes
abilities/          # Custom abilities
modifiers/          # Custom modifiers (DO NOT encrypt - need client execution)
shared/             # Frontend-backend shared types
  ├─ gameevents.d.ts    # CustomGameEvent declarations
  └─ x-net-table.d.ts   # XNetTable type definitions
utils/              # Utility functions
config/             # KV data access
global/             # Global declarations
index.ts            # ActivateModules() entry point
addon_game_mode.ts  # Main Dota 2 entry point
```

### Frontend (`content/panorama/src/`)
```
view/               # Main UI screens (hud, loading, end_screen)
agent/              # UI agents/controllers
mgr/                # UI managers
hooks/              # React custom hooks (useXNetTable, useKeyboard)
utils/              # Utility libraries
  ├─ event-bus.ts       # Local UI event system
  ├─ keybinding.ts      # Keyboard handling
  └─ ...
def/                # Type definitions
shared/             # Symlink to game/scripts/src/shared/
json/               # Auto-generated JSON from KV files
styles/             # Global styles
precache/           # Auto-generated image precache CSS
```

### Data Pipeline
```
excels/             # Excel source files (*.xlsx)
  ↓ (gulp sheet_2_kv)
game/scripts/npc/   # KV files (abilities, items, units)
  ↓ (gulp kv_2_js)
game/scripts/src/json/         # JSON for VScripts
content/panorama/src/json/     # JSON for Panorama
```

## Code Conventions

### Always Use Uber Style (Go)
When writing Go code, **always use [Uber Go Style Guide](https://github.com/uber-go/guide/blob/master/style.md)**. This is a project rule.

### TypeScript Naming (Server-Side VScripts)
- **Classes**: `PascalCase` (PlayerCharacter, GameEventManager)
- **Interfaces**: `PascalCase` with optional `I` prefix (ICustomState, C_ATTR_DATA)
- **Enums**: `PascalCase`, members also `PascalCase` (GameStateType.InProgress)
- **Constants**: `UPPER_SNAKE_CASE` (global) or `camelCase` (local)
- **Functions/Methods**: `camelCase` (calculateDamage, getValue)
- **Private Members**: `_` prefix (_eventMap, _insertDataToQueue)
- **Protected Members**: no prefix (owner, calculateValue)

### TypeScript Naming (Client-Side Panorama)
- **React Components**: `PascalCase` (BlessAbilityUI, FlameGraph)
- **Props Interfaces**: `ComponentNameProps` (DraggableWindowProps)
- **Hooks**: `use` prefix + `camelCase` (useXNetTableKey, useLocalEvent)
- **Utility Functions**: `camelCase` (emitLocalEvent, registerCustomKey)

### Component Lifecycle
```typescript
export class BaseComponent<T extends Actor> {
  // 1. Constructor - initialize internal state
  constructor() { }
  
  // 2. onAttach - called when attached to Actor
  onAttach(owner: T): void {
    this.owner = owner;
    // Register events here
    this.registerEvent('GameStartEvent', this.onGameStart.bind(this));
  }
  
  // 3. Business logic methods
  private onGameStart(data: EventList['GameStartEvent']): void { }
  
  // 4. onDetach - cleanup when detached
  onDetach(): void {
    // Auto-cleanup all registered events
    this.eventRecord.forEach((name, id) => 
      GameRules.EventBus.unsubscribeByID(name, id)
    );
  }
}
```

### Memory Management
Always implement cleanup in:
- **Actor.onDestroy()**: Clean all components, remove references
- **Component.onDetach()**: Unregister events, remove timers
- **CustomAttr.destroyAttr()**: Remove timers, unsubscribe events

### Hot Reload
Use `@reloadable` decorator for classes to support hot reload during development:
```typescript
import { reloadable } from '../utils/tstl-utils';

@reloadable
export class GameEventManager {
  // Code changes auto-reload without restarting game
}
```

## Data Pipeline Details

### Excel → KV Conversion
1. Place Excel files in `excels/` folder
2. Sheet names become KV sections (ignore `__*` or `Sheet[1-3]`)
3. Row 2 contains key names
4. Cells with `#Loc{key}` generate localization entries in `game/resource/kv_generated.csv`

### KV → JSON Sync
Auto-generated JSON appears in:
- `game/scripts/src/json/` (for VScripts)
- `content/panorama/src/json/` (for Panorama)

Access in TypeScript: `import data from '../json/abilities.json'`

### Localization Workflow
1. `#Loc{key}` in Excel → `game/resource/kv_generated.csv`
2. Run `gulp csv_to_localization` → `game/resource/addon_*.txt`
3. Access: `$.Localize('#key')` (Panorama) or `LocalizeString('#key')` (Lua)

## Configuration Files

### `scripts/addon.config.ts`
- `addon_name`: Must match dota_addons folder name (lowercase, underscores only)
- `encrypt_files`: Patterns for production encryption
  - **Critical**: DO NOT encrypt abilities/modifiers (need client execution)
- `exclude_files`: Files not copied to publish folder
- `encryptDedicatedServerKey*`: Encryption keys
  - Get via `get_key_v3 [version]` console command after first upload

### TypeScript Configurations
- **VScripts**: `game/scripts/tsconfig.json` - TSTL config
- **Panorama**: `content/panorama/tsconfig.json` - React config
- **Scripts**: Root `tsconfig.json` - Node.js build scripts

## Testing & Debugging

### Debug Commands
- Defined in `game/scripts/src/system/Debug.ts`
- `get_key_v3 [version]` - Get encryption keys after workshop upload
- Custom console commands can be added here

### Debug Output
- **Server**: Use `DebugPrint()` for console output
- **Client**: Use `console.log()` in Panorama
- **vconsole2.exe** launches automatically for Lua debugging

### Flame Graph Profiler
Performance analysis tool available. See `game/scripts/src/utils/performance/flame_graph_profiler.md`.

## Common Pitfalls

### 1. Symlink Creation
If `yarn install` fails with symlink errors:
- Run PowerShell/CMD as Administrator
- Check dota_addons folder permissions
- Delete existing game/content folders and retry

### 2. Encryption Keys
Cannot encrypt for production? 
1. Upload unencrypted to workshop first
2. Run `get_key_v3 1` in game console (1 = version)
3. Add keys to `scripts/addon.config.ts`
4. Run `yarn prod` with encryption

### 3. KV File Access in Panorama
- ❌ Don't use `$.LoadKeyValues()` - unreliable
- ✅ Use auto-generated JSON from `gulp kv_2_js`
- ✅ Import: `import data from '../json/heroes.json'`

### 4. Image Precaching
Images showing as "?" in UI?
```powershell
# Generate image precache CSS
yarn imgcache
```
Then include in XML:
```xml
<include src="s2r://panorama/images/image_precache0.css" />
```

### 5. Event Subscription Leaks
Always clean up event subscriptions:
```typescript
// ✅ In Component: use registerEvent (auto-cleanup)
this.registerEvent('SomeEvent', this.handleEvent.bind(this));

// ✅ Manual: save ID and cleanup
const id = GameRules.EventBus.subscribe('Event', data => {});
// Later: GameRules.EventBus.unsubscribeByID('Event', id);
```

### 6. XNetTable vs CustomNetTables
**Use XNetTable when**:
- Large data (>100KB)
- Low update frequency (<1/sec)

**Use CustomNetTables when**:
- Small data (<10KB)
- High frequency updates (multiple/sec)

## TypeScript-to-Lua (TSTL) Constraints

### Avoid These
- ❌ `async/await` - use callbacks
- ❌ Destructuring in function parameters - destructure in body instead
- ❌ `for...of` loops - use `pairs()` or `ipairs()`

```typescript
// ❌ Wrong
async function loadData() { await fetchData(); }
function process({ x, y }: Point) { }
for (const item of list) { }

// ✅ Right
function loadData(callback: (data: any) => void) { fetchData(callback); }
function process(point: Point) { const { x, y } = point; }
for (const [_, item] of pairs(list)) { }
```

### Decorators
- `@reloadable` - Hot-reload support
- `@registerModifier()` - Register Dota 2 modifier

## Panorama UI Best Practices

### Performance
- Set `hittest="false"` on non-interactive panels
- Use `React.memo`, `useCallback`, `useMemo` to prevent re-renders
- Cache panel references vs repeated `$()` calls

### CSS/LESS
- Valve uses custom properties: `flow-children`, `horizontal-align`
- Keyframes need quoted names: `@keyframes 'my-animation'`
- Asset references: `s2r://` protocol

### React Patterns
```typescript
// Import render from react-panorama-x
import { render } from 'react-panorama-x';
render(<MyComponent />, $.GetContextPanel());

// Use hooks for game events
useGameEvent('dota_player_killed', (event) => { });

// Use hooks for NetTables
const value = useNetTableKey('table', 'key');
```

## Shared Type Definitions

Types shared between client/server live in `game/scripts/src/shared/`:
- `gameevents.d.ts` - CustomGameEvent declarations
- `x-net-table.d.ts` - XNetTable type definitions
- `game/export.ts` - Shared enums (GameStateType, GameModeType)

Panorama accesses via symlink: `content/panorama/src/shared`

**Custom game events must be declared**:
```typescript
declare interface CustomGameEventDeclarations {
  c2s_test_event: { key: string };
}
```

## Project-Specific Features

### Devour Condition System
Complete implementation with 9 condition types for ability evolution. Key files:
- `DEVOUR_CONDITION_COMPLETE.md` - Full implementation summary
- `DEVOUR_CONDITION_EXAMPLES.md` - Configuration examples
- `DEVOUR_CONDITION_IMPLEMENTATION.md` - Architecture details

### Alliance System
Abilities can have `AllianceTag` for synergy bonuses. When all abilities with same tag are collected, special effects trigger.

## Additional Resources

- **Panorama UI**: https://developer.valvesoftware.com/wiki/Dota_2_Workshop_Tools/Panorama
- **TypeScript to Lua**: https://typescripttolua.github.io/
- **ModDota Reference**: https://moddota.com/abilities/item-keyvalues/
- **Chinese Resources**: https://robincode.cn/
