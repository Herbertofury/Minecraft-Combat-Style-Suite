# ⚙️ Configuration Reference

This page documents the major RC16 behavior defaults in human terms. Exact option names may be grouped by screen/config section, but these are the policies that matter when deciding whether to change the mod.

## Safe baseline

| Area | RC16 default | Recommendation |
|---|---:|---|
| Selected/startup engine | **Vanilla** | Keep |
| Remember selected style | On | Keep unless you want every session to reset |
| Enforce selected style | On | Keep |
| Fallback when unavailable | On | Keep |
| Verify ownership | On | Keep |
| Cycle installed providers only | On | Keep |
| Live/direct mode keys | On | Personal preference |
| Controller support | On | Keep if used |
| Combat rules | On/available | Fine; unused rules are not a reason to disable core routing |
| Multiplayer presentation sync | On | Keep for multiplayer |
| Unsafe Better Combat force-enable | **Off** | **Keep OFF** |
| Auto YSM compatibility renderer | On | Keep unless a native model conflict proves otherwise |
| Smart Hybrid selected | **No** | Keep manual by default |
| Hybrid dual-capability preference | Epic Fight | Only matters in Smart Hybrid |
| Pure Standby | **Off** | Keep OFF for normal play |

---

## Provider integration gates

| Provider | Default gate | Meaning |
|---|---:|---|
| Epic Fight | On | Suite may integrate when selected/needed |
| Better Combat | On | Suite may integrate when safely ready |
| YSM | On | Suite may manage YSM presentation compatibility |
| Punchy | On | Suite may use Punchy if Punchy master/pairing permits |
| TaCZ | On | Suite may perform TaCZ gun ownership passthrough |

A gate being ON does **not** mean the provider is constantly active or constantly polled. It means the integration is eligible when a transition requires it.

---

## Primary combat selection

### Vanilla

Recommended startup state. Lowest ambiguity and makes a safe fallback when an external provider is absent.

### Better Combat

Use when you want Better Combat to own normal melee/basic attacks. Normal handshake/readiness rules still apply.

### Epic Fight

Use when you want Epic Fight to own normal melee/basic attacks.

### Smart Hybrid

Advanced only. Not part of F12. Leave off until you have explicit routing rules worth automating.

---

## Punchy

| Setting | Default |
|---|---:|
| Punchy master | **Off** |
| Punchy with Vanilla | On |
| Punchy with Better Combat | On |
| Punchy with Epic Fight | On |

Why this combination?

The permissions describe what is *allowed if Punchy is enabled*. Keeping the master OFF prevents an unexpected first-person presentation change on first install.

Recommended first change if you like Punchy: turn **Punchy master ON** and leave all three permissions ON. Disable only a pairing that demonstrates a native conflict in your pack.

---

## YSM

Default layer behavior is **Auto**.

Recommended:

- keep YSM integration gate ON if YSM is installed;
- let the Suite reconcile it automatically;
- use the live OFF toggle rather than deleting config if you want to temporarily remove YSM presentation;
- only disable the auto compatibility renderer after reproducing a specific model/render conflict.

---

## Better Combat safety

`allowUnsafeBetterCombatForceEnable=false` is a deliberate default.

Better Combat synchronizes gameplay-relevant data/config in multiplayer. RC16 should not claim readiness by brute-forcing a local state before the provider has completed the normal relationship it expects.

Change this only for a proven, version-specific reason with a native regression test.

---

## Ownership verification

Keep ownership verification ON.

Its job is to make a state transition truthful. If a provider write fails or an expected state cannot be established, the Suite should fail/fallback safely rather than leave two providers believing they both own the same lane.

This is not intended as a continuous server/client watchdog.

---

## Rules

Rules are useful for automatic per-item routing but optional for manual players.

Recommended progression:

1. Use F12 manually first.
2. Add exact-item rules only if repetitive switching becomes annoying.
3. Add namespace rules if an entire mod's weapons consistently belong to one engine.
4. Use capability-based Smart Hybrid rules last, when their automatic behavior actually provides value.

This keeps the cheapest and most deterministic matching method first.

---

## Multiplayer presentation sync

Keep enabled when playing with other players who need to see the selected presentation state.

RC16's design goal is to synchronize **changes**, coalesce bursts, and avoid heartbeat-style repetitive traffic.

For a purely single-player profile you may choose to disable multiplayer sync if the config exposes that option, but there should be no need to do so for performance in a normal server setup.

---

## Pure Standby

Pure Standby is for users who want the Suite installed/configurable but want its runtime integration hooks omitted on the next startup.

- Default: OFF
- Requires restart to physically change startup hook composition
- Not required to obtain low-overhead normal play

Do not use it as an everyday substitute for live provider gates.

---

## My recommended “cozy pack / QOL” profile

```text
Startup engine: Vanilla
F12 quick selector: enabled
Smart Hybrid: OFF
Provider gates: ON for installed providers
Unsafe Better force-enable: OFF
Ownership verification: ON
Fallback when unavailable: ON
YSM: Auto
Punchy master: ON only if you personally want Punchy
Punchy pairings: all ON initially
TaCZ passthrough: ON if TaCZ is installed
Multiplayer presentation sync: ON
Pure Standby: OFF
```

Then change only the one pairing/provider that demonstrates a real conflict.
