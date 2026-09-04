# 📚 References

Research snapshot: **2026-09-03**. These links are references for design/audit purposes; Combat Style Suite does not redistribute or claim ownership of upstream projects.

## Primary providers

### Better Combat
- GitHub source: https://github.com/ZsoltMolnarrr/BetterCombat
- Modrinth: https://modrinth.com/mod/better-combat
- CurseForge: https://www.curseforge.com/minecraft/mc-mods/better-combat-by-daedelus

**Why referenced:** official integration architecture, weapon attributes/fallback compatibility, multiplayer synchronization/readiness, and guidance around removing conflicting attack/model/input semantics.

### Epic Fight
- CurseForge: https://www.curseforge.com/minecraft/mc-mods/epic-fight-mod
- Source/project ecosystem: search GitHub for the official Epic Fight repository maintained by the Epic Fight team/Yesssssman for the exact branch/version under study.

**Why referenced:** independent combat-mode authority, animation/player-patch state, and documented ecosystem conflicts when another combat renderer overrides Epic presentation without actually transferring gameplay ownership.

### Yes Steve Model (YSM)
- Official documentation: https://ysm.cfpa.team/
- Modrinth: https://modrinth.com/mod/yes-steve-model

**Why referenced:** model/render scheduling, linked-mod feature controls, performance/stability notes, and evidence that presentation compatibility should remain separate from basic attack ownership.

### Punchy
- Modrinth: https://modrinth.com/mod/punchy
- CurseForge search: https://www.curseforge.com/minecraft/search?page=1&pageSize=20&sortBy=relevancy&class=mc-mods&search=Punchy

**Why referenced:** first-person presentation provider that must yield cleanly to external combat/render ownership.

### TaCZ / Timeless and Classics Zero
- CurseForge: https://www.curseforge.com/minecraft/mc-mods/timeless-and-classics-zero
- Modrinth search/project ecosystem: search for **Timeless and Classics Zero / TaCZ** for the exact 1.20.1 build used by your pack.

**Why referenced:** firearm-specific ADS/reload/handling state that should temporarily own presentation instead of being forced through melee animation authority.

---

## Compatibility projects studied

### Epic Fight × TaCZ (`Ardelhite/epic-tacz`)
- Source: https://github.com/Ardelhite/epic-tacz

**Evidence depth:** public source reviewed.

**Relevant lessons:** temporarily make Epic Fight yield battle presentation while a TaCZ gun is active, restore state afterward, integrate TaCZ third-person animation behavior, and consider retaining safe Epic movement/dodge capability separately from battle-pose ownership.

### EpicYSM
- CurseForge search: https://www.curseforge.com/minecraft/search?page=1&pageSize=20&sortBy=relevancy&class=mc-mods&search=EpicYSM
- Modrinth search: https://modrinth.com/mods?q=EpicYSM

**Evidence depth:** public project documentation/release behavior reviewed.

**Relevant lessons:** deeper Epic Fight ↔ YSM compatibility is about skeleton/model/transform/render blending and diagnostics, not about making YSM another basic-attack engine.

### Epic Fight & Better Combat x Punchy!
- CurseForge search: https://www.curseforge.com/minecraft/search?page=1&pageSize=20&sortBy=relevancy&class=mc-mods&search=Epic%20Fight%20Better%20Combat%20Punchy

**Evidence depth:** project-page audit; public source not confirmed during this research pass.

**Relevant lessons:** Punchy presentation should yield conditionally while Epic Fight battle state or Better Combat swinging owns the conflicting lane.

### Tijōn's Epic Punchy
- CurseForge search: https://www.curseforge.com/minecraft/search?page=1&pageSize=20&sortBy=relevancy&class=mc-mods&search=Tijon%20Epic%20Punchy

**Evidence depth:** project-page audit.

**Relevant lessons:** intelligent Punchy/Epic presentation switching and firearm-aware coexistence reinforce RC16's companion/specialist-owner architecture.

### Better Combat × Epic Fight compatibility projects
- CurseForge search: https://www.curseforge.com/minecraft/search?page=1&pageSize=20&sortBy=relevancy&class=mc-mods&search=Better%20Combat%20Epic%20Fight%20Compat

**Evidence depth:** project/release-note audit.

**Relevant lessons:** automatic weapon registration can create join-time work if done monolithically; compatibility maintainers have optimized registration workloads to reduce stutter. RC16 therefore avoids inventing a broad background registry crawler for its own routing job.

### Server Better Combat / toggle-command utilities
- CurseForge search: https://www.curseforge.com/minecraft/search?page=1&pageSize=20&sortBy=relevancy&class=mc-mods&search=Server%20Better%20Combat%20Toggle

**Evidence depth:** project-page audit.

**Relevant lessons:** live Better Combat enable/disable is a legitimate QOL need, but a simple command toggle does not solve the full ownership/render/provider-state problem RC16 handles.

---

## Combat Style Suite public artifacts

- Repository: https://github.com/Herbertofury/Minecraft-Combat-Style-Suite
- Wiki: https://github.com/Herbertofury/Minecraft-Combat-Style-Suite/wiki
- RC16 tag: https://github.com/Herbertofury/Minecraft-Combat-Style-Suite/tree/v5.0.0-rc16
- RC16 release: https://github.com/Herbertofury/Minecraft-Combat-Style-Suite/releases/tag/v5.0.0-rc16
- Public checksums in repository: `dist/RC16-SHA256.txt`

---

## Reference-use policy

When a compatibility project is closed source or its exact source was not confirmed, this wiki treats its public project description as **behavioral reference only**.

We do not claim to have decompiled/analyzed binaries that were not actually acquired in the runner.

When public source was available—such as Better Combat and `Ardelhite/epic-tacz`—the source review is explicitly labeled as the stronger evidence class.

That distinction should be preserved in future compatibility research so the project never turns a project-page claim into fake implementation proof.
