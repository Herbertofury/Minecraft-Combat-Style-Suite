# 🛠️ Things You May Want to Change

This page is intentionally opinionated. It separates **good candidates for user preference** from **things I would not touch unless a real native reproducer demands it**.

## At-a-glance recommendations

| Decision | RC16 | My recommendation |
|---|---|---|
| Startup in Vanilla | Yes | **KEEP** |
| Smart Hybrid off by default | Yes | **KEEP** |
| F12 only Vanilla / Better / Epic | Yes | **KEEP** |
| Punchy master off, pairings pre-authorized | Yes | **KEEP**, or turn master on for your personal pack |
| All installed-provider integration gates available | Yes | **KEEP** |
| Better Combat unsafe force-enable off | Yes | **KEEP** |
| Ownership verification on | Yes | **KEEP** |
| TaCZ temporary takeover | Yes | **KEEP** |
| One-second NBT fallback only for advanced NBT rules | Yes | **KEEP** |
| Pure Standby restart-hard | Yes | **KEEP** |
| Deep Epic/YSM skeleton blending | Not generalized | **ONLY ADD IF NATIVE DEFECT APPEARS** |
| Preserve Epic dodge while TaCZ owns gun presentation | Not guaranteed as a formal RC16 feature | **WORTH TESTING LATER** |
| Automatic full weapon-registry crawling | No | **DO NOT ADD** |
| Server heartbeat/watchdog | No | **DO NOT ADD** |

---

# 1. Startup engine: Vanilla

### Current

RC16 starts from Vanilla as the safe baseline.

### Why keep it

- guaranteed available;
- easiest state to reason about after adding/removing mods;
- avoids claiming Better Combat readiness before handshake;
- avoids entering Epic battle state automatically;
- clean fallback if an optional provider is missing.

### When to change

Only if you personally want every new install/config to boot directly into one external engine and are comfortable with its readiness behavior.

**Recommendation: KEEP.**

---

# 2. Smart Hybrid OFF by default

### Current

Smart Hybrid is advanced and not part of F12.

### Why keep it

- manual F12 is predictable;
- no surprise weapon-triggered mode switching;
- capability probes stay out of normal play;
- the NBT freshness sampler remains dormant;
- simpler native debugging.

### When to enable

When your pack has large weapon families that clearly belong to different combat engines and manual switching becomes annoying.

**Recommendation: KEEP OFF by default.**

---

# 3. F12 only cycles Vanilla / Better Combat / Epic Fight

### Current

```text
Vanilla → Better Combat → Epic Fight → Vanilla
```

### Alternative

Insert Smart Hybrid or other fusion presets into F12.

### Why I would not

F12 should be muscle-memory predictable. Advanced modes already have other controls/menu access. Adding Hybrid makes accidental automatic routing much easier.

**Recommendation: KEEP.**

---

# 4. Punchy default

### Current

- master OFF;
- all three primary pairings allowed.

### Option A — keep current

Best general distribution default: installing the Suite does not suddenly change first-person presentation.

### Option B — your personal modpack

If Punchy is absolutely part of the desired visual identity, you could ship your **pack config** with Punchy master ON while leaving the mod's universal default OFF.

That is better than changing the mod-wide default for every user.

**Recommendation: KEEP code default; customize your pack config if desired.**

---

# 5. Provider gates ON by default

### Misleading interpretation

“Every gate ON means five mods are being polled all the time.”

### Actual meaning

The integrations are eligible. The selected owner and dirty context decide when work is required.

Turning a gate OFF is useful when you deliberately want the Suite to stop using that provider, not as a routine performance tweak.

**Recommendation: KEEP for installed providers.**

---

# 6. Better Combat unsafe force-enable

### Current

OFF.

### Why

Better Combat has multiplayer-aware readiness/synchronization. Lying to the local client about readiness is more dangerous than briefly falling back safely.

**Recommendation: DO NOT CHANGE** without a version-specific native reproducer and test.

---

# 7. One-second NBT freshness sampler

### Current

Dormant unless an advanced NBT-sensitive Smart Hybrid/capability rule needs eventual detection of in-place NBT mutation.

### Options

- remove it entirely;
- make it faster;
- make it slower;
- keep current scoped behavior.

### My choice

