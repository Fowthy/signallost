# Signal Lost - Lethal Company-like Web Game

## Project Overview

**Signal Lost** is a 3D multiplayer cooperative horror survival web game inspired by Lethal Company. Players work together to collect valuable scrap from procedurally generated facilities while avoiding monsters, all under time pressure to meet quotas.

### Tech Stack (Finalized)

| Layer | Technology | Reason |
|-------|------------|--------|
| **Frontend Framework** | Next.js 14 (App Router) | Best React framework, SSR/SSG support |
| **3D Engine** | Three.js + React Three Fiber | Best React integration, huge ecosystem |
| **3D Helpers** | @react-three/drei | Useful abstractions (controls, loaders) |
| **Physics** | @react-three/rapier | Rust-based, extremely fast, WASM |
| **State Management** | Zustand | Lightweight, perfect for games |
| **Game Server** | Colyseus | Purpose-built for multiplayer games |
| **Voice Chat** | Simple-peer (WebRTC) | P2P voice, no server bandwidth |
| **Audio** | Howler.js + Web Audio API | Spatial audio, pooling |
| **Database** | SQLite (optional) | Simple persistence if needed |
| **Package Manager** | pnpm | Fast, disk efficient |
| **Monorepo** | Turborepo | Efficient builds, caching |

### Architecture

```
signallost/
├── apps/
│   ├── web/                    # Next.js frontend (game client)
│   │   ├── app/                # App router pages
│   │   ├── components/         # React components
│   │   │   ├── game/          # 3D game components (R3F)
│   │   │   ├── ui/            # UI components (menus, HUD)
│   │   │   └── lobby/         # Lobby components
│   │   ├── hooks/             # Custom React hooks
│   │   ├── lib/               # Core libraries
│   │   │   ├── game/          # Game logic (client-side)
│   │   │   ├── network/       # Colyseus client
│   │   │   ├── audio/         # Audio system
│   │   │   └── voice/         # WebRTC voice chat
│   │   ├── stores/            # Zustand stores
│   │   └── public/
│   │       └── assets/        # 3D models, textures, sounds
│   │
│   └── server/                 # Colyseus game server
│       ├── src/
│       │   ├── rooms/         # Game room definitions
│       │   ├── schemas/       # Colyseus state schemas
│       │   ├── systems/       # Game systems (AI, spawning)
│       │   ├── generation/    # Procedural generation
│       │   └── utils/         # Server utilities
│       └── package.json
│
├── packages/
│   └── shared/                 # Shared code (types, constants)
│       ├── types/             # TypeScript interfaces
│       ├── constants/         # Game constants
│       └── utils/             # Shared utilities
│
├── scripts/
│   └── start.sh               # Single command startup
├── turbo.json                 # Turborepo config
├── pnpm-workspace.yaml        # Workspace config
└── package.json               # Root package.json
```

---

## Phase 1: Project Foundation

### 1.1 Project Setup
- [ ] Initialize pnpm workspace with Turborepo
- [ ] Create `apps/web` - Next.js 14 with TypeScript
- [ ] Create `apps/server` - Colyseus game server
- [ ] Create `packages/shared` - Shared types and constants
- [ ] Configure TypeScript paths and references
- [ ] Set up ESLint and Prettier
- [ ] Create environment variable structure (.env.example)
- [ ] Set up Git hooks (husky) for linting

### 1.2 Development Infrastructure
- [ ] Create `scripts/start.sh` - Single command to start everything
- [ ] Create `scripts/start.bat` - Windows version
- [ ] Set up hot reloading for both web and server
- [ ] Configure concurrent development servers
- [ ] Add environment detection (dev/prod)
- [ ] Create Docker Compose for optional containerization
- [ ] Document Cloudflare tunnel setup instructions

### 1.3 Shared Package Setup
- [ ] Define core game types (Player, Monster, Item, etc.)
- [ ] Define network message types
- [ ] Define game constants (speeds, distances, timings)
- [ ] Create shared utility functions
- [ ] Set up package exports

---

## Phase 2: Game Server (Colyseus)

### 2.1 Server Foundation
- [ ] Set up Express + Colyseus server
- [ ] Configure CORS for local development
- [ ] Set up server-side logging
- [ ] Create health check endpoint
- [ ] Implement graceful shutdown handling

### 2.2 Room System
- [ ] Create `LobbyRoom` for matchmaking/server browser
- [ ] Create `GameRoom` for actual gameplay
- [ ] Implement room creation with custom settings
- [ ] Implement room joining with username
- [ ] Add room capacity limits (max 6 players)
- [ ] Implement room cleanup on empty
- [ ] Add room state persistence (optional)

### 2.3 State Schemas (Colyseus)
- [ ] `GameState` - Root state schema
- [ ] `PlayerState` - Position, rotation, health, inventory
- [ ] `MonsterState` - Position, AI state, target
- [ ] `ItemState` - Position, type, value, picked up status
- [ ] `MapState` - Seed, generated areas, doors
- [ ] `TimeState` - Current time, day phase, quota deadline
- [ ] `ShipState` - Position, door status, landed status

