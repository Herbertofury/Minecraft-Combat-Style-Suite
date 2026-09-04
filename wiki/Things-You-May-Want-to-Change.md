# 🛠️ Things You May Want to Change — RC17

RC17 intentionally closes most of the “should we keep tweaking this?” questions. The normal compatibility architecture is now considered **frozen unless the real five-provider stack reproduces a concrete bug**.

## At-a-glance

| Decision | RC17 | Recommendation |
|---|---:|---|
| Startup in Vanilla | Yes | **KEEP** |
| F12 = Vanilla / Better Combat / Epic Fight | Yes | **KEEP** |
| Smart Hybrid off by default | Yes | **KEEP** |
| Smart Hybrid hidden behind Advanced | Yes | **KEEP** |
| YSM live gate | Yes | **KEEP** |
| Punchy master + per-engine pairings | Yes | **KEEP** |
| Better Combat unsafe pre-handshake bypass | **Removed entirely** | **KEEP REMOVED** |
| Safe Vanilla fallback while waiting for Better handshake | Yes | **KEEP** |
| Event-only action-bar notices | Yes | **KEEP** |
| Fix Everything Safely | One-shot only | **KEEP** |
| RC14 single-arm/right-click ownership fix | Yes | **KEEP** |
| TaCZ temporary firearm takeover/restore | Yes | **KEEP** |
| Full weapon-registry crawler | No | **DO NOT ADD** |
| Client/server/player tick enforcement | No | **DO NOT ADD** |
| Server heartbeat/watchdog | No | **DO NOT ADD** |
| Recurring repair loop | No | **DO NOT ADD** |
| Always-on ownership HUD | No | **DO NOT ADD by default** |
| Deep Epic/YSM skeleton bridge | No | **Only add for a real native defect** |
| Preserve experimental Epic dodge underneath TaCZ gun ownership | Not a formal feature | **Native-test first** |

## What RC17 deliberately improved

### Simpler default menu

The main page is now for everyday use only. Advanced routing/rule/fusion/controller tools still exist, but they no longer make the normal experience look like a cockpit.

That separation should remain. If a feature is not something you expect to touch during ordinary play, it belongs under **Advanced**.

### Better Combat safety is no longer optional

The old pre-handshake force-enable option is gone from production source/config/UI.

If Better Combat is selected before readiness is observed, RC17 keeps the selection but temporarily uses Vanilla as the effective safe engine. When the real handshake arrives, Better Combat activates live.

There is nothing left for a user to accidentally enable here.

### Notices do not become a monitoring system

Action-bar notices are emitted only for actual transitions. Stable gameplay emits nothing. Do not turn them into an always-visible HUD, timer, tick listener or polling service just to display state continuously.

### Repair stays one-shot

**Fix Everything Safely** exists to reconcile the current state when the user asks. It must not evolve into a watchdog that continuously “fixes” the game behind the player's back.

## The one unusual recurring fallback that remains

The advanced NBT-sensitive rule path retains the documented client-only freshness fallback for in-place NBT mutation that Forge cannot universally signal.

It remains acceptable because it is:

- client-only;
- dormant for normal F12/provider/Punchy/TaCZ use;
- armed only by an advanced NBT-sensitive rule;
- not a server heartbeat, watchdog or global combat-enforcement tick loop.

Smart Hybrid remains **OFF by default**, so normal users never need this path.

## Things I would still consider later

### 1. Exact native five-provider certification

This is the most valuable remaining work, because it adds evidence instead of complexity:

- Epic Fight
- Better Combat
- YSM
- Punchy
- TaCZ

Test F12, left/right click, offhand/use-item, YSM/Punchy/provider toggles and TaCZ takeover/restore in the actual packaged Forge client.

If it is green, freeze the compatibility core.

### 2. Tiny diagnostics polish after a real complaint

A better explanation string or Doctor report is low risk because it can remain on-demand/event-only.

### 3. Deeper pairwise renderer integration only for a reproduced defect

Do not copy an Epic/YSM or Epic/TaCZ bridge simply because it exists. Add deeper transforms/pose coexistence only when the exact modpack shows a visible problem that high-level ownership cannot solve.

## What should not be added

The RC17 CI guard intentionally rejects the architectural regressions most likely to turn a lightweight compatibility layer into background overhead:

```text
no ClientTickEvent combat enforcement
no ServerTickEvent combat enforcement
no PlayerTickEvent combat enforcement
no heartbeat/watchdog
no recurring repair service
no full Forge item-registry crawler
no full Better Combat weapon-registry crawler
no unsafe Better Combat readiness bypass
no recurring transition-notice system
```

That is now a build policy, not merely a recommendation.

## Preferred frozen baseline

```text
Vanilla default
F12 manual three-engine cycle
simple everyday UI
Smart Hybrid advanced + off by default
live provider gates
Punchy companion matrix
YSM presentation arbitration
TaCZ firearm takeover/restore
real Better Combat handshake safety
single first-person owner
event/transition-driven routing
one-shot repair
CI performance regression guard
```

This is the version I would stop redesigning and start **using**.
