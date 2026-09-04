# Tachiko Fortune — Architecture

Status: **Canonical architecture direction for v0**

## 1. Architecture goal

Build the smallest architecture that can support a complete local vertical slice today while preserving clean seams for AI, replays, content packs, automated balance simulation, and future authoritative multiplayer.

The core rule is:

> **Simulation owns truth. Godot scenes project truth.**

UI, animation, audio, and input may react to canonical game state, but they must not become the hidden source of game rules.

## 2. Initial technology choices

### Runtime

- **Godot 4.x**, with the exact stable version pinned by the bootstrap issue and CI.
- **Typed GDScript is the v0 implementation default, not a long-term product contract.**
- Headless Godot execution is the initial test/simulation path.

Do not add C#, Rust, native extensions, an external backend, ECS frameworks, or networking libraries during the bootstrap unless an accepted GitHub decision explicitly changes the scope.

### Why typed GDScript first

The first risk is whether the game loop and architecture are good, not raw compute performance. Typed GDScript minimizes Godot integration/tooling friction, provides enough type information for disciplined AI-assisted development, and is sufficient for a compact deterministic board-game vertical slice when the domain stays independent of scene-tree behavior.

That convenience must not leak into the product model. Canonical state, commands, domain events, content schemas, deterministic RNG behavior, persistence semantics, and rule invariants must not depend on GDScript-only or scene-instance semantics where a plain typed data contract is sufficient.

A future domain-language/runtime change is a normal architectural migration, not a rewrite of the game model. It may be justified by evidence such as:

- maintainability or refactoring quality;
- stronger static typing or tooling needs;
- testability or headless-simulation ergonomics;
- measured performance requirements;
- platform/export constraints;
- reuse of the deterministic domain outside the Godot runtime;
- networking/server-authority requirements that materially benefit from a different runtime.

Any such change requires an accepted GitHub decision that defines the migration boundary and compatibility impact. It does **not** require changing the product vision or constitution unless one of the actual architectural principles changes.

## 3. Layer model

```text
+----------------------------------------------------+
| Presentation / Interaction                        |
| Godot scenes, UI, animation, audio, input          |
+---------------------------+------------------------+
                            |
                            v
+----------------------------------------------------+
| Application / Orchestration                        |
| match controller, command dispatch, view models,   |
| save/replay orchestration                          |
+---------------------------+------------------------+
                            |
                            v
+----------------------------------------------------+
| Domain / Deterministic Simulation                  |
| game state, turn state machine, movement, economy, |
| effects, victory rules, seeded RNG                 |
+---------------------------+------------------------+
                            |
                            v
+----------------------------------------------------+
| Content / Contracts                                |
| board graph, districts, locations, events,         |
| tactical actions, ruleset, schemas/versioning      |
+----------------------------------------------------+
```

Dependencies point downward. Domain code must not import or manipulate presentation scenes.

## 4. Canonical game model

The exact schema will evolve, but v0 should converge on explicit serializable structures equivalent to:

### `GameState`

Owns canonical match state:

- schema/version;
- match id or local identifier;
- deterministic RNG state/seed;
- ruleset id/version;
- board/content pack id/version;
- turn/round counters;
- active participant;
- participant states;
- board/location mutable state;
- pending decision/resolution state;
- event/effect queues if required;
- end-state/winner information.

### `ParticipantState`

Contains rule-relevant player state only:

- participant id;
- seat/order;
- human/AI controller metadata that does not alter rules;
- current board node;
- currency/resources;
- owned investments/interests;
- tactical inventory/capabilities;
- temporary statuses/modifiers.

### `BoardDefinition`

Immutable content definition:

- nodes;
- directed/undirected edges as appropriate;
- branch/path metadata;
- district membership;
- location references;
- special movement semantics expressed through reusable mechanics.

The board is a **graph**, not an assumed fixed loop. A loop can be authored as data, but branching and alternate routes are first-class.

### `LocationDefinition` / mutable location state

Definitions describe original gameplay semantics and content. Mutable state records match-specific ownership/development/status.

### `Command`

All rule-changing external intent enters through validated commands. Examples may include:

- start match;
- roll/advance movement phase;
- choose route;
- choose location action;
- buy/invest/decline;
- use tactical action;
- choose event option;
- end/confirm phase.

Command names and exact phase structure must be designed for Tachiko Fortune rather than copied from another title.

### `DomainEvent`

Simulation emits structured facts describing what occurred, for example money changed, participant moved, location changed, effect applied, or match ended.

Presentation consumes domain events to animate/explain changes. Domain events do **not** replace canonical state.

## 5. Turn engine

The turn system should be an explicit state machine.

Illustrative phases—not a locked final design—may resemble:

```text
TurnStart
  -> PreMoveDecision?
  -> MovementRequest
  -> RouteDecision? / MovementResolution
  -> ArrivalResolution
  -> LocationDecision? / EventDecision?
  -> PostArrivalEffects
  -> TurnEnd
  -> next participant
```

Requirements:

- legal commands are determined by current state/phase;
- invalid commands fail explicitly without partially mutating state;
- UI cannot skip required simulation transitions;
- AI uses the same legal command interface as human controllers;
- future network clients can use the same command contract.

## 6. Determinism

Determinism is mandatory because it enables reproducible bugs, headless tests, AI simulation, replay, and future multiplayer.

Rules:

1. Simulation may not call global random functions directly.
2. All randomness flows through one deterministic RNG abstraction/state.
3. Simulation may not depend on wall-clock time, frame delta, render order, node lifecycle, platform locale, or unordered collection iteration where order affects results.
4. Commands and content versions must be enough to reproduce a run when combined with initial seed/state.
5. Floating-point values should be avoided for currency and other exact rule values when integer/fixed representations are sufficient.

A determinism regression test should eventually run the same command log twice and compare canonical state hashes/snapshots.

## 7. Content architecture

Content should be data-driven and validated at load time.

Recommended initial repository shape:

```text
res://
  app/
  domain/
    commands/
    systems/
    state/
    rng/
  content/
    schemas/
    packs/
      tokyo_v0/
  presentation/
    board/
    hud/
    effects/
  ai/
  persistence/
  tests/
```

Exact directories may change if the bootstrap issue discovers a clearer Godot-native layout, but the dependency boundary must remain.

### Content pack contract

A pack should eventually declare:

- pack metadata/version;
- board graph;
- districts/locations;
- original event definitions;
- original tactical actions;
- tuning parameters;
- presentation references owned/licensed by this project.

Do not create an importer for *Richman 4 / 大富翁 4* or another commercial title.

### Tachiko Work integration seam

Tachiko Work may later author or validate these same semantic contracts. Therefore:

- contracts should be documented and versioned;
- content should not require editing Godot scene internals for routine data changes;
- the runtime must not depend on Tachiko Work being installed or available;
- exported content should be ordinary repository artifacts consumable by Godot and CI.

## 8. Economy and effect systems

Avoid baking each content item into bespoke scene scripts.

Use small reusable mechanics such as:

- transfer currency;
- modify location value/state;
- grant/remove resource;
- apply temporary modifier;
- move participant according to a rule;
- select from an event pool;
- alter future costs/yields for a bounded duration.

Complex content should compose reusable effect primitives where possible. New primitives require tests because they expand the trusted simulation surface.

This architecture is **not** permission to recreate another game’s item/event catalog through renamed data.

## 9. AI architecture

AI is a controller, not a separate rules implementation.

Flow:

```text
GameState + legal commands
        -> AI policy
        -> chosen Command
        -> normal simulation validation/execution
```

v0 AI should be simple and deterministic under a supplied AI seed/tie-break policy where practical. Start with heuristic decisions; do not add machine learning infrastructure.

## 10. Persistence and replay

### Save

A save contains enough versioned canonical state to resume a local match. Save migrations are explicit once more than one format exists.

### Replay/debug log

A development replay should prefer:

- initial seed/config/content versions;
- ordered validated commands;
- optional checkpoints/state hashes.

Replay is initially a development/debug feature, not a polished consumer feature.

## 11. Presentation boundary

Godot scenes may:

- collect input;
- request legal actions;
- dispatch commands;
- render current state;
- animate emitted domain events;
- manage camera, audio, transitions, accessibility, and visual polish.

Godot scenes may NOT:

- silently change canonical money/ownership/position;
- roll canonical random outcomes themselves;
- encode hidden victory conditions;
- decide legal moves independently from the domain;
- require animation completion to determine rule outcomes.

Animation follows resolved state. The game must remain logically correct with animations disabled.

## 12. Future multiplayer seam

Do not implement online multiplayer in v0.

When it arrives, the preferred model is an authoritative simulation host/server that validates ordered commands against canonical state. Clients render projected state and submit intent.

The v0 architecture should therefore avoid:

- rules encoded only in local UI;
- non-deterministic transitions;
- state mutated by arbitrary node scripts;
- player-controller code directly owning domain state.

No commitment is made yet to dedicated servers, peer hosting, rollback, lockstep, or a specific networking provider.

## 13. Testing strategy

### Domain tests

High priority:

- state-machine legality;
- movement/path selection;
- seeded RNG reproducibility;
- economy transfers and invariants;
- location state transitions;
- event/effect application;
- end conditions;
- invalid command rejection;
- save round-trip;
- deterministic replay equivalence.

### Content validation

CI should reject malformed packs, invalid board edges/references, duplicate ids, impossible required values, and unsupported schema versions.

### Presentation tests

Keep these selective. The critical contract is that presentation can render/use domain state without owning rules.

## 14. Performance posture

A small board game does not justify premature optimization.

Targets:

- instant-feeling local interaction;
- fast headless simulation for tests and future balance runs;
- no avoidable per-frame domain work;
- profile before introducing native code or architectural complexity.

## 15. Architectural decision triggers

Open a decision issue before:

- changing the v0 implementation language or introducing a second domain/runtime language such as C#, Rust, GDExtension, or another runtime;
- adding a backend/database/account service;
- implementing online networking;
- changing canonical save/replay contracts after release usage exists;
- making Tachiko Work a runtime dependency;
- adding an importer/compatibility layer for third-party game data;
- weakening deterministic simulation requirements;
- moving canonical rules into scenes/UI.

A language-change decision should evaluate migration cost against concrete benefits in maintainability, testability, tooling, performance, platform support, or cross-runtime reuse. The default is to preserve the canonical contracts and replace only the implementation surface that needs to change.

## 16. v0 dependency direction summary

```text
content definitions
      ↓
deterministic domain
      ↓
application orchestration
      ↓
presentation / AI adapters / persistence adapters
```

AI and human input both produce commands. Presentation and persistence observe or serialize domain truth. Future networking must connect at the command/state boundary rather than becoming a second rules engine.

The language choice sits **inside** the deterministic-domain implementation box; it is not part of the external product contract. Changing that language later must not require redesigning board/content semantics, player commands, canonical state meaning, or presentation ownership boundaries.