### 2.4 Server Game Loop
- [ ] Implement fixed tick rate (20 ticks/second)
- [ ] Player input processing
- [ ] Monster AI updates
- [ ] Physics simulation (server authoritative)
- [ ] Item spawn management
- [ ] Time progression
- [ ] Death/respawn handling
- [ ] Quota tracking

### 2.5 Server Commands/Messages
- [ ] Player movement input
- [ ] Player interaction (pickup, use, drop)
- [ ] Player voice state (talking, walkie-talkie)
- [ ] Chat messages
- [ ] Game actions (start round, return to ship)
- [ ] Admin commands (kick, restart)

---

## Phase 3: Economy & Progression System

### 3.1 Credits System
- [ ] Credits as main currency
- [ ] Starting credits (configurable)
- [ ] Credits earned from selling scrap
- [ ] Credits shared between all players (team pool)
- [ ] Credits persist across rounds (until game over)
- [ ] Credits display in UI

### 3.2 Shop/Store System
- [ ] Shop terminal on ship
- [ ] Item catalog with prices
- [ ] Purchase confirmation
- [ ] Item delivery system (items appear on ship)
- [ ] Limited stock (optional)
- [ ] Shop categories (tools, ship upgrades, suits)

### 3.3 Purchasable Items
- [ ] **Tools**
  - [ ] Flashlight - $15
  - [ ] Pro-flashlight - $25
  - [ ] Walkie-talkie - $12
  - [ ] Shovel - $30
  - [ ] Stun grenade - $40
  - [ ] Zap gun - $400
  - [ ] Boombox - $60
  - [ ] TZP-Inhalant (speed boost) - $120
  - [ ] Radar-booster - $50
  - [ ] Spray paint - $50
  - [ ] Extension ladder - $60
  - [ ] Lockpicker - $20
  - [ ] Jetpack - $700
- [ ] **Ship Upgrades**
  - [ ] Teleporter - $375
  - [ ] Inverse Teleporter - $425
  - [ ] Loud horn - $100
  - [ ] Signal translator - $255
  - [ ] Ship lights upgrade - $50

### 3.4 Moon/Map Selection
- [ ] Multiple moons with different difficulties
- [ ] Moon selection terminal
- [ ] Travel cost per moon
- [ ] Moon information display (weather, difficulty, loot multiplier)
- [ ] **Starter Moon** - Free, easy, low loot
- [ ] **Medium Moons** - $50-100, moderate danger
- [ ] **Hard Moons** - $200+, high danger, high reward
- [ ] Moon-specific monster spawns
- [ ] Moon-specific layouts

---

## Phase 4: Procedural Map Generation

### 4.1 Generation Algorithm
- [ ] Implement seeded random number generator
- [ ] Create Binary Space Partition (BSP) room generator
- [ ] Create corridor generation between rooms
- [ ] Implement room type assignment (loot room, monster den, etc.)
- [ ] Add entrance/exit placement
- [ ] Generate navmesh for AI pathfinding

### 4.2 Map Features
- [ ] Define room prefab types (small, medium, large, special)
- [ ] Implement door placement and state
- [ ] Add vent/alternate path generation
- [ ] Create outside terrain generation (simple)
- [ ] Implement ship landing zone
- [ ] **Fire exits** - Multiple facility entrances/exits
- [ ] **Main entrance** - Large, obvious entry point
- [ ] **Emergency exits** - Side doors, harder to find
- [ ] **Ladders** - Vertical navigation between floors
- [ ] **Catwalks** - Elevated walkways

### 4.3 Facility Hazards
- [ ] **Landmines** - Proximity triggered explosives
  - [ ] Visual indicator (can be spotted)
  - [ ] Beeping sound when near
  - [ ] Lethal damage on trigger
  - [ ] Can be disarmed (optional)
- [ ] **Turrets** - Auto-targeting defense systems
  - [ ] Detection cone
  - [ ] Charging sound before firing
  - [ ] High damage, can kill quickly
  - [ ] Can be disabled temporarily (stun)
  - [ ] Safe zones (behind cover)
- [ ] **Steam vents** - Periodic damage zones
- [ ] **Broken floors** - Fall hazards
- [ ] **Locked doors** - Require keys or lockpicker
- [ ] **Powered doors** - Require facility power

### 4.4 Chunk System
- [ ] Implement chunk-based map loading
- [ ] Create chunk visibility system
- [ ] Optimize memory with chunk pooling
- [ ] Implement chunk serialization for network sync

### 4.5 Map Decoration
- [ ] Procedural prop placement
- [ ] Lighting placement
- [ ] Scrap/item spawn point generation
- [ ] Monster spawn point generation

---

