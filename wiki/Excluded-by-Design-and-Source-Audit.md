# 🧱 Excluded by Design — RC16 Source & Binary Audit

> **Purpose:** this page records features and architectures that are deliberately **not present** in the shipped RC16 runtime. It exists so future compatibility work does not slowly turn Combat Style Suite into an always-running scanner, watchdog, or multi-owner combat stack.

**Audited release:** Combat Style Suite 5.0.0 RC16 · Forge 1.20.1  
**Director SHA-256:** `ed71dcc65d02d55fcc708e6ce4b188e05cd063cfb71d1b4a504cce6c20c149c6`  
**Compat SHA-256:** `02bd08e79323fae9fc513ccb2870f0c414f94ca8cf14cff246e0b956d08dd8cd`

---

## ✅ Hard exclusions verified in RC16

| Architecture / feature | RC16 state | Verification |
|---|---:|---|
| Automatic **full weapon-registry crawler** | ❌ **NOT PRESENT** | No item-registry enumeration, `getValues()`, `getEntries()`, registry `stream()` or global weapon sweep in production source |
| Server heartbeat | ❌ **NOT PRESENT** | No heartbeat loop/string/path in production source or compiled classes |
| Runtime watchdog | ❌ **NOT PRESENT** | No watchdog loop/string/path in production source or compiled classes |
| Forge client tick enforcement | ❌ **NOT PRESENT** | No `ClientTickEvent` listener |
| Forge server tick enforcement | ❌ **NOT PRESENT** | No `ServerTickEvent` listener |
| Forge player tick enforcement | ❌ **NOT PRESENT** | No `PlayerTickEvent` listener |
| Fixed-rate scheduler loop | ❌ **NOT PRESENT** | No `scheduleAtFixedRate`, `scheduleWithFixedDelay`, `ScheduledExecutorService`, or `TimerTask` |
| Deep EpicYSM skeleton/transform bridge | ❌ **NOT PRESENT** | No EpicYSM linkage or classes in RC16 |
| Always-on HUD/overlay | ❌ **NOT PRESENT** | No Forge GUI overlay listener or HUD renderer |
| Formal “keep Epic dodge active while TaCZ owns gun presentation” feature | ❌ **NOT PRESENT** | TaCZ takeover restores the selected melee state afterward; no finer retained-Epic capability mode was added |
| Continuous server admin-policy enforcement | ❌ **NOT PRESENT** | RC16 has no policy heartbeat or arena/world enforcement loop |

These are not merely disabled defaults. The listed mechanisms are **absent from the shipped RC16 implementation**.

---

# The registry nuance: lookup ≠ crawler

RC16 **does** reference Forge's item registry for one narrow operation: resolving the ID of the **current item**.

`ItemIdentity` lazily resolves `ForgeRegistries.ITEMS` and calls its `getKey(item)` method for the one item it is identifying. The result is cached by item identity.

That is intentionally different from a registry crawler:

```text
RC16
held item changes / rule needs identity
        ↓
getKey(current item)
        ↓
cache ID
        ↓
stop
```

What RC16 does **not** do:

```text
join world
   ↓
iterate every registered item
   ↓
probe Better Combat / Epic Fight for every weapon
   ↓
build giant compatibility table
   ↓
repeat after config/datapack changes
```

Likewise, Better Combat integration can call `WeaponRegistry.getAttributes(ItemStack)` for the relevant stack when Smart Hybrid/capability routing needs an answer. It does **not** enumerate Better Combat's complete registry.

This narrow lookup model is exactly what we want for the “QOL, it just works, no hidden lag” design.

---

# The one allowed sparse fallback

RC16 contains `DynamicNbtFreshnessObserver`, but it is **not** a Forge tick listener, server heartbeat, watchdog, or permanent worker.

It uses a one-shot `CompletableFuture.delayedExecutor(1 second)` sample and only re-arms while **all** of the following remain true:

1. a player is actually in a client world;
2. Suite runtime integration is enabled;
3. the held stack has dynamic NBT;
4. an advanced capability-sensitive rule or **Smart Hybrid** actually needs freshness;
5. TaCZ is not currently taking over that held gun path where applicable.

Normal F12 use does not require it.

```text
Vanilla / Better Combat / Epic Fight manual mode
                    ↓
           NBT sampler dormant

Smart Hybrid or capability rule + dynamic-NBT held item
                    ↓
        sparse 1-second client sample
                    ↓
    stop automatically when no longer needed
```

It performs **zero server-side recurring work**.

---

# Defaults verified

RC16 production config declares:

| Setting | Default |
|---|---|
| Primary combat engine | **Vanilla** |
| Smart Hybrid | **Not selected** |
| Punchy companion master | **OFF** |
| Punchy allowed with Vanilla | ON |
| Punchy allowed with Better Combat | ON |
| Punchy allowed with Epic Fight | ON |
| Unsafe Better Combat pre-handshake override | **OFF** |
| Pure Standby | OFF |

The three Punchy pairing permissions being ON does **not** activate Punchy by itself; the Punchy master remains OFF until the player enables it.

---

# Better Combat handshake safety

Normal RC16 Better Combat activation requires the real Better Combat server/client enable handshake to have been observed.

The advanced configuration contains an explicitly named **unsafe override**, but its default is `false`. That option was **not** one of the architectures marked “DO NOT ADD”; the recommendation is to leave it OFF. Normal F12/provider behavior does not use it.

If the goal later becomes “make unsafe pre-handshake enabling impossible even by advanced opt-in,” that would be a separate deliberate removal—not something RC16 currently does silently.

---

# Threads: shutdown cleanup only

The only direct `new Thread(...)` production references found are JVM **shutdown hooks**:

- `combat-style-director-shutdown`
- `PunchyStyleCompat-Restore`

Those execute during JVM shutdown to restore/release managed state. They are not gameplay loops or background workers.

---

# Freeze rule for future versions

A future change should fail review if it introduces any of the following without a concrete native reproducer and an explicit decision to change this policy:

- full registry enumeration for automatic combat discovery;
- per-tick ownership enforcement;
- server heartbeat/watchdog polling;
- permanent background worker threads;
- simultaneous hidden basic-attack owners;
- automatic Smart Hybrid startup;
- deep EpicYSM transforms “just in case”;
- always-on diagnostics/HUD that performs recurring compatibility scans.

Prefer this order instead:

1. lifecycle/event callback;
2. held-item transition;
3. cached exact-item lookup;
4. provider API query for the **current** item/state;
5. rare scoped fallback only when no reliable event exists.

---

## Bottom line

**The two things explicitly marked DO NOT ADD — a global weapon-registry crawler and server heartbeat/watchdog behavior — are not in RC16.**

The additional speculative features discussed during the audit (deep EpicYSM blending, retained Epic dodge during TaCZ takeover, always-on HUD, continuous server policy enforcement) are also not present.

RC16 remains centered on **transition-driven ownership, narrow cached lookups, and one combat owner at a time**.

← [[Things You May Want to Change]] · [[Performance and No-Lag Design]] · [[Testing and Verification]]
