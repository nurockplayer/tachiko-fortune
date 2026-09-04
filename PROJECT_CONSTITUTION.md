# Tachiko Fortune Project Constitution

Status: **Canonical**  
Authority: this document governs product, architecture, implementation, review, and content decisions unless explicitly amended by a later accepted GitHub decision.

## 1. Mission

Tachiko Fortune is an original, modern, city-driven digital board game about movement, investment, risk, tactical interference, and emergent stories.

It may learn from the broad **genre structure and player experience** of classic digital property/tycoon board games, including *Richman 4 / 大富翁 4*, but it is **not a remake, clone, compatibility project, asset replacement project, or rules reproduction project**.

The goal is to build a new game that can eventually support multiple city content packs, strong single-player AI, local multiplayer, online multiplayer, modding/content authoring, and deep integration with Tachiko Work as an optional authoring/control plane.

## 2. Non-negotiable originality boundary

*Richman 4 / 大富翁 4* is a **genre reference only**, never a specification.

The repository MUST NOT copy, extract, trace, recreate, transcribe, or intentionally imitate protected expression from the reference game or any other commercial game, including:

- source code, binaries, reverse-engineered implementation details, data files, file formats, or internal constants;
- characters, character names, portraits, animations, mascots, dialogue, story text, jokes, voice lines, or distinctive terminology;
- board layouts, maps, tile sequences, property arrangements, coordinates, progression tables, prices, probabilities, balance tables, card/item lists, event text, AI scripts, or scenario data;
- UI layouts, HUD composition, iconography, visual trade dress, sprites, textures, fonts, sound effects, music, video, logos, branding, or marketing copy;
- a feature whose only justification is “the original has it” without an independent product rationale.

Allowed inspiration is limited to **unprotectable/high-level genre ideas** such as: turn-based board movement, dice/randomized movement, branching routes, buying or developing locations, earning and spending currency, random events, tactical consumables/skills, computer-controlled opponents, and local/online multiplayer.

When a mechanic resembles a familiar genre mechanic, Tachiko Fortune must define its **own terminology, data model, tuning, presentation, interactions, and player-facing expression**.

If a contributor is unsure whether something is too close to a reference work, the default action is **HOLD and open a GitHub decision issue** before implementation.

## 3. Product principles

1. **City first, board second.** A board represents a living place with districts, movement patterns, local identity, opportunities, and events—not merely a loop of purchasable squares.
2. **Readable immediately, deep over time.** A new player should understand the current situation quickly; mastery comes from routing, timing, investment, risk management, and interaction.
3. **Emergent stories over scripted imitation.** Memorable sessions should arise from systems interacting, not copied scenarios or scripted beats.
4. **Short feedback loops.** Turns should resolve briskly. Animation supports comprehension and delight, never blocks the game unnecessarily.
5. **Original content by default.** Placeholder content must also be original or clearly licensed; “temporary copied assets/data” are not allowed.
6. **Data-driven content.** Cities, districts, locations, events, items, and tuning should be content definitions interpreted by reusable systems.
7. **Deterministic core.** Given the same initial state, commands, and seed, simulation results must be reproducible.
8. **Local-first architecture, multiplayer-ready contracts.** The first playable build is local/single-device, but simulation boundaries must not make future authoritative networking impossible.
9. **Test the rules, project the experience.** Core game rules must be testable without UI. Godot scenes render and control the game; they do not own canonical rules.
10. **No premature platform complexity.** Prove the game loop before adding servers, databases, live ops, account systems, or distributed infrastructure.

## 4. Architecture principles