## Phase 5: Weather System

### 5.1 Weather Types
- [ ] **Clear** - Normal visibility, baseline difficulty
- [ ] **Foggy** - Reduced visibility outside, spooky atmosphere
- [ ] **Rainy** - Wet sounds, puddles, slightly reduced visibility
- [ ] **Stormy** - Lightning, thunder, very dangerous outside
- [ ] **Flooded** - Water levels risen, some areas inaccessible
- [ ] **Eclipsed** - Darkness during "day", all monsters active

### 5.2 Weather Effects
- [ ] Visual effects (rain particles, fog shader, lightning flashes)
- [ ] Audio effects (rain sounds, thunder, wind)
- [ ] Gameplay effects per weather type
- [ ] Weather affects outside monster spawns
- [ ] Weather display on moon selection
- [ ] Weather changes over time (optional)

### 5.3 Weather-Specific Mechanics
- [ ] Lightning strikes (can kill players outside)
- [ ] Flooding affects movement speed
- [ ] Eclipse triggers night-time monster behavior
- [ ] Fog reduces monster detection range too

---

## Phase 6: Client Core Systems

### 4.1 Network Client
- [ ] Set up Colyseus client connection
- [ ] Implement reconnection logic
- [ ] Create state synchronization handlers
- [ ] Implement client-side prediction
- [ ] Add server reconciliation
- [ ] Create network latency indicator
- [ ] Handle disconnection gracefully

### 4.2 Game State Management (Zustand)
- [ ] `useGameStore` - Core game state
- [ ] `usePlayerStore` - Local player state
- [ ] `useNetworkStore` - Connection state
- [ ] `useAudioStore` - Audio settings
- [ ] `useSettingsStore` - User preferences
- [ ] `useInventoryStore` - Inventory state
- [ ] `useVoiceStore` - Voice chat state

### 4.3 Input System
- [ ] Keyboard input handling (WASD, Space, Shift, E, Q, Tab)
- [ ] Mouse input handling (look, click)
- [ ] Pointer lock implementation
- [ ] Input mapping configuration
- [ ] Gamepad support (optional)
- [ ] Mobile touch controls (optional)

---

## Phase 5: 3D Rendering (React Three Fiber)

### 5.1 Scene Setup
- [ ] Create main game scene component
- [ ] Set up camera system (first-person)
- [ ] Configure renderer settings (shadows, antialiasing)
- [ ] Implement render loop optimization
- [ ] Add post-processing effects (optional: film grain, vignette)

### 5.2 Player Rendering
- [ ] First-person view (arms, held item)
- [ ] Other players' character models
- [ ] Player name tags
- [ ] Player animations (walk, run, idle, use item)
- [ ] Flashlight cone rendering

### 5.3 Environment Rendering
- [ ] Facility interior meshes
- [ ] Outside terrain rendering
- [ ] Ship model and interior
- [ ] Door animations
- [ ] Prop rendering (furniture, debris, etc.)

### 5.4 Lighting System
- [ ] Dynamic day/night lighting (outside)
- [ ] Facility interior lighting (flickering, broken lights)
- [ ] Flashlight dynamic light
- [ ] Emergency lighting
- [ ] Baked lightmaps for performance

### 5.5 Effects
- [ ] Fog (distance + facility fog)
- [ ] Particle effects (dust, sparks)
- [ ] Screen effects (damage, low stamina)

---

## Phase 6: Player Controller

### 6.1 First-Person Controller
- [ ] WASD movement
- [ ] Mouse look (pitch/yaw limits)
- [ ] Sprint (Shift) with stamina drain
- [ ] Crouch (Ctrl) with speed reduction
- [ ] Jump (Space) with cooldown
- [ ] Footstep sounds based on surface
- [ ] Head bobbing (subtle)

### 6.2 Player Physics
- [ ] Collision detection (walls, objects)
- [ ] Gravity and ground detection
- [ ] Slope handling
- [ ] Step climbing (small obstacles)
- [ ] Push physics (moveable objects)

### 6.3 Player Stats
- [ ] Health system (100 HP)
- [ ] Stamina system (sprint resource)
- [ ] Stamina regeneration
- [ ] Encumbrance (inventory weight affects speed)
- [ ] Death state and ragdoll

### 6.4 Interactions
- [ ] Raycast interaction system
- [ ] Interaction UI prompts
- [ ] Item pickup (E key)
- [ ] Item drop (G key)
- [ ] Door opening
- [ ] Ship controls
- [ ] Terminal usage
- [ ] **Item Scanning** (RMB or dedicated key)
  - [ ] Scan animation/effect
  - [ ] Shows item name and value
  - [ ] Scan range limit
  - [ ] Can scan through walls (short range)
  - [ ] Scan sound effect

