# Tachiko Fortune — Product Vision

Status: **Canonical product direction**

## Product statement

**Tachiko Fortune is a modern city-strategy board game where movement, investment, timing, and tactical interference turn a compact city map into an emergent story machine.**

It should feel immediately legible to players who know digital board/tycoon games while developing a clearly original identity in its systems, content, presentation, and pacing.

The first content pack is Tokyo-inspired, but the product is designed from day one as a reusable city-game platform rather than a single hard-coded map.

## Why this product exists

Classic digital board games created strong social stories from a small set of interacting systems: movement, money, luck, ownership, disruption, and rivalry. Tachiko Fortune keeps that **systemic appeal**, but updates the product around modern expectations:

- faster turns and less dead time;
- clearer state and consequences;
- stronger city identity;
- more meaningful route and timing decisions;
- deterministic rules suitable for replays, debugging, AI, and future multiplayer;
- data-driven city/content packs;
- polished modern presentation instead of nostalgia-dependent imitation.

## Target experience

A good Tachiko Fortune session should repeatedly create moments like:

- “Do I take the safer route or risk the district with better upside?”
- “Do I invest now, keep liquidity, or prepare a tactical response?”
- “That event changed the whole board, but I could have planned around it.”
- “This session produced a story none of us expected.”
- “I want to replay the same city with a different strategy.”

The game should be competitive and mischievous without becoming opaque or purely random.

## Product pillars

### 1. The city is a system

A city pack is more than art on top of a generic board. Districts may differ through movement topology, investment profiles, event pools, local modifiers, traffic/tempo, and risk/reward patterns.

Tokyo is the first proving ground because it naturally supports distinct neighborhoods, route choices, transit-like movement, density differences, and recognizable urban rhythms.

Real-world place names or geography may be used only where appropriate and lawful, but branded logos, protected visual identities, proprietary maps, or third-party commercial assets are not assumed to be available. The game’s board geometry, presentation, economy, and events must remain original.

### 2. Decisions before dice

Randomness creates uncertainty, not the whole game.

Players should make consequential choices around:

- route selection;
- cash versus investment;
- timing of upgrades or development;
- tactical resources;
- event exposure;
- opponent positioning;
- short-term recovery versus long-term value.

A lucky roll should matter, but preparation and response should matter more across a full session.

### 3. Fast, readable turns

The game should aggressively reduce waiting and ambiguity.

Design targets for the vertical slice:

- most routine turns should require only a few clear decisions;
- animations should be skippable/accelerable after comprehension is established;
- players should always know whose turn it is, what just happened, and why money/state changed;
- event/effect resolution should expose cause and consequence clearly.

### 4. Emergent rivalry

Interaction should come from shared systems, not just arbitrary punishment.

Good interference creates counterplay: positioning, resource commitment, telegraphing, opportunity cost, defensive options, or meaningful timing. Avoid designs that only remove agency for long stretches.

### 5. Content packs, not forks

A new city should primarily be authored through data and content definitions rather than new core rules.

The platform should eventually support:

- different board graphs;
- district/location catalogs;
- event decks/pools;
- tactical action sets;
- scenario/rule presets;
- AI tuning;
- visual/audio themes.

Core changes should be required only when a genuinely new reusable mechanic is introduced.

### 6. Local-first, online-ready

The first goal is a great single-device game with human and AI seats. Online multiplayer comes later.

However, the simulation is deterministic and command-driven from the start so that future authoritative networking does not require a redesign of the game rules.

## Audience

Primary audience:

- players who enjoy accessible strategy, party competition, digital board games, city themes, and emergent stories;
- players nostalgic for the **genre** of classic PC board/tycoon games but who want a genuinely new game rather than a remake;
- small groups who want a 20–45 minute competitive session that can scale in complexity over time.

Secondary audience:

- solo players who enjoy AI opponents and challenge presets;
- creators who may eventually build city/content packs through Tachiko Work or other authoring tools.

## Initial game shape

The first playable vertical slice should prove one complete session, not maximize feature count.

### Required

- one original Tokyo-inspired compact board;
- 2–4 seats;
- human and baseline AI participants;
- seeded movement/randomness;
- branching movement decisions;
- a small location/investment system;
- income/cost pressure and meaningful liquidity decisions;
- an event/effect framework with a small original event set;
- a small original tactical-action set;
- explicit end/win conditions;
- save/load;
- developer replay/debug support;
- clean game-state UI and turn feedback.

### Deliberately deferred

- online multiplayer and matchmaking;
- user accounts/cloud sync;
- monetization/economy services;
- large-scale progression/meta-game;
- workshop/mod distribution;
- final audiovisual production;
- dozens of cities;
- deep simulation of real-world Tokyo;
- any compatibility/import path for another commercial game.

## Originality test

Before accepting a mechanic or content set, ask:

1. Would this still make product sense if *Richman 4 / 大富翁 4* did not exist?
2. Can we explain the player value without saying “because the reference game has it”?
3. Are terminology, tuning, content, presentation, and implementation independently designed?
4. Does it strengthen Tachiko Fortune’s city-first identity?

If the answer is weak, redesign or omit it.

## First-city direction: Tokyo-inspired

The vertical slice should use a **fictionalized, original board inspired by Tokyo’s urban structure**, not a copied map or a one-to-one geographic simulation.

Useful inspiration includes:

- dense central districts versus quieter outer areas;
- hub-and-spoke or loop-and-branch movement;
- contrasting district identities;
- transit/tempo concepts;
- seasonal/local events;
- choices between expensive high-upside areas and resilient lower-cost areas.

The first board should remain small enough to balance quickly and understand at a glance.

## Long-term vision

If the core proves fun, Tachiko Fortune can grow into:

1. **More city packs** with distinct topology and strategic identity.
2. **Better AI** with personalities/strategic profiles derived from the same command interface as humans.
3. **Online multiplayer** built around authoritative deterministic simulation.
4. **Scenario and ruleset presets** for shorter, longer, casual, or competitive sessions.
5. **Creator tooling** where Tachiko Work can author/validate city packs, event data, balance tables, and simulation experiments.
6. **Replayable simulation** supporting balance analysis, regression tests, spectating, and eventually asynchronous or tournament formats.

## Product success criteria for the first milestone

The first milestone is successful when:

- a complete 2–4 player match can start, progress, and finish without manual intervention;
- a player can understand the board state and major consequences without reading debug logs;
- repeated runs with the same seed and commands produce the same result;
- the game remains fun enough to immediately justify a second match during internal playtesting;
- testers describe meaningful route/economy/timing decisions rather than “mostly random”; and
- no shipped mechanic, asset, text, board layout, or tuning table depends on copying a commercial reference title.