Keep it. A roughly one-second client-only fallback is a good tradeoff for a rare event Forge cannot universally signal. Removing it could leave automatic routing stale; making it faster provides little value for a non-animation-timing decision.

**Recommendation: KEEP.**

---

# 8. Epic Systems fusion

### Current

Advanced/explicit, not silently retained when a direct primary quick profile is chosen.

### Could change

You could make fusion “sticky” across F12 changes.

### Why I would not

That makes the visible primary mode lie by omission. If the UI says Better Combat but a hidden Epic layer remains active, diagnosing ownership becomes much harder.

**Recommendation: KEEP explicit.**

---

# 9. TaCZ + Epic Fight movement/dodge

### Current

RC16's important guaranteed behavior is **firearm presentation takeover + restoration of selected melee state**.

### Interesting reference

`Ardelhite/epic-tacz` demonstrates a deeper pairwise integration where Epic-related movement/dodge behavior can coexist with gun presentation while battle-mode pose ownership yields.

### Possible improvement

Split Epic Fight into finer capabilities during TaCZ ownership:

```text
Epic melee attack/battle pose: OFF while gun owns state
Epic safe movement/dodge: optionally remain ON
TaCZ ADS/reload/weapon presentation: ON
```

### Risk

If the retained Epic subsystem touches input, arms, pose, camera or movement in a way TaCZ expects to own, this can reintroduce conflicts.

**Recommendation: TEST IN REAL GAME FIRST. This is the best genuine future feature candidate.**

---

# 10. Deeper Epic Fight + YSM integration

### Current

RC16 arbitrates provider/model ownership at a high level.

### Reference

EpicYSM shows how deep this problem can become: skeleton transforms, model switching, renderer choice, armor overlays, animation blending and diagnostics.

### Possible improvement

Add deeper dedicated Epic/YSM transform/blend handling.

### Risk

This is much more native-renderer/version-sensitive than high-level ownership routing. It can easily create a new maintenance project.

**Recommendation: ONLY add for a specific visible defect in your exact YSM/Epic stack.**

---

# 11. Automatic full weapon compatibility discovery

### Temptation

On join, scan every registered weapon, determine Better/Epic compatibility, and build a giant internal registry.

### Why I would not

- duplicates upstream compatibility data;
- risks join-time hitching;
- can become stale after datapack/config/provider changes;
- creates broad hidden work for a feature manual F12 users never asked for;
- some existing bridges have had to optimize exactly this kind of registration spike.

RC16's cached narrow rule evaluation is safer.

**Recommendation: DO NOT ADD globally.**

---

# 12. Server-side policy / admin lock

### Current

RC16 focuses on player-side live selection plus minimal presentation synchronization.

### Possible future feature

Servers could optionally define policies such as:

- allowed primary engines;
- forced Vanilla in specific worlds/arenas;
- disable Smart Hybrid server-wide;
- lock Punchy presentation policy;
- advertise recommended configuration to clients.

### Why it could be useful

For a curated multiplayer pack, admins may want deterministic combat rules.

### Risk

Do not turn this into a heartbeat/continuous enforcement system. Policy should synchronize on login/config change and be event-driven.

**Recommendation: OPTIONAL FUTURE FEATURE if you actually need server governance.**

---

# 13. More UI automation

### Could add

- a tiny HUD badge showing current primary owner;
- a one-screen “safe defaults” preset;
- per-engine Punchy indicators;
- provider readiness indicators;
- native conflict diagnostics (“Better Combat is gated OFF”, “waiting for handshake”, etc.).

### Why these are safer than combat-code changes

They improve observability without modifying ownership semantics.

**Recommendation: if you want more polish, add diagnostics/UI before adding more automatic routing.**

---

# My preferred frozen RC16

If this were my long-term modpack baseline, I would ship exactly this behavioral policy:

```text
Vanilla default
F12 manual three-engine cycle
Smart Hybrid off
Hard live provider gates
Punchy companion matrix
YSM presentation arbitration
TaCZ temporary firearm takeover
Better Combat handshake safety
single first-person owner
cached/event-driven routing
no server watchdog
no broad registry crawler
```

Then I would change code only after a **native exact-stack bug report** identifies a specific boundary.

That is the shortest path to the “QOL, it just works, stop editing it every day” goal.