### 6.5 Death & Body System
- [ ] Player death state
- [ ] Death ragdoll physics
- [ ] **Body persistence** - Dead bodies remain in world
- [ ] **Body recovery** - Living players can pick up bodies
- [ ] Body as two-handed item (heavy)
- [ ] Bring body to ship for... respect? (optional mechanic)
- [ ] Body despawn after round end
- [ ] Death camera (brief view of killer)
- [ ] Death notification to team

---

## Phase 7: Inventory System

### 7.1 Inventory Structure
- [ ] Inventory slots (4 main slots like Lethal Company)
- [ ] Currently held item tracking
- [ ] Item switching (scroll wheel, number keys)
- [ ] Two-handed item handling

### 7.2 Item Types
- [ ] **Scrap Items** - Collectible value items
  - [ ] Small scrap (one-handed)
  - [ ] Large scrap (two-handed)
  - [ ] Various scrap models and values
- [ ] **Tools**
  - [ ] Flashlight (toggleable, battery drain)
  - [ ] Walkie-talkie (voice range extension)
  - [ ] Pro-flashlight (brighter, more battery)
  - [ ] Shovel (weapon, can stun monsters)
  - [ ] Key (for locked doors)
- [ ] **Ship Items**
  - [ ] Terminal
  - [ ] Cupboard (item storage)

### 7.3 Item Mechanics
- [ ] Item weight system
- [ ] Battery system for electronic items
- [ ] Item durability (optional)
- [ ] Item selling at ship
- [ ] Item value randomization

### 7.4 Inventory UI
- [ ] Hotbar display
- [ ] Selected item highlight
- [ ] Item tooltips
- [ ] Weight indicator
- [ ] Battery indicator

---

## Phase 8: Monster/Enemy System

### 8.1 AI Foundation
- [ ] Behavior tree implementation
- [ ] State machine for AI states
- [ ] Pathfinding (A* or nav mesh)
- [ ] Vision cone detection
- [ ] Sound-based detection
- [ ] Interest/alert system

### 8.2 AI States
- [ ] Idle/Patrol - Wander predetermined paths
- [ ] Investigate - Check last known sound/sight
- [ ] Chase - Pursue detected player
- [ ] Attack - Deal damage to player
- [ ] Search - Look for lost target
- [ ] Return - Go back to patrol

### 8.3 Monster Types
- [ ] **Crawler** - Fast, low to ground, hunts by sound
  - Stats: Speed high, damage medium, health low
  - Behavior: Patrols vents, attracted to noise
- [ ] **Stalker** - Slow, watches from distance, ambushes
  - Stats: Speed low, damage high, health medium
  - Behavior: Follows at distance, attacks when alone
- [ ] **Screamer** - Alerts other monsters, moderate threat
  - Stats: Speed medium, damage low, health low
  - Behavior: Screams when sees player, summons others
- [ ] **Brute** - Tank, slow but deadly
  - Stats: Speed very low, damage very high, health very high
  - Behavior: Guards high-value loot areas

### 8.4 Outside Monsters (Different from inside)
- [ ] **Giant** - Massive, patrols outside, instant kill
  - [ ] Huge model, visible from far
  - [ ] Slow movement, predictable patrol
  - [ ] Eats players it catches
  - [ ] Can be avoided by hiding
- [ ] **Worm** - Underground, ambush predator
  - [ ] Burrows underground
  - [ ] Emerges to attack
  - [ ] Triggered by surface movement
  - [ ] Very rare spawn
- [ ] **Dogs** - Pack hunters, blind but hear well
  - [ ] Hunt in groups
  - [ ] Completely blind
  - [ ] Attracted to any sound
  - [ ] Fast and deadly
- [ ] **Birds** - Flying scouts
  - [ ] Alert other monsters
  - [ ] Fly away when approached
  - [ ] Passive unless provoked

### 8.5 Monster Spawning
- [ ] Spawn point system in generated maps
- [ ] Difficulty scaling (time-based)
- [ ] Max monster limits
- [ ] Respawn mechanics
- [ ] Outside vs inside monster separation
- [ ] Time-based spawn increases
- [ ] Moon difficulty affects spawn rates

