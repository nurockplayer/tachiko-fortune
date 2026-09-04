# AGENTS.md

This repository is governed by:

1. `PROJECT_CONSTITUTION.md`
2. `docs/PRODUCT_VISION.md`
3. `docs/ARCHITECTURE.md`
4. the assigned GitHub Issue and accepted GitHub decisions

Read them before making changes.

## Hard rules

- Tachiko Fortune is an **original game**, not a clone/remake/compatibility implementation of *Richman 4 / 大富翁 4* or any other commercial title.
- Do not use commercial-game source code, binaries, extracted/decompiled data, proprietary formats, board layouts, numeric tables, characters, text, assets, UI compositions, sound/music, or renamed catalogs as implementation input.
- Screenshots, videos, walkthroughs, wikis, fan tables, and reverse-engineered material from a reference game are not specifications for this repository.
- High-level genre mechanics are allowed only when independently designed and justified by Tachiko Fortune’s own product goals.
- If originality/IP boundaries are unclear, stop and open/raise a GitHub decision instead of guessing.

## Engineering defaults

- Godot 4.x is the initial runtime/presentation engine.
- Typed GDScript is the **v0 implementation default**, chosen to minimize integration/tooling friction. It is not a permanent product or domain contract.
- Keep canonical state, commands, domain events, content contracts, determinism, and rule invariants independent of scene-instance behavior and avoid unnecessary GDScript-only semantics at public/domain boundaries.
- A later move of some or all domain implementation to C#, Rust, or another suitable technology is allowed through an accepted GitHub architecture decision when maintainability, testability, tooling, performance, platform constraints, networking, or cross-runtime reuse justify it.
- Keep canonical rules in deterministic domain code, not scenes/UI/animation.
- All rule-changing input goes through validated commands.
- Randomness must be seeded and deterministic.
- AI must use the same legal command surface as human input.
- Content must be data-driven, versioned, and validated.
- Tachiko Work may become an authoring tool later, but is not a runtime dependency.
- Do not add online services, databases, additional runtime languages/native extensions, networking infrastructure, or large frameworks without an accepted issue/decision that requires them.

## Change discipline

- Implement one issue at a time.
- Make the smallest coherent change.
- Do not perform unrelated refactors or cleanup.
- Add/update tests for domain behavior and invariants.
- Preserve repository toolchain, pinned versions, formatting, and CI once established.
- Never force-push.
- PR descriptions should link the issue, summarize the change, list verification, and call out any contract/architecture impact.

## Definition of done

An implementation issue is done only when its acceptance criteria are met, relevant tests pass, no canonical rule is hidden in presentation code, and the change does not cross the originality boundary.
