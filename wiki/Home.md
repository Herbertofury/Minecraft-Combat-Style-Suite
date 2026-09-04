# ⚔️ Combat Style Suite 5.0.0 RC17

> **Forge 1.20.1 · Java 17 · Vanilla / Better Combat / Epic Fight live selector · YSM / Punchy / TaCZ compatibility**

Combat Style Suite is a client-focused compatibility/orchestration layer for running several major combat and presentation mods together without letting them all fight over the same click, animation or first-person arm.

RC17 is the **Just Works Polish** release: it keeps the proven RC14–RC16 routing/ownership behavior, simplifies the everyday menu, removes the unsafe Better Combat pre-handshake bypass entirely, adds event-only action-bar notices, turns repair into a clear one-shot **Fix Everything Safely** action, and adds CI guards against laggy architectures creeping back in.

## 🚀 Start here

- [[Getting Started]]
- [[RC17 Just Works Polish]]
- [[What It Does]]
- [[Combat Routing]]
- [[Provider Toggles]]
- [[Punchy Pairing Matrix]]
- [[TaCZ and First-Person Ownership]]
- [[Performance and No-Lag Design]]
- [[Testing and Verification]]
- [[Excluded by Design and Source Audit]]

## 🎮 Everyday controls

The default config screen is intentionally small:

| Control | What it does |
|---|---|
| **Vanilla** | Makes vanilla the live primary melee engine |
| **Better Combat** | Selects Better Combat live; safely waits for its real server handshake if needed |
| **Epic Fight** | Selects Epic Fight live |
| **YSM** | Live enable/disable for YSM integration |
| **Punchy** | Live Punchy companion master toggle |
| **Punchy Pairings** | Choose whether Punchy may accompany Vanilla, Better Combat and/or Epic Fight |
| **Provider Toggles** | Fully gate/re-enable installed providers live |
| **Fix Everything Safely** | One-shot reconcile/repair; no watchdog or polling loop |
| **Advanced** | Smart Hybrid, rules, fusion, controller/diagnostic tools and other specialist controls |

### F12 remains intentionally simple

```text
Vanilla → Better Combat → Epic Fight → Vanilla
```

No restart is required for that normal engine cycle.

## 🛡️ Better Combat handshake safety

RC17 physically removes the old unsafe “pretend Better Combat is ready before its real handshake” override.

If Better Combat is selected too early, the Suite temporarily keeps **Vanilla as the effective safe fallback**, shows a small action-bar waiting notice, and activates Better Combat automatically when the real handshake arrives. Your selected mode is preserved; safety is no longer optional.

## ✨ Event-only notices

RC17 only posts a notice when something meaningful changes. Stable gameplay produces no notice work.

There is **no notice tick listener, timer, scheduler, executor, heartbeat, watchdog or render polling loop**.

## 🔧 Fix Everything Safely

This is deliberately one-shot. It reasserts provider/pairing/controller state, invalidates only the relevant routing caches, reapplies the selected stack once, runs the existing Punchy repair once and reports health.

It does not keep checking afterward.

## ⚡ Performance contract

The project intentionally rejects hidden continuous work for compatibility enforcement. RC17's CI guard fails if production code tries to add:

- global `ClientTickEvent`, `ServerTickEvent` or `PlayerTickEvent` enforcement;
- watchdog/heartbeat loops;
- recurring timers/schedulers/executors;
- a full Forge item-registry crawler;
- a full Better Combat weapon-registry crawler;
- the removed unsafe Better Combat bypass;
- recurring transition-notice behavior.

The rare client-only NBT freshness fallback is the documented exception: it stays dormant unless an advanced NBT-sensitive Smart Hybrid/rule needs eventual detection of an in-place NBT mutation.

## 🧠 Smart Hybrid

**OFF by default.** It remains an advanced feature and is not part of F12.

Normal use — F12 engine switching, provider hot toggles, Punchy pairings, YSM and TaCZ handling — does not require Smart Hybrid.

## 🔫 TaCZ

TaCZ temporarily owns firearm presentation while a gun needs it, then the Suite restores the selected melee presentation. This keeps firearm arms/camera/presentation from competing with the melee engine.

## 🖐️ First-person ownership

RC14's single-arm/right-click fix remains part of RC17. When another provider owns first-person rendering, the Suite makes Better Combat yield both relevant first-person hand lanes temporarily, then restores the exact previous values.

The rule is simple:

> **one basic-attack / first-person owner at a time; optional companion layers yield to that owner.**

## ✅ Verification status

RC17 passed clean-source build/harness/static/package verification, including RC13 hard standby, RC14 single-arm ownership, old/new Better Combat restoration routes, RC16 provider/Punchy regressions and the new RC17 no-lag/safe-handshake/simple-menu guards.

A clean extraction rebuilt the Director and Compat JARs byte-for-byte, and deterministic packaging reproduced the install ZIP byte-for-byte.

### Honest remaining boundary

The runner does not currently have the five exact upstream provider binaries together for a fresh native Forge client launch, so the final exact-stack field test remains **Epic Fight + Better Combat + YSM + Punchy + TaCZ** in the real game.

Until that is run, the project is accurately described as source/harness/static/package verified rather than falsely claiming exact-five-provider native visual certification.

---

## 📚 Wiki map

### Use it
- [[Getting Started]]
- [[RC17 Just Works Polish]]
- [[Configuration Reference]]
- [[Provider Toggles]]
- [[Punchy Pairing Matrix]]
- [[Troubleshooting]]

### Understand it
- [[What It Does]]
- [[Combat Routing]]
- [[Smart Hybrid and Rules]]
- [[TaCZ and First-Person Ownership]]
- [[Performance and No-Lag Design]]

### Audit it
- [[Compatibility Matrix]]
- [[Compatibility Research]]
- [[Testing and Verification]]
- [[Excluded by Design and Source Audit]]
- [[Things You May Want to Change]]
- [[References]]
