# ANSELM

*A tale of the last age.*

> "The universe is dying. Five dimensions fracture at their seams. You are the only force that can stop it."

ANSELM is a free, single-file HTML5 browser game. No install. No account. No server. Open the file in any modern browser and play immediately.

---

## Files

| File | Description |
|------|-------------|
| `Anselm-v3_7_0-1.html` | **The game.** This is the only file you need to play. |
| `ANSELM_v3_7_1_landing.html` | Marketing / landing page with 3D realm flythrough and embedded Daedalus Codex. |
| `ANSELM_v3_7_1_Report.md` | Full technical report — every system documented in detail. |

---

## How to Play

**Locally** — download `Anselm-v3_7_0-1.html`, double-click it. Done.

**Hosted** — drop the single HTML file anywhere. No build step, no dependencies, no `.htaccess` needed. Works on GitHub Pages, Netlify, any static host.

**Mobile** — works in Safari (iOS) and Chrome (Android). Tap to move; pinch to zoom.

---

## What Is ANSELM

You are the Traveller — an ancient being woven into the fabric of creation before the first star. Five parallel dimensions are dying, consumed by a spreading black corruption called **the Goo** that seeps from a hidden Nexus Eye in each world. You alone can cross between them.

Your mission: nurture five indigenous civilisations from primitive huts to the Grid Awakening, forge weapons from dimensional crystals, and destroy the Nexus Eye in every realm before the darkness consumes everything.

---

## The Five Realms

| # | Name | Hazard | Boss | Weapon |
|---|------|--------|------|--------|
| 1 | **Ashveil** — the Pink Realm | None (tutorial) | Void Leviathan | Ashen Blade |
| 2 | **Thalassorn** — the Blue Realm | Flooding | Crystal Behemoth | Tidecaller |
| 3 | **Verdekai** — the Green Realm | None | Storm Wyrm | Verdant Spear |
| 4 | **Solenthar** — the Beige Realm | Sandstorm | Bone Colossus | Duskbrand |
| 5 | **Umbrakor** — the Purple Realm | Gravity drift | Shadow Titan | Voidreaver |

---

## Core Loop

1. **Enter the Chamber** — power the Daedalus machine with a Pink Crystal to read your briefing, then walk through the door into Ashveil.
2. **Mine Silverstone** — collected from silver ore deposits and rock clusters. Refine it at the Obelisk (10 raw → 2 refined).
3. **Mine MSM Crystals** — costs 1 refined Silverstone per deposit. Each dimension's crystals have a different value (Dim 1 = 1 pt through Dim 5 = 5 pts).
4. **Travel via Gateways** — spend 10 pts of MSM to cross to the next dimension. Once you've forged a weapon for a dimension, travel there is free.
5. **Forge weapons** — at each realm's Forge using MSM from multiple dimensions. Kill the Boss with the matching weapon.
6. **Combine all five** into the Ultimate Weapon.
7. **Destroy five Nexus Eyes** — step onto each one while carrying the Ultimate Weapon.
8. **Keep playing** — after victory, civilisations keep advancing through the Extended Ages indefinitely.

---

## Features

### Genetics Engine
Every native carries 10 heritable traits: agility, work, social, faith, resilience, curiosity, hair colour, skin colour, home dimension, and gender. Children inherit traits 50/50 from each parent with a 15% mutation chance. Cross-dimensional couples produce hybrid children that accelerate civilisation XP. After a Nexus is destroyed, crystal-gene survivors spawn — their descendants glow purple-cyan and slowly merge the tile fabric of two dimensions together.

### Civilisation Ages
Each realm evolves independently through five base ages (Primitive → Heaven) and six named extended ages driven by the in-world text *Lux in Tenebris*:

| Age | Name | Landmark Built |
|-----|------|---------------|
| 5 | Possession Age | Great Library |
| 6 | Shared Age | Telegraph Tower |
| 7 | Bandwidth Age | Satellite Antenna |
| 8 | Feed Field Age | Feed Tower + Dimensional Contact |
| 9 | Hyper-Holo-Grid Age | Grid Node |
| 10 | The On and On | Dimension Seed |

When all five dimensions reach Age 8 and make contact, the Grid Awakening triggers — Ascension seeds every realm with enlightened natives from every bloodline. Civilisations in Age 10 begin seeding entirely new dimensions from pure native intention.

### The Daedalus Codex
An in-game encyclopedia accessible from the Chambers tile. Seven chapters — Natives, Dimensions, Forge, Mining, Genetics, Civilisation, and the full text of *Lux in Tenebris* with prev/next navigation. The Forge page shows live FORGED / NOT YET FORGED status for each weapon. The title screen has its own Codex button that shows your chapter progress and named artifacts across all sessions.

