# Combat Style Suite 5.0.0 RC17 — Public Handoff

RC17 is the **Just Works Polish** release for Forge 1.20.1 / Java 17.

## Everyday experience

The default screen intentionally exposes only the normal controls: Vanilla, Better Combat, Epic Fight, YSM, Punchy, Punchy Pairings, Provider Toggles, Fix Everything Safely and Advanced.

Advanced automation/rules/fusion/controller/diagnostic controls remain available under Advanced rather than crowding normal play.

## Safety and performance invariants

- Better Combat's unsafe pre-handshake force-enable path has been physically removed.
- Selecting Better Combat before its real handshake safely uses Vanilla as the temporary effective fallback, then activates Better Combat live when the handshake arrives.
- Transition messages are event-only action-bar notices with no tick listener/timer/scheduler/executor/watchdog/heartbeat.
- Fix Everything Safely is a one-shot reconciliation, not a recurring repair service.
- Smart Hybrid remains OFF by default and advanced-only.
- CI rejects global client/server/player tick enforcement, watchdog/heartbeat behavior, recurring scheduler loops, full item/weapon-registry crawling, unsafe Better Combat readiness bypasses and recurring notice behavior.

## Frozen release hashes

```text
381ecd3f4b730712420c2c5112628072a8fe55a006f676360d8b014808fef191  combat-style-director-5.0.0.jar
02bd08e79323fae9fc513ccb2870f0c414f94ca8cf14cff246e0b956d08dd8cd  punchy-style-compat-5.0.0.jar
4064d4ce0cf7febbb2691eb1274eda9a0a652adce2703aaf787bd12d1060d9d4  Combat-Style-Suite-5.0.0-RC17-just-works-polish.zip
91858d54301c4add8f96814f5fde024df6e007073ec0220ae67982c4fd4d52b2  Combat-Style-Suite-5.0.0-RC17-just-works-polish-source.zip
```

## Verification boundary

The deterministic source/harness/static/package suite is green, and a clean source extraction reproduces the JARs and install bundle byte-for-byte. The final unclaimed field gate is a fresh native Forge client test with the exact Epic Fight + Better Combat + YSM + Punchy + TaCZ provider binaries together.

See the [Wiki](../../wiki) and **RC17 — Just Works Polish** page for the user-facing guide.