### 8.6 Monster Mechanics
- [ ] **Grab/Drag** - Some monsters grab players
  - [ ] Grabbed state (can't move)
  - [ ] Teammates can save (hit monster)
  - [ ] Drag player to kill zone
- [ ] **Stun vulnerability** - Monsters can be stunned
  - [ ] Shovel stun
  - [ ] Stun grenade effect
  - [ ] Zap gun continuous stun
  - [ ] Stun duration varies by monster

### 8.7 Monster Models & Animation
- [ ] Find/create monster models (Mixamo, Sketchfab)
- [ ] Walk/run animations
- [ ] Attack animations
- [ ] Death animations
- [ ] Idle animations
- [ ] Sound effects for each monster

---

## Phase 9: Day/Night & Time System

### 9.1 Time Management
- [ ] Server-authoritative time
- [ ] Configurable day length (real-time minutes)
- [ ] Time phases (Morning, Afternoon, Evening, Night)
- [ ] Time UI display
- [ ] Time sync across clients

### 9.2 Day Phases
- [ ] **Morning (0:00-6:00)** - Safe, monsters dormant
- [ ] **Afternoon (6:00-12:00)** - Moderate danger
- [ ] **Evening (12:00-18:00)** - Increased spawns
- [ ] **Night (18:00-24:00)** - Maximum danger, must be in ship

### 9.3 Visual Changes
- [ ] Sun/moon position
- [ ] Skybox color transitions
- [ ] Dynamic shadows
- [ ] Ambient light changes
- [ ] Inside lighting unaffected

### 9.4 Gameplay Effects
- [ ] Monster spawn rate by time
- [ ] Monster aggression by time
- [ ] Outside danger at night
- [ ] Ship departure deadline

---

## Phase 10: Ship System

### 10.1 Ship Structure
- [ ] Ship 3D model (find or create)
- [ ] Ship interior layout
- [ ] Ship door (open/close)
- [ ] Terminal inside ship
- [ ] Item storage area
- [ ] Player spawn points

### 10.2 Ship Mechanics
- [ ] Ship as safe zone (no monsters)
- [ ] Ship landing/takeoff animations
- [ ] Ship door controls
- [ ] Ship horn (warning)
- [ ] Ship lights

### 10.3 Ship Terminal
- [ ] View current quota
- [ ] View collected scrap value
- [ ] Start departure sequence
- [ ] View crew status
- [ ] View time remaining

### 10.4 Quota System
- [ ] Daily quota target
- [ ] Quota progress tracking
- [ ] End of day calculation
- [ ] Quota failure consequences
- [ ] Quota success rewards
- [ ] Increasing quota difficulty

### 10.5 Ship Upgrades (Purchasable)
- [ ] **Teleporter**
  - [ ] Teleport pad in ship
  - [ ] Select player on radar to teleport
  - [ ] Teleports player back to ship
  - [ ] Drops all items at original location
  - [ ] Cooldown between uses
- [ ] **Inverse Teleporter**
  - [ ] Teleports player INTO facility randomly
  - [ ] Useful for reaching deep areas quickly
  - [ ] Risk: random location may be dangerous
- [ ] **Loud Horn**
  - [ ] Warning horn for ship departure
  - [ ] Can be heard from anywhere on moon
  - [ ] Attracts monsters briefly
- [ ] **Signal Translator**
  - [ ] Shows monster signals on radar
  - [ ] Useful for tracking threats

### 10.6 Ship Radar System
- [ ] Radar screen on terminal
- [ ] Shows facility layout (if scanned)
- [ ] **Player tracking** - Dots for each player
- [ ] Player names on hover
- [ ] Real-time position updates
- [ ] Dead players shown differently
- [ ] Can select player for teleporter
- [ ] Radar range limited to facility

---

## Phase 11: Sound/Noise Mechanics

### 11.1 Noise System
- [ ] Every action has a noise level
- [ ] Noise attracts monsters
- [ ] Noise propagation through rooms
- [ ] Noise visualization (optional debug)

### 11.2 Noise Sources
- [ ] **Walking** - Low noise
- [ ] **Running** - Medium noise
- [ ] **Jumping** - Medium noise
- [ ] **Dropping items** - Varies by item weight
- [ ] **Opening doors** - Low-medium noise
- [ ] **Voice chat** - Attracts monsters!
- [ ] **Walkie-talkie static** - Low noise
- [ ] **Boombox** - High noise (distraction tool)
- [ ] **Shovel hit** - Medium noise
- [ ] **Flashlight click** - Very low noise

### 11.3 Stealth Mechanics
- [ ] Crouching reduces noise significantly
- [ ] Slow walking (walk key) even quieter
- [ ] Surface affects noise (metal louder than carpet)
- [ ] Monsters have noise detection threshold
- [ ] Can hide and wait for monsters to pass

---

## Phase 12: Voice Chat System

### 12.1 WebRTC Setup
- [ ] Simple-peer integration
- [ ] Colyseus signaling (offer/answer exchange)
- [ ] ICE candidate handling
- [ ] Connection state management
- [ ] Automatic reconnection

### 12.2 Audio Processing
- [ ] Microphone capture
- [ ] Noise suppression (optional)
- [ ] Volume normalization
- [ ] Push-to-talk option
- [ ] Voice activity detection

### 12.3 Proximity Voice
- [ ] Distance-based volume falloff
- [ ] Maximum hear distance (15 units default)
- [ ] Obstruction detection (walls muffle)
- [ ] Spatial audio (left/right panning)
- [ ] Dead players can't talk to living

### 12.4 Walkie-Talkie
- [ ] Extended range communication
- [ ] Channel system (everyone on same channel)
- [ ] Static/distortion effect
- [ ] Battery consumption
- [ ] Toggle on/off (T key)
- [ ] Visual indicator when in use

### 12.5 Voice UI
- [ ] Speaking indicator (who's talking)
- [ ] Microphone mute toggle
- [ ] Volume controls
- [ ] Push-to-talk indicator

---

## Phase 13: Text Chat System

### 12.1 Chat Implementation
- [ ] Chat input field
- [ ] Chat message display
- [ ] Message history (scrollable)
- [ ] Send on Enter key
- [ ] Chat toggle (Y key)

### 12.2 Chat Features
- [ ] Player name with message
- [ ] Timestamp display
- [ ] System messages (joins, leaves, deaths)
- [ ] Chat proximity (same as voice, optional)
- [ ] Global chat in lobby

### 12.3 Chat UI
- [ ] Semi-transparent chat window
- [ ] Auto-hide after inactivity
- [ ] Unread message indicator
- [ ] Chat position customization

---

## Phase 14: Audio System

### 13.1 Audio Engine
- [ ] Howler.js setup
- [ ] Audio pooling for performance
- [ ] Spatial audio (Web Audio API)
- [ ] Volume categories (master, music, SFX, voice)
- [ ] Audio loading and caching

### 13.2 Sound Categories
- [ ] **Ambient** - Environment sounds, wind, facility hum
- [ ] **Footsteps** - Surface-based footstep sounds
- [ ] **Interactions** - Pickup, drop, use sounds
- [ ] **Monsters** - Movement, attacks, alerts
- [ ] **UI** - Menu clicks, notifications
- [ ] **Voice** - Player voice chat

### 13.3 3D Audio
- [ ] Distance-based falloff
- [ ] Stereo panning
- [ ] Reverb in enclosed spaces
- [ ] Occlusion (through walls)

### 13.4 Sound Design
- [ ] Find/create horror ambient sounds
- [ ] Footstep sounds (metal, concrete, dirt)
- [ ] Item pickup/drop sounds
- [ ] Monster sounds (footsteps, growls, attacks)
- [ ] Ship sounds (engine, door, horn)
- [ ] UI sounds

---

## Phase 15: User Interface

### 14.1 Main Menu
- [ ] Game logo/title
- [ ] "Join Server" button
- [ ] Username input
- [ ] Server browser
- [ ] Settings button
- [ ] Credits/About

### 14.2 Server Browser
- [ ] List of available servers
- [ ] Server info (players, map, time)
- [ ] Join button
- [ ] Refresh button
- [ ] Direct connect (IP:Port)
- [ ] Create server option

### 14.3 In-Game HUD
- [ ] Health bar
- [ ] Stamina bar
- [ ] Inventory hotbar
- [ ] Current time display
- [ ] Quota progress
- [ ] Compass/direction indicator
- [ ] Interaction prompts

### 14.4 Pause Menu
- [ ] Resume button
- [ ] Settings
- [ ] Leave server
- [ ] Player list

### 14.5 Settings Menu
- [ ] Graphics settings (quality presets)
- [ ] Audio settings (volume sliders)
- [ ] Control settings (sensitivity, keybinds)
- [ ] Voice settings (input device, PTT key)
- [ ] Accessibility options

### 14.6 Death/Spectate UI
- [ ] Death screen
- [ ] Spectate mode
- [ ] Player selection for spectate
- [ ] Respawn timer (if applicable)

### 14.7 End Round Screen
- [ ] Scrap collected summary
- [ ] Quota status
- [ ] Player contributions
- [ ] Continue/Next round button

---

## Phase 16: Game Flow & Round System

### 15.1 Lobby Flow
- [ ] Player joins lobby
- [ ] Player enters username
- [ ] Player selects/creates server
- [ ] Player waits in ship for others
- [ ] Host can start game

### 15.2 Round Structure
- [ ] Round start (land on moon)
- [ ] Exploration phase (collect scrap)
- [ ] Time pressure (day progresses)
- [ ] Return to ship phase
- [ ] Round end (quota check)

### 15.3 Win/Lose Conditions
- [ ] Meet quota = continue to next round
- [ ] Fail quota = game over (restart)
- [ ] All players dead = round failed
- [ ] Ship leaves without players = those players lose items

### 15.4 Progression
- [ ] Increasing quota each round
- [ ] Difficulty scaling
- [ ] New items available (shop - optional)
- [ ] Different moon/map selection (optional)

### 16.5 Host Migration
- [ ] Detect host disconnection
- [ ] Automatic host transfer to next player
- [ ] Seamless game continuation
- [ ] State preservation during migration
- [ ] Notification to all players
- [ ] Fallback if migration fails

### 16.6 Ping/Marker System
- [ ] Ping key (Middle mouse or Z)
- [ ] Visual marker in world
- [ ] Marker visible through walls
- [ ] Marker color per player
- [ ] Marker auto-expire (10 seconds)
- [ ] "Danger" ping variant (double-tap)
- [ ] Ping sound notification
- [ ] Compass shows ping direction

---

## Phase 17: Performance Optimization

### 16.1 Rendering Optimization
- [ ] Level of Detail (LOD) system
- [ ] Frustum culling
- [ ] Occlusion culling
- [ ] Instance rendering for repeated objects
- [ ] Texture compression (Basis Universal)
- [ ] Mesh optimization (draco compression)

### 16.2 Network Optimization
- [ ] Delta compression
- [ ] Interest management (only sync nearby)
- [ ] Update rate optimization
- [ ] Bandwidth monitoring
- [ ] Message batching

### 16.3 Memory Optimization
- [ ] Asset pooling
- [ ] Chunk unloading
- [ ] Texture streaming
- [ ] Garbage collection management

### 16.4 Loading Optimization
- [ ] Asset preloading
- [ ] Loading screen with progress
- [ ] Lazy loading non-critical assets
- [ ] Code splitting

---

## Phase 18: Polish & Quality

### 17.1 Visual Polish
- [ ] Screen shake on damage
- [ ] Camera effects (fear, low health)
- [ ] UI animations
- [ ] Loading animations
- [ ] Smooth transitions

### 17.2 Audio Polish
- [ ] Audio ducking during important events
- [ ] Ambient sound layers
- [ ] Music stingers (danger, discovery)
- [ ] Death sounds
- [ ] Victory sounds

### 17.3 Gameplay Polish
- [ ] Tutorial/onboarding
- [ ] Tooltips and hints
- [ ] Controller vibration (if supported)
- [ ] Accessibility features

### 17.4 Bug Fixing
- [ ] Desync handling
- [ ] Edge case handling
- [ ] Error boundaries
- [ ] Crash recovery

---

## Phase 19: Bestiary & Log System

### 19.1 Bestiary (Monster Log)
- [ ] Unlockable monster entries
- [ ] Monster discovered on first encounter
- [ ] Monster image/silhouette
- [ ] Monster stats (speed, danger level)
- [ ] Monster behavior hints
- [ ] Scan monster to add to bestiary
- [ ] Bestiary accessible from pause menu

### 19.2 Game Log/Journal
- [ ] Mission history
- [ ] Scrap collected per round
- [ ] Deaths per round
- [ ] Moons visited
- [ ] Total credits earned
- [ ] Playtime tracking

### 19.3 Achievements (Optional)
- [ ] First scrap collected
- [ ] Survive first night
- [ ] Meet quota 5 times
- [ ] Discover all monsters
- [ ] Visit all moons
- [ ] Achievement notifications

---

## Phase 20: Assets

### 18.1 3D Models (Priority)
- [ ] Player character model (first-person arms)
- [ ] Player character model (third-person, for others)
- [ ] Ship model (exterior and interior)
- [ ] Facility wall/floor/ceiling tiles
- [ ] Door models
- [ ] Prop models (tables, chairs, debris)
- [ ] Scrap item models (10+ varieties)
- [ ] Tool models (flashlight, walkie-talkie, shovel)
- [ ] Monster models (4 types)

### 18.2 Textures
- [ ] Facility textures (metal, concrete, rust)
- [ ] Outside terrain textures
- [ ] Skybox textures (day/night cycle)
- [ ] UI textures and icons
- [ ] Item icons

### 18.3 Audio Assets
- [ ] Footstep sounds (4+ surface types)
- [ ] Ambient loops (facility, outside, ship)
- [ ] Monster sounds (per monster type)
- [ ] Item sounds (pickup, use, drop)
- [ ] UI sounds
- [ ] Music tracks (menu, tension, danger)

### 18.4 Asset Sources
- [ ] Quaternius (low-poly models) - https://quaternius.com
- [ ] Kenney Assets (props, items) - https://kenney.nl
- [ ] Mixamo (character animations) - https://mixamo.com
- [ ] Poly Haven (textures) - https://polyhaven.com
- [ ] Sketchfab (models, CC license) - https://sketchfab.com
- [ ] Freesound (audio) - https://freesound.org
- [ ] OpenGameArt (various) - https://opengameart.org

---

## Phase 21: Deployment & DevOps

### 19.1 Build Configuration
- [ ] Production build scripts
- [ ] Environment variable management
- [ ] Build optimization (minification, tree-shaking)
- [ ] Bundle analysis

### 19.2 Startup Scripts
- [ ] `start.sh` - Unix/Mac/Linux startup
- [ ] `start.bat` - Windows startup
- [ ] `start-server-only.sh` - Just game server
- [ ] `start-web-only.sh` - Just web client
- [ ] Process management (PM2 or similar)

### 19.3 Cloudflare Tunnel Setup
- [ ] Document cloudflared installation
- [ ] Tunnel configuration for web (port 3000)
- [ ] Tunnel configuration for game server (port 2567)
- [ ] Auto-start tunnel script
- [ ] SSL/TLS configuration

### 19.4 Monitoring
- [ ] Server health monitoring
- [ ] Player count logging
- [ ] Error logging
- [ ] Performance metrics

---

## Phase 22: Testing & Documentation

### 20.1 Testing
- [ ] Unit tests for game logic
- [ ] Integration tests for networking
- [ ] Load testing (max players)
- [ ] Cross-browser testing
- [ ] Mobile device testing (if supported)

### 20.2 Documentation
- [ ] README.md with setup instructions
- [ ] CONTRIBUTING.md for developers
- [ ] API documentation (if applicable)
- [ ] Game mechanics documentation
- [ ] Troubleshooting guide

### 20.3 Player Guide
- [ ] Controls reference
- [ ] Game objectives explanation
- [ ] Monster guide
- [ ] Item guide
- [ ] Tips and strategies

---

## Implementation Order (Recommended)

### Sprint 1: Foundation
1. Phase 1 - Project Foundation (setup, infrastructure)
2. Phase 2 - Game Server (Colyseus rooms, state)

### Sprint 2: Core Movement & Rendering
1. Phase 6 - Client Core Systems (network, state, input)
2. Phase 7 - 3D Rendering (scene, player, environment)
3. Phase 8 - Player Controller (movement, physics, interactions)

### Sprint 3: Multiplayer Sync
1. Server game loop implementation
2. Player state synchronization
3. Multiplayer movement testing
4. Client-side prediction

### Sprint 4: World Generation
1. Phase 4 - Procedural Map Generation (BSP, rooms, hazards)
2. Phase 5 - Weather System
3. Environment rendering and lighting

### Sprint 5: Economy & Items
1. Phase 3 - Economy System (credits, shop, moons)
2. Phase 9 - Inventory System (items, tools, scanning)
3. Item networking and sync

### Sprint 6: Enemies & AI
1. Phase 10 - Monster System (AI, states, pathfinding)
2. Inside monsters implementation
3. Outside monsters implementation
4. Monster spawning and difficulty

### Sprint 7: Core Game Loop
1. Phase 11 - Day/Night & Time System
2. Phase 12 - Ship System (teleporter, radar, upgrades)
3. Phase 16 - Game Flow (rounds, quota, host migration)

### Sprint 8: Communication
1. Phase 13 - Sound/Noise Mechanics
2. Phase 14 - Voice Chat (proximity, walkie-talkie)
3. Phase 15 - Text Chat
4. Phase 16 - Audio System

### Sprint 9: UI & Features
1. Phase 17 - User Interface (menus, HUD, settings)
2. Phase 19 - Bestiary & Log System
3. Ping/marker system

### Sprint 10: Polish & Assets
1. Phase 18 - Polish & Quality
2. Phase 20 - Assets (models, textures, audio)

### Sprint 11: Optimization
1. Phase 17 - Performance Optimization
2. Network optimization
3. Rendering optimization

### Sprint 12: Deployment & Testing
1. Phase 21 - Deployment & DevOps
2. Phase 22 - Testing & Documentation
3. Final bug fixes and polish

---

## Quick Reference

### Key Bindings (Default)
| Key | Action |
|-----|--------|
| W/A/S/D | Move |
| Mouse | Look |
| Shift | Sprint |
| Ctrl | Crouch |
| Alt | Slow Walk (quieter) |
| Space | Jump |
| E | Interact/Pickup |
| G | Drop Item |
| Q | Switch Item Left |
| Scroll | Switch Item |
| 1-4 | Select Item Slot |
| F | Toggle Flashlight |
| T | Toggle Walkie-Talkie |
| V | Push-to-Talk |
| Y | Open Chat |
| Tab | Inventory/Status |
| Esc | Pause Menu |
| RMB | Scan Item |
| Z / MMB | Ping/Mark Location |
| B | Open Bestiary |

### Network Ports
| Service | Port |
|---------|------|
| Web Client (Next.js) | 3000 |
| Game Server (Colyseus) | 2567 |

### Commands
```bash
# Start everything (development)
pnpm dev

# Start everything (production)
pnpm start

# Build for production
pnpm build

# Start with Cloudflare tunnel
./scripts/start-with-tunnel.sh
```

---

## Notes

### Performance Targets
- **Client FPS**: 60 FPS minimum at 1080p
- **Server Tick Rate**: 20 ticks/second
- **Network Latency**: < 100ms acceptable
- **Max Players**: 6 per room
- **Max Rooms**: Limited by server hardware (estimated 10-20 on good PC)

### Browser Support
- Chrome 90+ (primary)
- Firefox 88+
- Edge 90+
- Safari 14+ (WebRTC may have issues)

### Known Limitations
- No mobile support initially
- Voice chat requires HTTPS or localhost
- WebRTC may have issues with strict firewalls

---

*Last Updated: Project Start*
*Status: Planning Complete - Ready for Implementation*
