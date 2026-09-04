# 🔬 Compatibility Research

**Research snapshot:** 2026-09-03  
**Target context:** Forge 1.20.1 Combat Style Suite RC16  
**Goal:** compare real compatibility approaches before changing RC16 again.

This page deliberately labels evidence depth. A public source review is stronger evidence than a closed-source project description.

## Evidence legend

| Grade | Meaning |
|---|---|
| **A — source reviewed** | Public source/code or official integration source was inspected |
| **B — official docs reviewed** | Author-maintained docs/release notes/config behavior reviewed |
| **C — project-page audit** | Public CurseForge/Modrinth description/release behavior reviewed; source not available/confirmed |
| **D — locator only** | Project identified as relevant but not used for a design conclusion |

---

# 1. Better Combat — the most important upstream integration reference

**Evidence:** A/B  
**Source:** https://github.com/ZsoltMolnarrr/BetterCombat  
**Project:** https://modrinth.com/mod/better-combat

Better Combat is the strongest reference for what a compatibility bridge should *not* overlap semantically.

## Relevant upstream design points

Better Combat exposes weapon attributes/configuration rather than requiring every integration to hardcode animation ownership. Its compatibility guidance historically distinguishes dedicated weapon attributes from fallback compatibility configuration.

Important categories an integration must avoid double-owning include:

- attack input;
- attack timing/range/cooldown semantics;
- attack animation/model state;
- attack sound behavior;
- mining/attack interaction;
- dual-wield behavior;
- custom wield appearance.

Better Combat also has multiplayer-aware synchronization/readiness behavior. That makes “just force a local enabled flag before the normal handshake” a bad compatibility strategy.

## What RC16 does because of this

- one primary attack owner at a time;
- Better Combat handshake/readiness latch;
- `allowUnsafeBetterCombatForceEnable=false` by default;
- Better Combat OFF remains OFF even if readiness information arrives later;
- Better Combat first-person arm lanes are explicitly yielded when another renderer owns first person;
- Smart Hybrid does not replace Better Combat's own weapon-attribute system with a giant duplicate registry.

**Conclusion:** RC16's architecture matches the strongest upstream guidance; no ownership rewrite indicated.

---

# 2. Epic Fight — competing combat authority must be explicit

**Evidence:** B + upstream behavior/issue review  
**Project:** https://www.curseforge.com/minecraft/mc-mods/epic-fight-mod

Epic Fight is not merely a weapon animation pack; it has its own combat mode/state and player patch behavior. Running it as an equal simultaneous basic-attack owner beside Better Combat creates predictable conflicts.

Public issue history in the Epic Fight ecosystem includes examples where an external combat animation can visually override Epic Fight's expected two-handed presentation while Epic gameplay state remains active. The important lesson is architectural:

> Visual survival of two combat mods at once does not prove coherent gameplay ownership.

## What RC16 does because of this

- F12 selects **Epic Fight or Better Combat**, not “both and hope”;
- direct quick selection clears accidental hidden Epic Systems fusion;
- advanced fusion is a separate deliberate opt-in;
- Epic Fight live OFF forces primary ownership away from Epic instead of only hiding a bridge control.

**Conclusion:** keep the explicit authority transfer.

---

# 3. Yes Steve Model (YSM)

**Evidence:** B  
**Official docs:** https://ysm.cfpa.team/  
**Project:** https://modrinth.com/mod/yes-steve-model

YSM is primarily a model/render/animation presentation system, not the primary melee gameplay engine. Recent 2.6.x documentation/release behavior emphasizes renderer scheduling/performance/stability and version-aware linked-mod features.

The YSM ecosystem also exposes settings intended to disable linked-mod behavior when a combination is risky or incompatible.

## What RC16 does because of this

- YSM is modeled as a presentation companion, not an F12 primary engine;
- a dedicated live YSM gate exists;
- OFF guards against a later YSM config/state event silently re-showing the self model;
- ON restores/reconciles prior intended presentation;
- YSM compatibility renderer behavior remains separately configurable.

