---
name: provider-acceptance
description: Validate Combat Style Suite provider ownership, OFF/readiness boundaries, optional integrations, and native RC17 acceptance without confusing static evidence with gameplay proof.
---

# Combat provider acceptance

Use when modifying combat routing, provider toggles, pairing rules, source recovery or release acceptance in this repository. This is project-local guidance, not part of the synced Foundry core.

## Establish the test subject

1. Read `RC17-PUBLICATION.md`, `wiki/Provider-Toggles.md`, `wiki/Combat-Routing.md`, `wiki/Punchy-Pairing-Matrix.md` and `wiki/Compatibility-Matrix.md` from the repository root.
2. Record commit, source archive hash, Forge/Minecraft/Java versions, provider filenames and SHA-256 digests, client/server placement and test profile. Preserve the original mods, configs and saves; use an isolated test profile.
3. Main currently documents frozen RC17 but is not an unpacked buildable source tree. Resolve the canonical source from the publication provenance and verify its recorded digest before extraction. Inspect archive paths before extracting into a new directory; reject traversal or unexpected links. Do not trigger bootstrap/publication workflows as a source-inspection shortcut.
4. Inspect the recovered source's own instructions and scripts before running them. Only then use its actual build and regression commands. Do not infer that documentation mentioning a passing historical harness means the current change passes.

## Required transition matrix

Capture requested mode, effective primary owner, provider enabled/readiness state, visible companion state and observed restoration for each case. A log alone cannot prove visible rendering or input ownership.

| Sequence | Required observation |
| --- | --- |
| Select Better Combat before handshake, then receive handshake | Vanilla fallback before readiness; live activation after readiness, without unsafe forcing |
| Better Combat OFF, then late handshake | Readiness can update, OFF remains authoritative; no surprise activation |
| Better Combat ON after readiness | Reconcile the selected setup once; no repeated repair loop |
| Epic Fight OFF, then external provider transition | OFF remains respected; no dual primary attack/input owners |
| YSM OFF, then external model/config event | Self-model remains disengaged; ON restores intended presentation |
| Punchy master/pairing permission combinations | Render only if master and selected pairing both permit it |
| TaCZ gun equip, unequip, then provider OFF | Takeover and restore prior owner correctly; OFF disables Suite-specific handling |
| Direct primary quick-profile selection after fusion | Clear accidental hidden fusion; explicit advanced choice remains separate |
| Smart Hybrid disabled or integration absent | Ordinary combat and companion features still function; default remains OFF |
| Fix Everything Safely twice | Each invocation is one-shot and idempotent; no timer/watchdog starts |
| Reconnect/reload, repeated toggles, interrupted transition | Preserve intent and readiness boundaries; no leaked ownership or duplicate notices |

Test each optional provider absent as well as present where relevant. An absent provider must not crash unrelated routing. Compare idle and transition behavior: investigate recurring tick enforcement, scheduler/watchdog/heartbeat loops, full item-registry crawling or repeated action-bar notices instead of masking them.

## Evidence and release decision

- Keep a result ledger with case, exact versions, expected/actual outcome, artifact/log/screenshot location and pass/fail/not-run status. Retain failures, not just final successes.
- Static guards and deterministic harness tests establish only their tested contracts. Run the fresh rendered Forge client with the exact Epic Fight + Better Combat + YSM + Punchy + TaCZ combination before claiming that native integration passes.
- For multiplayer readiness behavior, identify the actual client/server topology; a single-player trace is not substitute evidence for a remote handshake.
- Report source/build/static/package/native acceptance separately. If source or native environment is unavailable, leave those gates explicitly blocked and continue safe documentation or harness work.
- No provider upgrades, asset redistribution, release replacement or frozen hash changes merely to make acceptance green. Preserve rollback to the known artifact/config set and explain any deliberate compatibility change.
