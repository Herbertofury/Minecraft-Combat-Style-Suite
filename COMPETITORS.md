# Comparable combat integrations and alternatives

## Identity and canonical research

The [existing Compatibility Research](wiki/Compatibility-Research.md) is the detailed RC16 authority and historical evidence. This index adds an ongoing comparison entry point; it does not relabel September's source audit as fresh native verification. Combat Style Suite is an ownership/compatibility layer, not another replacement melee engine.

| Reference and primary source | Role in comparison | Evaluation target |
|---|---|---|
| [Better Combat](https://github.com/ZsoltMolnarrr/BetterCombat) | Primary combat provider | Preserve weapon attributes, input semantics, readiness and explicit user OFF state. |
| [Epic Fight](https://modrinth.com/mod/epic-fight) | Alternative primary combat provider | One owner at a time; mode changes must transfer attack and presentation authority coherently. |
| [epic-tacz](https://github.com/Ardelhite/epic-tacz) | Narrow Epic Fight/TaCZ bridge | Compare specialist firearm takeover and restoration with suite-wide orchestration. A narrower bridge can be the better solution for a smaller pack. |
| [Better Combat × Epic Fight Compat](https://www.curseforge.com/minecraft/mc-mods/better-combat-epic-fight-compat) | Author-described mode-based switching alternative | Compare its supported pairwise workflow against the suite's broader provider matrix without claiming broader automatically means better. |

## Target-native comparison backlog

- [ ] Pin exact Forge 1.20.1 provider versions and reproduce vanilla → Better Combat → Epic Fight → firearm → prior owner transitions in both perspectives.
- [ ] Verify explicit OFF persists through delayed readiness, config reload, reconnect and weapon change. Never force-enable a provider to fake compatibility.
- [ ] Exercise Punchy/YSM companion rendering separately from melee ownership; compare hand/model glitches and input behavior, not screenshots alone.
- [ ] Benchmark transition cost, join-time spikes and steady-state frame/tick work with identical packs. A cached transition design is a hypothesis until measured.
- [ ] Retain one minimal reproducer per provider conflict. Investigate deeper skeleton blending or retained dodge only when a real native defect/use case justifies it.
- [ ] Keep recommendations narrow: direct pairwise bridge, suite or no bridge depending on actual pack needs. No speculative universal weapon registry.

**Priority:** native regression evidence for the existing architecture before another ownership rewrite. Fresh documentation review does not validate the earlier audit's implementation checkmarks on a newly assembled pack.

## Evidence and maintenance contract

Reviewed 2026-11-12. This is a public-source design comparison, not a hands-on benchmark, exhaustive market ranking, feature acceptance, or permission to copy code/assets. Source pages may describe newer loaders or editions than our target. Recheck exact versions, licenses and integration boundaries before implementation. Preserve approved identity and existing progress; do not replace working systems merely because a competitor exists.

During each daily review, check linked official release notes/issues and update only material changes. Record source date, observation, relevance, proposed experiment and result separately. Retain failed comparisons and the last-good evidence. Community complaints are leads, not established product defects. New speculative features remain proposals until approved. Before claiming improvement, compare the same workload and retain latency/frame-time, memory, correctness and recovery results on the actual project build.

## Review log

- 2026-11-12: initial repository-scoped comparison index; official sources reviewed at documentation level. No gameplay, performance or compatibility tests run in this pass. No implementation or native GitHub Wiki publication claimed.