**Conclusion:** keeping YSM separate from basic attack arbitration is correct.

---

# 4. EpicYSM

**Evidence:** C/B project documentation  
**Project locator:** search CurseForge/Modrinth for **EpicYSM** (current releases are primarily newer NeoForge-era targets).

EpicYSM is a useful reference because it specializes in the difficult part of Epic Fight + YSM: model/animation presentation coordination rather than trying to create another combat engine.

Project documentation describes features around:

- Epic Fight ↔ YSM model/animation coordination;
- model switching;
- third-person transforms;
- held-item renderer behavior;
- armor/overlay compatibility;
- animation blending/physics settings;
- diagnostics.

## RC16 comparison

RC16 already owns the **high-level provider/presentation arbitration**. EpicYSM demonstrates that deeper skeleton/animation blending is a separate specialized problem.

### Possible future feature

If the exact 1.20.1 native stack shows YSM/Epic visual defects, an **EpicYSM-style deeper presentation bridge** could be considered.

I would *not* add that proactively. It raises native-model/skeleton/animation complexity and should be driven by a reproducible visual defect.

**Conclusion:** valuable future reference; not evidence that RC16 needs another combat-routing rewrite.

---

# 5. Epic Fight & Better Combat x Punchy!

**Evidence:** C — project-page audit; public source not confirmed during this audit  
**Target relevance:** Forge 1.20.1  
**Locator:** CurseForge search for **Epic Fight & Better Combat x Punchy!**

The project description's central technique is exactly the category RC16 cares about: **Punchy rendering yields while Epic Fight battle state or Better Combat swinging needs ownership.**

## RC16 comparison

The pairwise bridge hardens a particular relationship. RC16 generalizes the same concept into:

- Punchy master;
- Punchy + Vanilla permission;
- Punchy + Better permission;
- Punchy + Epic permission;
- one primary combat authority underneath.

**Conclusion:** RC16's Punchy matrix is a broader, user-configurable version of the same ownership principle.

---

# 6. Tijōn's Epic Punchy

**Evidence:** C — project-page audit  
**Target relevance:** Forge 1.20.1  
**Locator:** CurseForge search for **Tijōn's Epic Punchy**

Project documentation describes intelligent switching between Punchy first-person presentation and Epic Fight third-person/combat presentation, plus firearm-aware compatibility including TaCZ-like gun handling.

## RC16 comparison

This reinforces two RC16 decisions:

1. Punchy should **yield contextually**, not remain a blind always-on renderer.
2. firearm ownership should be treated separately from melee presentation.

Tijōn's project also makes firearm-aware behavior worth keeping in our native regression matrix.

**Conclusion:** supports the Punchy companion + TaCZ specialist-owner model.

---

# 7. Epic Fight × TaCZ — `Ardelhite/epic-tacz`

**Evidence:** A — public source reviewed  
**Source:** https://github.com/Ardelhite/epic-tacz

This is one of the best concrete source references in the audit.

## Source behavior relevant to us

The bridge detects TaCZ gun usage and temporarily makes Epic Fight yield its battle-mode presentation, then restores non-gun behavior afterward. It also integrates TaCZ third-person animation handling and newer project behavior discusses preserving useful Epic movement/dodge behavior while firearm presentation is active.

Its implementation uses a client-tick path for its narrow pairwise problem.

## RC16 comparison

Same ownership idea, different orchestration scope:

- **epic-tacz:** narrow Epic ↔ TaCZ bridge, client tick is acceptable to that design.
- **Combat Style Suite:** five-provider router, so copying a global every-tick enforcement model would multiply unnecessary work.

RC16 instead tries to make temporary gun takeover/restoration transition-driven/cached wherever possible.

### Potential future enhancement

Preserving safe Epic Fight **dodge/movement** while TaCZ suppresses melee battle presentation is worth native testing.

Do not enable it by assumption; prove which Epic subsystems are safe to retain without reclaiming attack/pose ownership.

**Conclusion:** RC16 matches the core ownership transfer; one optional movement-preservation enhancement is worth future field testing.

