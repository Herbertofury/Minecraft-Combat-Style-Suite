# Repository instructions

Read `.agents/foundry/AGENTS-adapter.md` for the pinned shared engineering core.
Keep this file and nested instructions project-owned: setup, commands, constraints and acceptance belong here.

## Repository-local combat contract

- Read RC17-PUBLICATION.md, COMPETITORS.md and wiki/Provider-Toggles.md. Preserve Forge 1.20.1 / Java 17 scope and exact frozen source/artifact lineage. This main checkout contains staged bootstrap archives and documentation, not an unpacked buildable source tree; recover and hash-verify the canonical source before inventing build commands.
- Keep one primary combat/input owner at a time; presentation companions are a separate responsibility. Preserve explicit provider OFF states, safe readiness/handshake gates and restoration of prior ownership. Optional providers or Smart Hybrid must not become mandatory dependencies; Smart Hybrid stays OFF by default and advanced-only.
- Preserve event-driven transitions and one-shot reconciliation. Do not introduce tick enforcement, watchdogs, recurring repair loops or whole weapon-registry crawling as substitutes for causal fixes.
- Existing publication/bootstrap workflows are path-filtered and can publish artifacts/source. Do not run or alter them merely to validate guidance. No ordinary pull-request test workflow exists in this checkout.
- Distinguish historical RC16/RC17 harness/static/package evidence from fresh native acceptance. The exact Epic Fight + Better Combat + YSM + Punchy + TaCZ client combination requires its own real rendered-game verification.
- Keep licensing and version-specific provider compatibility evidence local. Public source does not grant redistribution rights, and research recommendations are not proven performance improvements.

## Project skills

For provider routing, toggle changes, source recovery or RC17 acceptance, read `.agents/skills/provider-acceptance/SKILL.md`. Its transition matrix supplements, rather than replaces, executable tests and fresh native-client evidence.

## Mandatory pre-release optimization gate

Before finalizing every release, follow the owner-mandated [Athena zero-loss pre-release optimization standard](https://github.com/Herbertofury/Athena/blob/main/guidance/PRE_RELEASE_OPTIMIZATION.md). Evaluate shrink-anything, fast-anything and rust-anything using reviewed pinned tools in isolated worktrees; run applicable passes, compare equivalent complete workloads, keep only independently verified improvements, restore rejected changes, and record per-tool evidence or explicit inapplicability/blockers. Never weaken tests, delete required content, force a Rust rewrite or regress functionality, quality, portability, compatibility, security or protected performance metrics. Data/documentation-only releases and existing native Rust paths require honest applicability decisions, not artificial ports. Existing local and pinned governance requirements remain intact.
