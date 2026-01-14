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

## Phase 3: Procedural Map Generation

### 3.1 Generation Algorithm
- [ ] Implement seeded random number generator
- [ ] Create Binary Space Partition (BSP) room generator
- [ ] Create corridor generation between rooms
- [ ] Implement room type assignment (loot room, monster den, etc.)
- [ ] Add entrance/exit placement
- [ ] Generate navmesh for AI pathfinding

### 3.2 Map Features
- [ ] Define room prefab types (small, medium, large, special)
- [ ] Implement door placement and state
- [ ] Add vent/alternate path generation
- [ ] Create outside terrain generation (simple)
- [ ] Implement ship landing zone
- [ ] Add environment hazards (optional)

### 3.3 Chunk System
- [ ] Implement chunk-based map loading
- [ ] Create chunk visibility system
- [ ] Optimize memory with chunk pooling
- [ ] Implement chunk serialization for network sync

### 3.4 Map Decoration
- [ ] Procedural prop placement
- [ ] Lighting placement
- [ ] Scrap/item spawn point generation
- [ ] Monster spawn point generation

---

## Phase 4: Client Core Systems

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

### 8.4 Monster Spawning
- [ ] Spawn point system in generated maps
- [ ] Difficulty scaling (time-based)
- [ ] Max monster limits
- [ ] Respawn mechanics
- [ ] Outside vs inside monsters

### 8.5 Monster Models & Animation
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

---

## Phase 11: Voice Chat System

### 11.1 WebRTC Setup
- [ ] Simple-peer integration
- [ ] Colyseus signaling (offer/answer exchange)
- [ ] ICE candidate handling
- [ ] Connection state management
- [ ] Automatic reconnection

### 11.2 Audio Processing
- [ ] Microphone capture
- [ ] Noise suppression (optional)
- [ ] Volume normalization
- [ ] Push-to-talk option
- [ ] Voice activity detection

### 11.3 Proximity Voice
- [ ] Distance-based volume falloff
- [ ] Maximum hear distance (15 units default)
- [ ] Obstruction detection (walls muffle)
- [ ] Spatial audio (left/right panning)
- [ ] Dead players can't talk to living

### 11.4 Walkie-Talkie
- [ ] Extended range communication
- [ ] Channel system (everyone on same channel)
- [ ] Static/distortion effect
- [ ] Battery consumption
- [ ] Toggle on/off (T key)
- [ ] Visual indicator when in use

### 11.5 Voice UI
- [ ] Speaking indicator (who's talking)
- [ ] Microphone mute toggle
- [ ] Volume controls
- [ ] Push-to-talk indicator

---

## Phase 12: Text Chat System

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

## Phase 13: Audio System

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

## Phase 14: User Interface

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

## Phase 15: Game Flow & Round System

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

---

## Phase 16: Performance Optimization

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

## Phase 17: Polish & Quality

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

## Phase 18: Assets

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

## Phase 19: Deployment & DevOps

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

## Phase 20: Testing & Documentation

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

### Sprint 1: Foundation (Week 1)
1. Phase 1.1 - Project Setup
2. Phase 1.2 - Development Infrastructure
3. Phase 2.1 - Server Foundation
4. Phase 2.2 - Room System
5. Phase 4.1 - Network Client

### Sprint 2: Core Movement (Week 2)
1. Phase 5.1 - Scene Setup
2. Phase 6.1 - First-Person Controller
3. Phase 6.2 - Player Physics
4. Phase 4.2 - State Management
5. Phase 4.3 - Input System

### Sprint 3: Multiplayer Sync (Week 3)
1. Phase 2.3 - State Schemas
2. Phase 2.4 - Server Game Loop
3. Phase 5.2 - Player Rendering
4. Multiplayer movement sync testing

### Sprint 4: World Generation (Week 4)
1. Phase 3.1 - Generation Algorithm
2. Phase 3.2 - Map Features
3. Phase 5.3 - Environment Rendering
4. Phase 5.4 - Lighting System

### Sprint 5: Items & Inventory (Week 5)
1. Phase 7.1 - Inventory Structure
2. Phase 7.2 - Item Types
3. Phase 7.3 - Item Mechanics
4. Phase 7.4 - Inventory UI

### Sprint 6: Enemies (Week 6)
1. Phase 8.1 - AI Foundation
2. Phase 8.2 - AI States
3. Phase 8.3 - Monster Types
4. Phase 8.4 - Monster Spawning

### Sprint 7: Time & Ship (Week 7)
1. Phase 9 - Day/Night System (all)
2. Phase 10 - Ship System (all)
3. Phase 15 - Game Flow (all)

### Sprint 8: Communication (Week 8)
1. Phase 11 - Voice Chat (all)
2. Phase 12 - Text Chat (all)
3. Phase 13 - Audio System (all)

### Sprint 9: UI & Polish (Week 9)
1. Phase 14 - User Interface (all)
2. Phase 17 - Polish (all)
3. Phase 18 - Assets (remaining)

### Sprint 10: Deployment (Week 10)
1. Phase 16 - Optimization (all)
2. Phase 19 - Deployment (all)
3. Phase 20 - Testing & Docs (all)

---

## Quick Reference

### Key Bindings (Default)
| Key | Action |
|-----|--------|
| W/A/S/D | Move |
| Mouse | Look |
| Shift | Sprint |
| Ctrl | Crouch |
| Space | Jump |
| E | Interact/Pickup |
| G | Drop Item |
| Q | Switch Item Left |
| Scroll | Switch Item |
| 1-4 | Select Item Slot |
| F | Toggle Flashlight |
| T | Toggle Walkie-Talkie |
| Y | Open Chat |
| Tab | Inventory/Status |
| Esc | Pause Menu |

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
