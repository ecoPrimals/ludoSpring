# kenshi.reborn — Sovereign A-Life Game Engine

**Date**: November 13, 2026 (Wave 229e)
**Status**: EXECUTION BEGUN
**License**: AGPL-3.0-or-later (scyBorg triple)
**GPU Target**: northGate RTX 5090 (32GB GDDR7, ~209 TFLOPS) via tissue proxy π:5820

## What This Is

kenshi.reborn is the convergence of everything built so far:

- **ludoSpring** → validated game science (Flow, DDA, A-Life math, 982 tests)
- **petalTongue** → game loop (60Hz), scene graph, render plan
- **tissue proxy** → CORS bridge to northGate RTX 5090
- **bingoCube** → coordination, game state
- **Lysogeny** → open recreation of proprietary mechanics (Usurper = Nemesis, Quorum = emergent narrative)
- **RPGPT** → NPC personality, dialogue, trust, internal voices

Named for Kenshi (252h played). Takes the DNA of squad-based open-world A-Life
and rebuilds it on sovereign hardware.

## Kenshi DNA → ecoPrimal Mapping

| Kenshi Feature | ecoPrimal Component | Module |
|----------------|--------------------|---------| 
| Squad control | `EntityRegistry` with party entities | `engine/entity.rs` |
| Open world | `TileWorld` with chunk loading | `engine/world.rs` |
| A-Life (NPCs live autonomously) | **Usurper** replicator dynamics + agent tick | `Lysogeny Target 1` |
| Emergent narrative | **Quorum** agent-based DAG causality | `Lysogeny Target 5` |
| Faction politics | **Symbiont** multi-species Lotka-Volterra | `Lysogeny Target 3` |
| Crafting/building | Action economy + Surface terrain | `engine/action.rs` |
| Training (stats improve via use) | DDA + engagement feedback | `interaction/difficulty.rs` |
| Brutal difficulty | Flow channel calibration | `interaction/flow.rs` |
| NPC personality/memory | RPGPT personality certs + trust dynamics | `rpgpt/personality_dynamics.rs` |
| Persistent world | Session + loamSpine certificates | `engine/session.rs` |

## Architecture

```
Browser (any device on LAN)
  │
  │ HTTP/WebSocket
  ▼
tissue proxy (π:5820, eastGate)          ← CORS bridge
  │
  │ HTTP forward
  ▼
northGate RTX 5090 (:11434)             ← A-Life inference engine
  │                                         (tissue-gut model for NPC behavior)
  │
  ▼
Game World State (membrane JSON)         ← persistent, survives restart
  │
  ▼
Browser renders world state              ← Canvas/WebGL/WebGPU
  │
  ▼
Player input → squad commands            ← keyboard/Steam Controller/touch
```

### The Key Insight: GPU as A-Life Brain

The RTX 5090 doesn't just render — it THINKS. Each A-Life entity's behavior
is an inference call through the tissue model. The GPU runs NPC decision-making:

```
Entity needs food → tissue inference → "wander toward market"
Entity attacked → tissue inference → "fight/flee/surrender based on personality"
Entity sees opportunity → tissue inference → "betray/cooperate/ignore"
```

This is the Kenshi pattern: the world lives. The GPU is the world's brain.

### Tick Architecture

```
Server tick (1 Hz game time, adjustable):
  1. Advance world clock
  2. For each A-Life entity:
     a. Evaluate needs (hunger, safety, ambition)
     b. Tissue inference for decision
     c. Execute action (move, fight, trade, build)
  3. Resolve interactions (combat, trade, diplomacy)
  4. Update world state
  5. Broadcast delta to connected clients

Client tick (60 Hz render):
  1. Receive world delta via WebSocket/polling
  2. Interpolate entity positions
  3. Render visible world chunk
  4. Handle player input → queue commands
  5. Send commands to server
```

## GPU Shader Usage

| Shader | Purpose | File |
|--------|---------|------|
| `fog_of_war.wgsl` | Squad visibility calculation | ✅ exists |
| `pathfind_wavefront.wgsl` | NPC movement planning | ✅ exists |
| `tile_lighting.wgsl` | Day/night cycle + torch lighting | ✅ exists |
| `dda_raycast.wgsl` | Line of sight for combat/detection | ✅ exists |
| `perlin_2d.wgsl` | Terrain generation for new chunks | ✅ exists |

