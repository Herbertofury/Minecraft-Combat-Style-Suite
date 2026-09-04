# 🧩 Compatibility Matrix

RC16 distinguishes **installed**, **eligible**, **primary owner**, and **presentation companion**. “Compatible” does not mean every provider should be simultaneously authoritative.

## High-level matrix

| Combination | RC16 policy | Live? | Notes |
|---|---|---:|---|
| Vanilla + Better Combat | Exclusive primary selection | Yes | F12 chooses one owner |
| Vanilla + Epic Fight | Exclusive primary selection | Yes | F12 chooses one owner |
| Better Combat + Epic Fight | Exclusive primary basic-attack owner | Yes | Advanced Epic Systems fusion is separate/explicit |
| Vanilla + Punchy | Optional companion pairing | Yes | Requires Punchy master + Vanilla permission |
| Better Combat + Punchy | Optional companion pairing | Yes | Requires Punchy master + Better permission |
| Epic Fight + Punchy | Optional companion pairing | Yes | Requires Punchy master + Epic permission |
| YSM + Vanilla | Presentation compatibility | Yes | YSM does not become attack owner |
| YSM + Better Combat | Presentation compatibility | Yes | Renderer ownership must remain coherent |
| YSM + Epic Fight | Presentation compatibility | Yes | Model/animation layer reconciled separately |
| TaCZ + Vanilla | Temporary firearm takeover | Automatic | Restore Vanilla after gun state |
| TaCZ + Better Combat | Temporary firearm takeover | Automatic | Better melee ownership parked/restored |
| TaCZ + Epic Fight | Temporary firearm takeover | Automatic | Epic melee presentation yields/restores |
| TaCZ + Punchy | Gun state wins where necessary | Automatic | Prevent first-person competition |
| TaCZ + YSM | Model/gun presentation coexistence | Automatic/Provider-dependent | YSM remains presentation layer, not firearm owner |

---

## Better Combat + Epic Fight

This is the most important pair to treat conservatively.

Both systems can influence:

- attack input;
- attack timing/range;
- weapon animation;
- hand rendering;
- wield behavior;
- third-person pose;
- weapon compatibility classification.

RC16 therefore does **not** solve the pair by pretending both are equally primary at once.

Normal state:

```text
Primary = Better Combat  → Epic Fight basic attack authority yields
Primary = Epic Fight     → Better Combat basic attack authority yields
Primary = Vanilla        → both external basic attack authorities yield
```

That aligns with Better Combat's own integration guidance: overlapping attack/model/input semantics should be removed or disabled by the integration instead of double-running.

---

## Punchy pairings

Punchy is a presentation companion with a user-controlled permission per primary owner.

That gives a useful escape hatch if one specific engine conflicts in a particular modpack:

```text
Punchy + Vanilla       ON
Punchy + Better Combat OFF  ← disable only the bad pair
Punchy + Epic Fight    ON
```

No need to remove Punchy globally.

---

## YSM

YSM is kept out of primary attack arbitration.

That is important because model/render compatibility is a different problem from deciding who owns left-click gameplay semantics.

The Suite may suppress or restore YSM presentation, but the F12 combat owner remains Vanilla / Better Combat / Epic Fight.

---

## TaCZ

TaCZ gets temporary specialist ownership when a firearm state demands it. This is an intentional preemption rather than a fourth permanent melee engine.

A good firearm compatibility pass must preserve:

- ADS;
- reload;
- gun-specific handling;
- first-person exclusivity;
- third-person gun pose where applicable;
- restoration of the previous melee state afterward.

---

## Optional provider absent

RC16's optional-provider bridges are designed so an absent provider does not become a hard class-link crash path.

The Suite should:

- detect availability;
- avoid provider-specific routing when absent/disabled;
- fall back safely when the selected provider cannot own the requested state;
- keep unrelated providers usable.

---

## Exact target versions used for RC16 compatibility work

RC16's development target lineage was built around these Forge 1.20.1-era providers:

| Provider | Target used by the project |
|---|---|
| Epic Fight | `20.14.17-mc1.20.1-forge` |
| Better Combat | `1.9.0+1.20.1-forge` |
| Yes Steve Model | `2.6.5-forge+mc1.20.1` |
| Punchy | `2.7d` Forge 1.20.1 |
| TaCZ | `1.20.1-1.1.8-hotfix` |

These are **compatibility targets**, not a claim that the Suite redistributes or owns those projects.

---

## Verification boundary

The Suite's source/harness/static/package regressions cover the five-way policy and known first-person ownership bug. The current runner did **not** complete a fresh native Minecraft client launch with all five exact upstream provider JARs simultaneously available.

Therefore the final field gate remains:

> launch the real Forge 1.20.1 pack with the exact providers, exercise the matrix above, and capture any native-only renderer/input conflict without rewriting already-passing ownership logic first.

See **[Testing & Verification](Testing-and-Verification)**.
