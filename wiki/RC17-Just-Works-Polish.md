# RC17 — Just Works Polish

**Combat Style Suite 5.0.0 RC17 · Forge 1.20.1 · Java 17**

RC17 is deliberately a polish release, not another combat-system redesign. The proven RC14–RC16 ownership/routing behavior stays intact while the everyday experience gets simpler, safer and harder to regress.

## What changed

### A much simpler default screen

The normal config screen now focuses on the things you actually use while playing:

- **Vanilla**
- **Better Combat**
- **Epic Fight**
- **YSM**
- **Punchy**
- **Punchy Pairings**
- **Provider Toggles**
- **Fix Everything Safely**
- **Advanced**

Smart Hybrid, Epic Systems fusion, item/rule automation, visual ownership tooling, controller orchestration and specialist diagnostics still exist, but they live under **Advanced → Advanced Tools** instead of crowding the everyday page.

## Better Combat is safe by construction

The old unsafe pre-handshake force-enable option has been **physically removed** from RC17 — it is not merely disabled by default.

If Better Combat is selected before its real multiplayer readiness handshake arrives:

1. the Suite remembers that Better Combat is the selected engine;
2. Vanilla remains the effective safe fallback for the moment;
3. the action bar tells you that the Suite is waiting for the handshake;
4. when the real handshake arrives, Better Combat activates live;
5. no restart and no unsafe readiness lie are needed.

## Event-only notices

RC17 adds small action-bar notices when something meaningful actually changes — for example selecting an engine, waiting for Better Combat readiness, changing a provider policy, changing a Punchy pairing, repairing the current stack, or using Emergency Vanilla.

The notice system has **no tick listener, scheduler, timer, executor, render poll, heartbeat or watchdog**. A stable state emits nothing.

## Fix Everything Safely

The old repair wording is now an obvious one-shot button: **Fix Everything Safely**.

When pressed it:

- reasserts provider gates;
- reconciles Punchy policy;
- reconciles controller/provider state;
- invalidates only the relevant cached routing state;
- reapplies the selected stack once;
- runs the existing Punchy repair once;
- reports health.

It does **not** start a background monitor or recurring repair loop.

## Strong regression guard

RC17 adds a repository/CI guard that fails a build if future edits try to reintroduce the architectures this project intentionally excludes, including:

- `ClientTickEvent`, `ServerTickEvent` or `PlayerTickEvent` enforcement;
- heartbeat/watchdog behavior;
- recurring timer/scheduler/executor loops;
- full Forge item-registry crawling;
- full Better Combat weapon-registry crawling;
- an unsafe Better Combat pre-handshake bypass;
- advanced controls leaking back onto the default menu;
- recurring transition-notice behavior.

The existing narrowly scoped client-only NBT freshness fallback remains allowed. It stays dormant unless an advanced NBT-sensitive Smart Hybrid/rule actually needs it.

## What did not change

- F12 is still **Vanilla → Better Combat → Epic Fight → Vanilla**.
- Smart Hybrid is still **OFF by default** and advanced-only.
- YSM, Better Combat and Epic Fight remain live-gated/re-enableable.
- Punchy still has independent Vanilla / Better Combat / Epic Fight pairing permissions.
- TaCZ still takes temporary firearm presentation ownership and restores the selected melee state afterward.
- RC14's right-click/use-item single-arm ownership fix is retained.
- There is still no server heartbeat, watchdog, global tick enforcement or full weapon-registry crawler.

## Verification

RC17 passed the deterministic source/harness/static/package suite, including the RC13 hard-standby checks, RC14 single-arm regression, old/new Better Combat restoration routes, RC16 provider/Punchy matrix regressions, the new RC17 no-lag guard and the RC17 safe-handshake/simple-UI tests.

A fresh extraction of the RC17 source archive rebuilt the Director and Compat JARs byte-for-byte, and the deterministic packager reproduced the RC17 install ZIP byte-for-byte.

### Remaining native boundary

The build runner does not currently contain all five exact upstream provider binaries together, so the final exact-stack native Forge client field test remains:

**Epic Fight + Better Combat + YSM + Punchy + TaCZ**, exercising F12, left/right click, offhand/use-item, live provider toggles, Punchy pairings and TaCZ takeover/restore.

That limitation is explicit; RC17 is not falsely labeled as having completed that exact five-provider native visual test in this runner.

---

**RC17 design goal:** install it, choose the combat style you want, and stop thinking about the compatibility layer unless an actual native bug appears.