### Edict System (E key)
Issue royal decrees once you have 20 loyalty points. Four types: Build Decree (forces construction every 5 seconds), Study Decree (doubles scripture speed), Militarize (lowers army threshold), Expansion Edict (triples migration). Each lasts 5 minutes.

### Diplomacy (D key)
Once two dimensions reach Age 8 and make contact, their relationship evolves toward alliance or rivalry based on pre-set affinity scores. Allied dimensions share migration bonuses. Rival dimensions spawn raiders. Broker peace with 10 refined Silverstone. Forge alliance bonuses with 5 refined Silverstone.

### Realm Tension & War
A Tension meter per dimension rises when trees are depleted, resources are low, or disputes are active. High Tension escalates through five war levels (Peace → Skirmish → Conflict → War → Total War), spawning raid parties at War 3+. Reaching the Hyper-Holo-Grid Age (9) automatically ends all wars in that dimension.

### Town Founding (F key)
Found new settlements anywhere with loyal native labour (30 wood + 20 stone). Towns with 10+ houses automatically build animal pens — 6×6 fenced enclosures that herd three wild animals inside. Farms grow food, mills process grain, hospitals reduce casualties, gunsmiths arm natives at Town age.

### Native Quests
Named neutral natives periodically offer quests: stone delivery, escort duty, or cross-dimensional travel. Completion rewards loyalty, town happiness, or XP.

### Meta-Progression
Four pieces of data persist between sessions in `localStorage`:
- **Ancestors** — top 3 highest-generation natives carry their trait sets into the next run.
- **Heaven Memory** — dimensions that reached Heaven grant bonus starting XP in the next run.
- **Codex Progress** — which *Lux in Tenebris* chapters have been studied, accumulating across sessions.
- **Named Artifacts** — when a weapon is forged with Generation 3+ natives present, it takes a native's bloodline name (e.g., *Vaelthar's Ashen Blade*) and persists in the Codex indefinitely.

---

## Keyboard Shortcuts

| Key | Action |
|-----|--------|
| `E` | Edict panel |
| `D` | Diplomacy panel |
| `F` | Found a town |
| `C` | Chronicle panel |
| `U` | Universe panel (all five dims at a glance) |
| `P` | Character panel / save |
| `M` | Minimap |
| `+` / `-` | Zoom in / out |
| `0` | Reset zoom |

---

## Technical

- **Single file** — 16,193 lines of vanilla JavaScript, HTML, and CSS. No frameworks, no build tools, no external assets.
- **Canvas 2D** — all graphics drawn procedurally with `ctx.fillRect()`. No sprite sheets, no images.
- **Web Audio API** — all sound synthesised in real time. Zero audio files. The title theme is a composed piece in A minor (56 BPM, five structural parts, six voice timbres). The Chamber plays an OoT Fairy Fountain reconstruction in D major.
- **Procedural maps** — seeded LCG RNG generates unique 80×60 tile maps per dimension. Seeds are saved for deterministic reloads.
- **Works offline** — after download, no network connection is needed.
- **Save system** — autosaves every 5 minutes. Manual save via Character Panel. Full export/import as JSON.
- **Tested on** — Chrome, Firefox, Safari, Edge (desktop and mobile).

---

## Lux in Tenebris

The in-world sacred text that seeds all post-Heaven civilisational development. Ten chapters, each named and themed:

1. Possession of My Reality
2. Shared Reality
3. Establishing Consensus of a Reality
4. The Universe Experiencing Itself
5. The Nature of Which Reality is Created
6. Birth of Self
7. Understanding Time is Value
8. Eternal Bandwidth
9. The Dragon and the Black Box
10. The Discourses of the Actuality

Each chapter names a new civilisational era. Scholars in every realm study successive chapters as they age, interpreting ancient wisdom to build new wonders. Read every chapter in full inside the Daedalus Codex.

---

## Version

**v3.7.0-1** — Extended Ages & Codex Depth Update.

Previous major changes include: the Chamber Room tutorial, Daedalus Codex, World Events, Edict System, Town Founding, Diplomacy, Realm Tension & War, meta-progression, Native Quests (v3.7.0); crystal ammo / overcharge, cross-town faction raids, Goo storm / blight → migration, ASCENSION on grid awakening, Dim Fusion, agility gene, Wildling enemies, minimap, Age 8+ plane vehicles, Age 10 robot workers (v3.6.0).

---

*Made in Austin, TX.*
