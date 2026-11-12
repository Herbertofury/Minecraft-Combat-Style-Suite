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

## Candidate foundations, concrete reuse and shared work

| Candidate | Concrete build-on technique | Decision / improvement experiment |
|---|---|---|
| Existing RC16 provider-state routing | Keep one primary input/combat authority, readiness gates, explicit user OFF and separate presentation ownership. | Baseline all transition paths before adding more bridges. A narrower, correct integration is preferable to broad untested claims. |
| [epic-tacz](https://github.com/Ardelhite/epic-tacz) | Study specialist firearm takeover/restoration and the upstream bridge's narrowly scoped integration. | Candidate licensed reference for an adapter, not blind replacement. Compare exact provider versions, reload/firearm state, mode restoration and network behavior. |
| [Better Combat API](https://github.com/ZsoltMolnarrr/BetterCombat) | Use documented weapon-attribute data and supported interfaces; leave weapon definitions with their provider. | Validate attribute/readiness handling rather than building a second universal weapon registry. |

**License evidence:** epic-tacz [LICENSE](https://github.com/Ardelhite/epic-tacz/blob/main/LICENSE), blob `d8ae6110a5f8dc82c9c7fd6557c5f1a06386bc14`, is MIT and requires preserving notices. Better Combat [LICENSE](https://github.com/ZsoltMolnarrr/BetterCombat/blob/26.2/LICENSE), blob `05965515e8dbc57587c88dc70d5d9c480d89dd25`, is All Rights Reserved. Its public source is not permission to vendor the combat engine. Audit exact versions, integration guidance and licenses of Epic Fight, TaCZ, YSM and Punchy separately before shipping combined code or assets.

### Shared-work proposals

- JetSetCraft: common ownership-transition scenarios for riding, melee, firearms, first-person hands and reconnect; distinct adapters remain owned by each repo.
- AoA: versioned weapon capability descriptions and original regression fixtures could prevent presentation upgrades from accidentally changing combat semantics.
- Enderloom/Minecraft-Mod-Vault: a provider compatibility receipt identifying exact versions, enabled modes, failures and tested transitions could improve modpack diagnostics. Do not advertise compatibility from static metadata alone.

- [ ] Source-review the exact epic-tacz target branch before adapting any code; preserve MIT notices and retain a minimal failing transition test.
- [ ] Measure idle tick overhead, provider switch latency and correctness on the actual five-provider stack, not only a standalone bridge microbenchmark.

### Reuse admission gates

These are **candidate foundations and experiments**, not selected dependencies or measured improvements. Before reuse: pin the exact version/commit and loader; inspect its source and dependency tree; record code, asset, trademark and redistribution terms separately; keep required notices; verify permission where terms are restrictive or unclear. Public visibility is not a license. A current default-branch license is evidence for that snapshot, not every historical release. Prefer an upstream dependency/API or a small original adapter over a fork when it preserves compatibility. Do not copy proprietary game assets or publish private fixtures.

For every admitted experiment retain baseline and candidate artifacts, representative native workloads, correctness/feature parity, cold/warm latency, p95 frame/tick time, memory, interrupted-save/recovery evidence and rollback. Promote only after the actual target improves without losing requirements. Cross-project sharing below is an interface proposal, not permission to change another project's implementation or declare integration complete.