---

# 8. Better Combat × Epic Fight compatibility bridges

**Evidence:** C/B public project/release-note audit  
**Locator:** CurseForge search for **Better Combat x Epic Fight Compat**

Current compatibility projects in this space automate selection/registration between Better Combat and Epic Fight weapon handling.

One useful performance lesson from public release notes in this category is that broad weapon registration done as one large join-time operation can create noticeable hitching; maintainers have moved such work out of one monolithic spike and distributed it to reduce stutter.

## RC16 comparison

RC16 should **not** respond by adding its own universal weapon-registry worker.

Its current approach is safer for a QOL compatibility layer:

- prefer upstream weapon attributes/config;
- inspect only what enabled rules need;
- cache classification;
- avoid background full-registry rediscovery.

**Conclusion:** validates RC16's avoidance of a giant automatic registry scan.

---

# 9. Server Better Combat / toggle-command projects

**Evidence:** C  
**Locator:** CurseForge search for **Server Better Combat and ParCool Toggle Commands**

There are server-side utility mods exposing live enable/disable/toggle commands for Better Combat-related behavior. They are useful precedent that runtime control is a real user need, but they do not solve the full first-person/YSM/Punchy/TaCZ ownership problem.

## RC16 comparison

RC16's live Better Combat gate is more than a command toggle:

- it has provider readiness awareness;
- it coordinates Epic Fight ownership;
- it coordinates Punchy pairing;
- it coordinates first-person arm yielding;
- it preserves the user's OFF choice across later provider events.

**Conclusion:** live toggling is a valid QOL goal; RC16 needs the broader arbitration around it.

---

# 10. Smaller Better Combat compatibility packs/addons

**Evidence:** C/D

The ecosystem contains many content-specific Better Combat compatibility packs (weapon packs, gun/melee content, animation packs). Their strongest general lesson is that **data-level weapon compatibility belongs as close as possible to the weapon/provider**, not in a universal runtime bridge.

RC16 should not absorb every weapon's attributes into Java code.

**Conclusion:** keep weapon data upstream/data-driven; keep RC16 focused on runtime ownership.

---

# Cross-project findings

After comparing these approaches, the recurring successful principles are:

| Principle | Seen in ecosystem | RC16 status |
|---|---:|---:|
| One combat/input owner should yield when another owns the state | Yes | ✅ Implemented |
| First-person render ownership needs explicit arbitration | Yes | ✅ Implemented |
| Punchy should yield conditionally | Yes | ✅ Pairing matrix |
| Gun state should preempt melee presentation | Yes | ✅ TaCZ passthrough |
| Provider readiness/handshake should be respected | Better Combat | ✅ Implemented |
| Model presentation should be separated from attack semantics | YSM/EpicYSM | ✅ Implemented |
| Avoid monolithic registry work/hitching | Compat bridges | ✅ Conservative RC16 design |
| Avoid global per-tick enforcement when a state transition can solve it | Mixed ecosystem | ✅ RC16 design target |
| Deeper YSM/Epic skeleton blending may need a dedicated bridge | EpicYSM | ⏳ Only if native defect appears |
| Preserve safe Epic movement/dodge during gun takeover | epic-tacz reference | 🧪 Worth field-testing |

---

# Audit decision

## Keep RC16 as-is

I do **not** see evidence that the central RC16 routing architecture should be changed again before native field testing.

The survey supports:

- F12 exclusive primary ownership;
- hard live provider gates;
- Punchy as an independent companion matrix;
- YSM as presentation, not attack owner;
- TaCZ temporary specialist ownership;
- conservative Better Combat readiness handling;
- transition/cached performance model.

## Only two areas worth considering later

1. **EpicYSM-style deeper model/skeleton presentation compatibility** — only if the real 1.20.1 stack shows a specific YSM/Epic visual defect.
2. **Epic movement/dodge retention during TaCZ takeover** — only after a native test proves it can remain without stealing attack/pose ownership.

Everything else should be driven by a real reproducer, not another speculative rewrite.