All 5 WGSL shaders are already validated in ludoSpring exp030-033.

## World Design

### Biomes (Terrain → TileWorld)
- **Wasteland**: Kenshi's Hub/Border Zone equivalent. Open terrain, scattered ruins.
- **Marsh**: Difficult terrain, water tiles. Rich resources, dangerous.
- **Mountain**: Wall-heavy, narrow passes. Defensive positions.
- **Desert**: Open, harsh. Training ground (like Kenshi's Great Desert).
- **Settlement**: Surface tiles (workbenches, merchants). Social zones.

### Factions (→ Symbiont Lotka-Volterra)
- Factions compete using multi-species competition coefficients
- Faction strength = sum of entity fitness in territory
- Player faction starts at zero — you're nobody (classic Kenshi feel)

### Squad Mechanics
- Player controls 1-30 entities (classic Kenshi squad)
- Each entity: stats (combat, athletics, toughness, crafting, etc.)
- Stats improve through USE (Kenshi's training system = DDA feedback loop)
- Limb system: body parts with HP, damage affects capability

## Execution Path

### Phase 1: World Alive (THIS SESSION)
- [ ] Prototype HTML page: canvas-rendered tile world
- [ ] Connect to tissue proxy for NPC behavior inference
- [ ] 3 entity types: Player squad, Hostile NPC, Neutral NPC
- [ ] Movement on tile grid, fog of war from squad vision
- [ ] World state persisted as JSON in membrane

### Phase 2: A-Life Ticks
- [ ] Server-side tick daemon on eastGate
- [ ] Entities have needs (hunger, safety) that drive behavior
- [ ] Tissue inference for NPC decisions
- [ ] World advances when player is away

### Phase 3: Combat + Training
- [ ] Action economy from `engine/action.rs`
- [ ] Stats improve through use (DDA integration)
- [ ] Limb/damage system
- [ ] Death/knockout/capture states

### Phase 4: Building + Economy
- [ ] Base building on Surface tiles
- [ ] Crafting chains (materials → products)
- [ ] Trade with NPC factions
- [ ] Economy driven by Lotka-Volterra faction dynamics

### Phase 5: RPGPT Integration
- [ ] NPC personality certificates
- [ ] Dialogue plane transitions
- [ ] Trust dynamics
- [ ] Internal voices for squad members

### Phase 6: Full Engine
- [ ] WebGPU rendering (petalTongue WASM)
- [ ] Steam Controller input mapping
- [ ] deckGate display (Steam Deck as game console)
- [ ] Multi-client (any browser on LAN)

## Primal Consumption

| Primal | Role in kenshi.reborn |
|--------|-----------------------|
| **ludoSpring** | Game science: entity, world, session, DDA, Flow, A-Life math |
| **petalTongue** | Rendering: scene graph, game loop, modality dispatch |
| **barraCuda** | GPU compute: shaders, tensor ops |
| **bingoCube** | Coordination: game state sync, multiplayer |
| **squirrel** | AI: tissue model routing for NPC behavior |
| **rhizoCrypt** | Provenance: session DAG, action signing |
| **loamSpine** | Persistence: world state certificates |
| **sweetGrass** | Attribution: player/NPC creative credit |
| **bearDog** | Identity: player signing, anti-cheat |
| **songBird** | Mesh: multiplayer discovery |
| **skunkBat** | Security: anti-cheat, audit logging |
| **sourDough** | Foundation: lifecycle, health, IPC |
| **dromedary** | Proxy: π-port routing |

**13 of 17 primals** serve kenshi.reborn. This is what they were built for.

## References

- Kenshi (Lo-Fi Games, 2018): 252h played. Squad-based open-world A-Life sandbox RPG.
- S.T.A.L.K.E.R. 2 A-Life 2.0 (GSC Game World, 2024): Autonomous NPC simulation.
- Dwarf Fortress (Bay 12 Games, 2006): Emergent narrative from world simulation.
- Rimworld (Ludeon Studios, 2018): Storyteller AI + colony management.
- Lysogeny Catalog: `specs/LYSOGENY_CATALOG.md` — all math has published prior art.