- Godot is the initial runtime and presentation engine.
- v0 defaults to **typed GDScript** because it minimizes Godot integration and tooling friction while the vertical slice is still proving the product and architecture.
- The implementation language is **not a product contract**. Canonical state, commands, domain rules, content contracts, determinism, and presentation boundaries must remain conceptually portable enough that the domain implementation can later move to C#, Rust, or another suitable technology without redesigning the game model.
- A language/runtime migration requires an accepted GitHub decision with a concrete reason such as maintainability, testability, performance, tooling quality, platform constraints, or cross-runtime reuse. It does not require a constitutional amendment unless one of the actual architecture principles changes.
- Domain/simulation code MUST NOT depend on scene-tree timing, rendered nodes, animation state, or wall-clock time.
- Canonical game state is explicit data; UI is a projection of that state.
- Player and AI actions enter the simulation as validated commands.
- Randomness comes only from an explicit seeded RNG owned by simulation state or an injected deterministic RNG service.
- Content definitions are versionable and schema-validated.
- Save games and replays must have explicit format versions.
- Network transport is not part of the v0 simulation core; future multiplayer should transmit commands/state transitions rather than embed rules in RPC handlers.
- Tachiko Work integration is optional and downstream: the game must remain buildable and playable without Tachiko Work.

## 5. Game-design guardrails

The initial game may contain systems for movement, locations, investment, cash flow, events, tactical actions, and victory/termination. Their exact design must be original.

For every substantial mechanic, its issue or design note should answer:

- What player decision does this create?
- How does it reinforce the city fantasy?
- What counterplay or trade-off exists?
- How is it materially expressed differently from obvious reference games?
- What data/configuration does the mechanic require?

Avoid one-to-one translations of familiar mechanics. Prefer combining or reframing systems around Tachiko Fortune’s own identity.

## 6. Initial product scope

The first milestone is a **single-device, single-board vertical slice** with:

- 2–4 participants, with human and simple AI seats;
- one small, original Tokyo-inspired board/content pack;
- deterministic turn flow and movement;
- a minimal original location/investment economy;
- an original event/effect system;
- a small set of original tactical actions;
- clear win/end conditions;
- save/load and deterministic replay/debug support sufficient for development;
- placeholder visuals that are original, generated for the project, public-domain, or properly licensed.

Explicitly out of scope for the first vertical slice:

- online multiplayer;
- accounts, cloud saves, matchmaking, live services, monetization;
- large content libraries;
- final art, final audio, localization breadth;
- compatibility with any existing game’s save/data formats;
- importing or converting assets/data from commercial games.

## 7. Engineering quality bar

A change is not complete merely because it runs once.

For rule/system work, completion normally requires:

- deterministic tests for expected behavior and edge cases;
- no canonical rule hidden in UI scenes;
- explicit error handling for invalid commands/content;
- stable data contracts or a documented migration when contracts change;
- no unrelated refactor;
- a concise PR description tied to an issue and acceptance criteria.

The default implementation strategy is the **smallest coherent vertical change**. Architecture must enable growth without building future infrastructure before it is needed.

## 8. AI-agent operating rules

AI accelerates implementation; GitHub remains project authority.

Agents MUST:

1. read this constitution plus `docs/PRODUCT_VISION.md`, `docs/ARCHITECTURE.md`, and the assigned issue before coding;
2. treat issue acceptance criteria as the implementation contract;
3. make the smallest coherent change required by the issue;
4. preserve deterministic simulation boundaries;
5. use only original or explicitly licensed content;
6. stop and raise a decision issue when a requested change conflicts with this constitution or creates an originality/IP concern;
7. never broaden scope merely to imitate a reference title.

Agents MUST NOT treat screenshots, videos, extracted files, walkthroughs, wikis, decompiled data, or fan-maintained numeric tables from a commercial reference game as implementation specifications.

## 9. Decision authority and amendments

Priority order:

1. current explicit user direction;
2. accepted project decisions in GitHub;
3. this constitution;
4. canonical product/architecture documentation;
5. issue acceptance criteria;
6. implementation and tests;
7. stale discussion/context.

Any constitutional amendment must be deliberate, explain why the existing rule is insufficient, identify compatibility/originality impact, and be recorded in GitHub before implementation that relies on it.

## 10. Definition of success

Tachiko Fortune succeeds when a player familiar with classic board/tycoon games immediately understands the genre, but after playing can describe **Tachiko Fortune’s own systems, city identity, decisions, stories, and visual language** without needing to compare it to *Richman 4 / 大富翁 4*.
