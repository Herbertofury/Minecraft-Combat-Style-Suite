# 🧯 Troubleshooting

The fastest way to debug Combat Style Suite is to identify **which ownership lane is wrong** instead of changing every compatibility setting at once.

## First rule: verify the installed RC16 pair

You should have exactly one copy of each:

- `combat-style-director-5.0.0.jar`
- `punchy-style-compat-5.0.0.jar`

Frozen RC16 hashes:

```text
ed71dcc66e2245a2b61b5c2d5c11090f83f1c31ff9509ebace63c895548149c6  combat-style-director-5.0.0.jar
02bd08e78718aab3787321cf5084ef0c43dcbe773f9a119b4010fe8bff04d8cd  punchy-style-compat-5.0.0.jar
```

Do not keep older RC14/RC15 copies beside them.

---

## “F12 selects Better Combat but it does not activate”

Check:

1. Better Combat is installed and version-compatible.
2. Its live provider gate is ON.
3. You have entered a server/world and allowed its normal readiness/handshake to occur.
4. Unsafe force-enable remains OFF unless you deliberately changed it.

RC16 intentionally prefers a safe fallback over pretending Better Combat is ready when it is not.

---

## “I turned Better Combat OFF, but a handshake happened”

That is okay.

RC16 can remember that Better Combat became ready without changing your live OFF choice. The handshake should not silently override the provider gate.

Turn the gate ON when you actually want Better Combat eligible again.

---

## “I turned YSM OFF and it came back”

That is a bug if it reproduces on frozen RC16.

RC16 specifically guards the YSM OFF state against external/config state trying to re-show the self model.

Capture:

- exact YSM version;
- whether you changed models at the same time;
- third or first person;
- exact log around the transition.

Do not solve it by disabling all YSM compatibility globally first; preserve the reproducer.

---

## “Punchy looks wrong only with one engine”

Use the pairing matrix as the isolation tool.

Example:

```text
Punchy master ON
Punchy + Vanilla ON
Punchy + Better OFF
Punchy + Epic ON
```

If only Better Combat conflicts, disable only Punchy + Better Combat while retaining the other two pairings.

That is preferable to deleting Punchy or changing global first-person ownership.

---

## “Extra arm/hand on right click”

Frozen RC16 already contains the RC14 single-arm fix.

Isolation order:

1. verify RC16 hashes;
2. reproduce with Punchy OFF;
3. reproduce in Vanilla / Better / Epic separately;
4. test main hand and offhand use-item;
5. identify which external renderer is drawing the intended view;
6. capture the exact item/action.

If a duplicate remains only with Punchy ON, fix the narrow Punchy ownership boundary rather than reverting Better Combat's proven both-hand suppression.

---

## “TaCZ gun animation is overridden”

Check:

- TaCZ integration gate ON;
- exact gun vs a non-TaCZ firearm;
- ADS vs hip fire vs reload;
- current primary melee engine;
- Punchy ON/OFF;
- YSM ON/OFF.

The correct behavior is for TaCZ to temporarily own firearm presentation and then return to the selected melee state.

---

## “Combat mode changes when I swap weapons”

Check whether **Smart Hybrid** or automatic combat rules are enabled.

Smart Hybrid is OFF by default. F12 manual mode should not automatically change merely because you equipped a different sword.

---

## “Why is there a one-second check?”

Only advanced NBT-sensitive capability rules may arm that client-only freshness fallback. Normal F12/provider/Punchy/TaCZ use does not require it.

See [[Smart Hybrid and Rules]].

---

## “Server performance got worse”

Before blaming the Suite, capture evidence:

1. server timings/profiler before and after;
2. Suite version/hash;
3. player count;
4. whether advanced rules are enabled;
5. whether presentation sync is producing repeated unchanged traffic;
6. fresh server log.

RC16's design deliberately avoids a primary-routing server tick listener, watchdog and heartbeat. A measurable server regression should therefore be treated as a real bug and traced to a concrete lifecycle/network path.

---

## Safe reset

Use the Suite's safe/default reset path rather than deleting random config keys.

The intended safe baseline is:

```text
Primary = Vanilla
Smart Hybrid = not selected
Unsafe Better force-enable = OFF
Ownership verification = ON
Fallback = ON
```

Then enable only the provider/presentation features you want.

---

## What to include in a useful bug report

- Minecraft version
- Forge version
- RC16 JAR hashes
- exact versions of Epic Fight / Better Combat / YSM / Punchy / TaCZ installed
- current primary mode
- provider gate states
- Punchy pairing states
- Smart Hybrid/rules state
- first-person or third-person
- exact held item/action
- whether issue happens after F12 switch or fresh login
- fresh log
- smallest repeatable sequence

A five-line reproducer is far more valuable than “combat animations are weird.”
