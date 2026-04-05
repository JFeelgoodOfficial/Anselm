# ANSELM v3.7.0-1 — Comprehensive Game Report
*A tale of the last age — "The universe is dying. Five dimensions fracture at their seams. You are the only force that can stop it."*

---

## Table of Contents

1. Overview & Setting
2. The Five Dimensions
3. Character Creation
4. Core Gameplay Loop
5. The World — Tiles & Map
6. Resources & Economy
7. The Goo — Enemy Force
8. Nexus Eyes & Victory
9. Weapon System & The Forge
10. The Civilization System — Ages 0 through Infinity
11. Lux in Tenebris — The Sacred Text
12. The Extended Ages (Post-Heaven)
13. Native AI — The People of the Realms
14. Genetics & Reproduction
15. Boss Monsters
16. Shrines, Obelisks & Sacred Structures
17. The Chronicle Panel
18. Save System
19. **Chamber Room — Pre-Game Tutorial**
20. **Daedalus Codex**
21. **World Events**
22. **Edict System**
23. **Town Founding**
24. **Diplomacy System**
25. **Realm Tension & War Escalation**
26. **Meta-Progression**
27. **Native Quests**
28. **Farm Growth System**
29. Technical Architecture

---

## 1. Overview & Setting

ANSELM (v3.7.0-1) is a single-file HTML5 browser game. You play as an ancient, cosmically significant being — woven into the fabric of creation itself before the first star — who must now travel across five dying dimensions, guide their civilizations, forge weapons, and destroy five Nexus Eyes to purge a spreading "black cancer" called **the Goo**.

The tone is literary and philosophical. The lore references an in-world sacred text called **Lux in Tenebris** ("Light in Darkness"), and the civilizations in each dimension evolve through technological and spiritual ages directly inspired by that text's ten chapters.

**v3.7.0-1 — Extended Ages & Codex Depth Update.** This version retains all v3.7.0 systems and adds: a dedicated **Silverstone ore tile** with neighbour-aware rendering, six **extended-age landmark tiles** (Library through Dimension Seed), a full **Animal Pen system**, a completely recomposed **ANSELM title theme** (A minor, 56 BPM, five structural parts, six voice timbres), and a **full-text Lux in Tenebris reader** inside the Daedalus Codex with prev/next chapter navigation.

---

## 2. The Five Dimensions

Each dimension is a procedurally generated 80×60 tile map with a distinct color palette, a resident civilization, and its own Nexus Eye to destroy.

| # | Name | Base Color | Native Skin | Weapon |
|---|------|------------|-------------|--------|
| 1 | The Pink Realm (Ashveil) | Rose-dusty | `[220,160,170]` | Ashen Blade |
| 2 | The Blue Realm (Thalassorn) | Steel-periwinkle | `[130,175,210]` | Tidecaller |
| 3 | The Green Realm (Verdekai) | Forest green | `[100,185,120]` | Verdant Spear |
| 4 | The Beige Realm (Solenthar) | Warm sandy | `[210,190,140]` | Duskbrand |
| 5 | The Purple Realm (Umbrakor) | Indigo-violet | `[175,140,215]` | Voidreaver |

Trees are always green (though the exact shade is dimension-specific), water always blue, and rocks always gray — natural elements remain consistent across realms while architecture and ground tiles take on their dimension's signature hue.

Dimensions are unlocked sequentially by traveling through **Gateways**. Each Gateway costs 10 points worth of MSM crystals to use. Visiting a dimension for the first time **activates the Goo** within it, and the Boss Monster spawns 45 seconds later.

**Dimensional Affinity.** Each pair of realms has a pre-set affinity score (the 5×5 `DIM_AFFINITY` matrix). High-affinity pairs naturally form alliances; low-affinity pairs become rivals. These relationships evolve through the Diplomacy system once contact is established at Age 8+. Two hate-pairs are hard-coded (Blue–Green, Green–Purple), which tend toward early conflict.

---

## 3. Character Creation

Before entering the game the player completes a two-step character creation sequence:

**Step 1 — Gender.** Choose She/Her or He/Him. Each option shows a pixel-art canvas preview of the character.

**Step 2 — Appearance.** Choose from four skin tones (Light, Tan, Brown, Deep) and six hair colors (Brown, Black, Auburn, Blonde, Silver, Red). The chosen combination renders live in a 96×144 preview canvas using direct pixel drawing.

After selecting their appearance, the player enters the **Chamber Room** (see Section 19) before entering Ashveil proper.

These choices are purely cosmetic for the base character. Outfit appearance is upgraded over time as the player receives gifts from native shrines, with each gift pushing items into ring, shoes, and accessory arrays that stack passive effects.

---

## 4. Core Gameplay Loop

The moment-to-moment loop follows this rhythm:

1. **Explore** — Move by tapping/clicking the map. The camera follows the player smoothly.
2. **Mine MSM** — Step on or near MSM tiles to collect dimension crystals. Higher-numbered dimensions yield more valuable MSM (Dim 1 = 1 pt, Dim 5 = 5 pts).
3. **Interact with Natives** — Build loyalty through repeated contact, shrine blessings, and gifts. Loyal natives patrol, trade, and escort you.
4. **Travel** — Spend MSM at Gateways to cross to the next dimension.
5. **Forge Weapons** — At each dimension's Forge, combine MSM from multiple dimensions to craft a weapon unique to that realm.
6. **Survive the Goo** — The spreading black corruption constantly threatens your Forge and Gateway. If Goo surrounds either structure completely, it's game over.
7. **Kill the Boss** — Each dimension has a Boss Monster you can only kill with its corresponding weapon (or the ultimate weapon).
8. **Destroy the Nexus** — Armed with the ultimate combined weapon, step onto each dimension's Nexus Eye tile to cleanse it. Do all five to win.

**New in v3.7.0:** Issue Edicts to accelerate your civilization, found new towns, manage inter-dimensional diplomacy, respond to World Events (meteor strikes, eclipses, storms, migrations), and complete quests for individual natives.

After winning, the game continues — civilizations keep advancing, dimensions keep evolving.

---

## 5. The World — Tiles & Map

Each map is 80 tiles wide × 60 tiles tall. Each tile is 32×32 pixels before scaling. The full tile type system:

### Natural Tiles
- **GROUND (0)** — Walkable flat land.
- **TREE (1)** — Animated, wind-responsive canopy. Three layers, each swaying independently. Source of wood for natives. Regrows from SAPLING in 30 seconds.
- **WATER (2)** — Impassable. Rendered with animated shimmer lines, rounded shore corners, and depth-fade strips.
- **ROCK (3)** — Impassable. Respawns after 40 seconds.
- **SILVERSTONE (12)** — Dedicated silver-ore deposit tile. Placed by the map generator in clusters near towns. Uses a neighbour-aware jagged-edge renderer (`drawSilverstone`) with a 5-layer palette (deep blue-grey base → mid silver → bright highlight → near-white specular → shadow crevice) and per-tile deterministic RNG for stable jagged teeth on every exposed face. Replaces the old practice of treating Silverstone as a byproduct of generic ROCK tiles.
- **SAPLING (7)** — Small shoot, sways dramatically in wind. Grows to TREE in 15 seconds if planted by a native.
- **GOLDEN_FRUIT (19)** — Special pickup.

### Player Structures
- **MSM (4)** — Crystal mineral veins. Mining these is the primary economy.
- **GATEWAY (5)** — Inter-dimensional travel terminal. Protected structure — if surrounded by Goo, game over.
- **FORGE (6)** — Weapon crafting terminal. Also protected — Goo surrounding it = game over.
- **NEXUS (17)** — The Goo's central eye. One per dimension. Destroyed only with the ultimate weapon.
- **SHRINE (14)** — Placed adjacent to Gateways when the player departs. Site of native worship and gift-giving.
- **OBELISK (30)** — Appears near the Forge after the first weapon is forged. Loyal natives deposit Silverstone here. Refines at 5:1 ratio to Refined Silverstone.
- **CHAMBERS (36)** — Chambers of Divine Enlightenment. The player's sacred house near the Forge. Stepping on it opens the Daedalus Codex.
- **SACRED (31)** — Golden ground around Forge/Gateway. Cannot be built on.

### Enemy Tiles
- **GOO (16)** — The spreading corruption. Converts natural and civilization tiles.
- **BURNING (9)** — A hut set alight during a faction dispute.

### Civilization Tiles (Age-Gated)
- **HUT (8)** — Native dwelling (Age 0).
- **HUT2 (22)** — Improved native dwelling (Age 1 — Settlement).
- **HUT3 (23)** — Stone dwelling (Age 2 — Town).
- **ROAD (10)** — Laid by natives between towns. Enables trade income and cart traffic.
- **GREAT_HALL (11)** — 2×2 communal building at the heart of every town.
- **PIER (13)** — Wooden dock extending into water.
- **TOWER (24)** — Defensive structure (Age 2).
- **HIGHWAY (25)** — Upgraded road (Age 3 — City).
- **CANNON (26)** — Siege weapon (Age 3).
- **ROCKET (27)** — Ballistic weapon (Age 3).
- **SKYSCRAPER (28)** — Tall building (Age 4 — Heaven).
- **HEAVEN_ROAD (29)** — Luminous road (Age 4).
- **FARM (32)** — Crop field. Grows food, reduces dispute chance.
- **MILL (33)** — Windmill. Processes grain, unlocked at Settlement age.
- **HOSPITAL (34)** — Heals wounded natives, reduces dispute casualties.
- **GUNSMITH (35)** — Gun shop. Natives carry rifles in disputes at Town age+.
- **MISSILEDEFENSE (99)** — Intercepts catapult/rocket projectiles within a 12-tile radius at ~50% of flight path. Requires Age 2 and 25 Refined Silverstone.
- **BRIDGE (37)** — Auto-built plank crossing over water when natives lay roads across rivers.
- **WATER_TEMPLE (18)** — Coastal religious structure.
- **CATAPULT (20)** — Wooden siege machine with animated arm and counterweight.
- **FENCE (21)** — Post-and-rail wooden perimeter. Also used as the wall of animal pens (see §Animal Pen System).

### Extended-Age Landmark Tiles (Ages 5–10)
Each of these tiles is placed once per dimension when the corresponding age is reached, is fully Goo-immune, and has its own procedural draw function:

- **LIBRARY (38)** — Age 5 (Possession Age). A carved stone archive with grand steps, classical pillars, a scroll window with warm amber glow, and a dark wooden door.
- **TELEGRAPH (39)** — Age 6 (Shared Age). A signal tower with crossbar, wire stretches to tile edges, and a blinking amber signal light (oscillates with `Math.sin(t*0.3)`).
- **SATELLITE (40)** — Age 7 (Bandwidth Age). A broadcast antenna mast with a rectangular dish and animated blue-tinted signal rings that pulse outward.
- **FEED_TOWER (41)** — Age 8 (Feed Field Age). A sleek tower tinted in the dimension's accent colour with an orbiting ring dot that rotates around the shaft in real time.
- **GRID_NODE (42)** — Age 9 (Hyper-Holo-Grid Age). A crystalline white pillar with horizontal grid lines, a glowing crystal cap, and a soft blue aura that pulses in and out.
- **DIMENSION_SEED (43)** — Age 10 (The On and On). An ethereal seed crystal with outer aura, inner light, cascading facets, and occasional random particle sparkles. The brightest object on any tile map.

### Other
- **SACRED (31)** — Also placed by generative ages near town centres as golden ground.

---

## 6. Resources & Economy

### MSM Crystals (Moonstone)
The primary currency. Five separate per-dimension stocks (Dim 0–4). Used to pay travel costs and craft weapons. Mining triggers a Goo spread event in the active dimension (+1 pending spread per mine).

**MSM Point Values:** Dim 1 = 1 pt · Dim 2 = 2 pts · Dim 3 = 3 pts · Dim 4 = 4 pts · Dim 5 = 5 pts

**Gateway Travel Cost:** 10 points worth of MSM.

### Silverstone & Refined Silverstone
Collected from both **ROCK tiles** (which respawn after 40 seconds) and dedicated **SILVERSTONE ore tiles** (permanent deposits placed by the map generator). Raw Silverstone is deposited at the Obelisk by loyal natives. The Obelisk refines it at a 5:1 ratio. Refined Silverstone is used to build Missile Defense installations (25 required per town), Animal Pen construction (8 raw Silverstone), and Diplomacy actions (Peace Brokering costs 10 refined; Alliance Bonus costs 5 refined).

### Water
Collected from Water Temples. Used in specific crafting recipes.

### Time Crystals (v3.7.0)
In v3.7.0, Time Crystals no longer swap tiles across dimensions. Instead, they can be **planted** on clear ground tiles (see Section 22 for details). Each planted crystal grows into a 3×3 MSM crop after 60 seconds. Time Crystals also still power Crystal Ammo — dealing 1.4× damage (2× when overcharged) and consuming one crystal per shot.

### Bone Fragments
Dropped by Boss Monsters when killed. One fragment per boss dimension. Used with the ultimate weapon in the Forge to craft the **Anselm Helm** (requires 5 bone fragments + ultimate weapon).

### Trade Income
When two towns are road-connected (verified by a BFS flood-fill over road tiles), passive resources accumulate as `tradeIncome` on both town objects each tick.

---

## 7. The Goo — Enemy Force

The Goo is the game's primary antagonist — a spreading black corruption described in lore as "a black cancer that spreads through every dimension, consuming everything it touches."

### Activation
The Goo is dormant until the player first visits a dimension. On first visit, `activateGooDim()` is called, which marks the dimension as active and schedules a Boss spawn 45 seconds later (2700 ticks at 60fps).

### Spreading
The Goo uses a frontier-based spread system. Every game tick it processes up to 20 pending spread operations from `goo.spreadQueue`. It cannot spread to immune tiles (Forge, Gateway, Water, Pier, Shrine, Obelisk, Water Temple, Nexus, Missile Defense, Sacred, Chambers, Great Hall, Huts, Bridge).

**Spread triggers:**
- Passive timer spread (always running once activated).
- Mining MSM (+1 spread per crystal mined).
- A native house burning during a dispute (+1 spread).
- **Goo Storms** — periodic events every 90–180 seconds that create a 4-tile-radius burst of corruption centered on a random frontier tile.
- **Crystal-Gene natives** (after Nexus destruction) can channel remnant energy to spawn new Goo patches near their position every 60 seconds.

### Goo Loss Condition
Checked once per second. If Goo tiles surround either the Forge or the Gateway within a 5-tile radius (≥24 surrounding tiles corrupted), the player dies — **"The enemy has claimed the dying universe. You are the last light, extinguished."**

### Nexus Formation
When enough Goo has spread within a dimension, it forms a NEXUS tile — the eye of the corruption. This tile can only be destroyed by the player stepping on it while carrying the ultimate weapon.

---

## 8. Nexus Eyes & Victory

### Destroying a Nexus
When the player steps on a NEXUS tile carrying the ultimate weapon, `destroyNexus()` fires:

1. All Goo and Nexus tiles in the dimension instantly revert to clean GROUND.
2. A major screen shake and flash celebrate the event.
3. 5–10 **crystal-gene survivors** spawn near the player — native refugees whose bloodlines carry the crystalGene trait.
4. The dimension immediately triggers **Heaven Age**.
5. A count updates: "Realm X is free! (N/5)"
6. If all five Nexuses are destroyed, `triggerVictory()` fires.

### Victory Screen
The victory screen displays "IT IS DONE" with the message:

> *"The enemy returns to nothing. Five realms breathe free for the first time. You alone remain — witness to what comes next."*

After victory, the player can choose **Keep Exploring** — the game continues with all civilizations still evolving, new dimensions being seeded, and the extended Lux ages unfolding indefinitely.

---

## 9. Weapon System & The Forge

### Dimensional Weapons
There are five dimensional weapons — one per realm. Each is forged at that realm's Forge using MSM from multiple dimensions. Recipes stack progressively:

| Weapon | Recipe |
|--------|--------|
| Ashen Blade (Dim 1) | 10× Dim 1 |
| Tidecaller (Dim 2) | 10× Dim 2 + 5× Dim 1 |
| Verdant Spear (Dim 3) | 10× Dim 3 + 5× Dim 2 + 5× Dim 1 |
| Duskbrand (Dim 4) | 10× Dim 4 + 5× Dim 3 + 5× Dim 2 + 5× Dim 1 |
| Voidreaver (Dim 5) | 10× Dim 5 + 5× Dim 4 + 5× Dim 3 + 5× Dim 2 + 5× Dim 1 |

Each weapon is required to damage and kill its dimension's Boss Monster. Crystal Ammo boosts weapon damage to **1.4×** (or **2× with overcharge**) and consumes one Time Crystal per shot.

**Named Artifacts (v3.7.0).** When a weapon is forged in a dimension with Generation 3+ natives, `saveNamedArtifact()` fires — a randomly chosen elder-generation native lends their bloodline name to the weapon. The named artifact (e.g., "Vaelthar's Ashen Blade") is stored in `localStorage` under `anselm_artifacts` and surfaces in both the in-game Daedalus Codex and the title-screen Codex button.

### Ultimate Weapon
After forging all five dimensional weapons, the Forge offers **"COMBINE ALL — FORGE ULTIMATE WEAPON."** The combined weapon:
- Deals **2.5× damage per tick** to any boss.
- Is required to destroy Nexus Eyes.
- Is used as the base ingredient (with 5 Bone Fragments) for the Anselm Helm.

### The Anselm Helm (Divine Helm)
Forged from 5 Bone Fragments + the ultimate weapon. When worn, it makes the player **completely immune** to both Goo damage and Boss Monster attacks. Its presence is shown in the HUD as a gold **"✦ DIVINE"** indicator.

---

## 10. The Civilization System — Ages 0 through Infinity

Every dimension has its own civilization that evolves over real time. Civilization XP (`dimCivXP`) accumulates each game tick, driven by the number of natives, towns, road connections, and player presence.

### Base Ages (0–4)

| Age | Name | XP Threshold | Key Buildings | Vehicles |
|-----|------|-------------|---------------|----------|
| 0 | Primitive | 0 | Huts, Dirt roads | On foot |
| 1 | Settlement | 3,600 (~1 min) | Hut2, Mill, Farm | Ox carts |
| 2 | Town | 10,800 (~3 min) | Hut3, Hospital, Tower, Cannon, Missile Defense | Wagons |
| 3 | City | 21,600 (~6 min) | Skyscrapers, Highway, Rocket, Gunsmith | Cars |
| 4 | Heaven | On Nexus Destroy | Skyscrapers of light, Heaven Road | Flying cars |

Town tiles upgrade automatically when an age threshold is crossed. Native buildings visually evolve — Huts become stone dwellings, roads become gleaming highways. Vehicle types spawn on road tiles and navigate using a momentum-weighted BFS (straight ahead = 3pts, turn = 1pt, U-turn = 0pts).

### Town Structure
Every dimension is procedurally seeded with 2–6 towns. Each town features:
- A **2×2 Great Hall** at the top centre.
- A road grid in a cross pattern.
- **Huts** flanking the roads (2–3 per side).
- A **specialty** determined by surrounding terrain: `coastal` (water), `forest` (trees), `stone` (rocks), or `plains`.
- A **unique outfit color** seeded from the map seed + town index.
- Road connection tracking (`roadConnections: Set`) used for trade income and trader AI.

**Player-Founded Towns (v3.7.0).** See Section 23 for the F-key Town Founding system.

---

## 11. Lux in Tenebris — The Sacred Text

*Lux in Tenebris* ("Light in Darkness") is the in-world sacred scripture that the native civilizations study. It has ten chapters, each with a title, theme, and full text.

| Chapter | Title | Theme |
|---------|-------|-------|
| 1 | Possession of My Reality | Individual awakening; wandering without purpose becomes purposeful |
| 2 | Shared Reality | Two souls merging; reality is co-created |
| 3 | Establishing Consensus of a Reality | Strangers finding common ground; choosing depth over distraction |
| 4 | The Universe Experiencing Itself | Love as the universe knowing itself through another |
| 5 | The Nature of Which Reality is Created | The self as living gallery; becoming; the revolution of morning |
| 6 | Birth of Self | Near-death; the voice in darkness; choosing to live |
| 7 | Understanding Time is Value | Home as a quality of presence; watering what matters |
| 8 | Eternal Bandwidth | Self-love; borrowed vessels; filling life with light |
| 9 | The Dragon and the Black Box | Surrender; service as the hard work; the black box opens quietly |
| 10 | The Discourses of the Actuality | The hyper-holo-grid; feed fields; the on and on |

### Scripture Interpretation Speed
Each dimension advances through the chapters over time. The speed is governed by:

```
interval = max(900, 7200
  - faithNatives × 300
  - shrineCount × 400
  - hasObelisk × 600
  - playerPresence × 300)
```

Faith-trait natives, shrines, the Obelisk, and the player's physical presence all accelerate understanding.

### Codex Progress (v3.7.0)
Chapter progress is now saved to `localStorage` under `anselm_codex` (an array of 10 booleans). It persists across sessions and is displayed in the title-screen Codex button. The Daedalus Codex panel in-game shows each chapter's title and theme once it has been studied.

---

## 12. The Extended Ages (Post-Heaven)

After a Nexus is destroyed and Heaven Age begins, the civilization clock continues. Six named extended ages (5–10) follow, plus infinite procedurally generated ages beyond that.

### Named Extended Ages (5–10)

| Age | Name | Era | Lux Reference | Special Build | Tile |
|-----|------|-----|---------------|---------------|------|
| 5 | Possession Age | The Self Awakens | Ch.1 — "this world is not mine at all" | Great Library | `LIBRARY:38` |
| 6 | Shared Age | Consensus of the Real | Ch.3 — "establishing consensus of a reality" | Telegraph Tower | `TELEGRAPH:39` |
| 7 | Bandwidth Age | Eternal Signal | Ch.8 — "Eternal bandwidth" | Satellite Antenna | `SATELLITE:40` |
| 8 | Feed Field Age | Dimensional Contact | Ch.10 — "every single person has their own video tape feed field" | Feed Tower | `FEED_TOWER:41` |
| 9 | Hyper-Holo-Grid Age | The Grid Awakens | Ch.10 — "hyper hologrid" | Grid Node | `GRID_NODE:42` |
| 10 | The On and On | Eternal Progression | Ch.10 — "there is always, only, the on. Spiral forward." | Dimension Seed | `DIMENSION_SEED:43` |

Each landmark is placed once per dimension by `updateExtendedCivAge` when the XP threshold is crossed. All six are Goo-immune (`GOO_IMMUNE` set). Each has its own procedural draw function using direct `ctx.fillRect()` calls — no sprites. See §5 Extended-Age Landmark Tiles for visual descriptions.

**Age 9 — War Ends.** When a realm reaches the Hyper-Holo-Grid Age (9), any active war in that realm is automatically resolved and `dimWarLevel` drops to 0. A Chronicle announcement is logged: *"Hyper-Holo-Grid achieved — the war between Realm N ends."* This is the universe healing itself.

### Generative Ages (11+)
Ages beyond 10 are algorithmically generated from combinations of the ten Lux Characters (Mind, Body, Soul, Self, Order, Life, Time, Eternity, Death, Repeat) and suffix types (Convergence, Transcendence, Recursion, Emergence, Dissolution, Becoming, Remembrance, Spiral, Threshold, Actuality).

### Philosophical Schisms
At Age 6+, there is a 40% chance that a new age transition triggers a **philosophical schism** — a split within the native population about interpreting the new age. This mirrors the dispute system with ideological overtones.

### Dimensional Contact & World Seeding
Age 8+ dimensions attempt every 60 seconds to **contact** other dimensions they share the extended age with. Age 10+ dimensions can **seed new dimensions** — spawning `newDimensions[]` entries representing entirely new cosmological realms.

---

## 13. Native AI — The People of the Realms

Natives are the heart of the game. Each dimension supports up to 80 natives simultaneously, each with a full behavior state machine.

### AI States

| State | Behavior |
|-------|----------|
| `idle` | Wander near home hut, rest, socialize |
| `plant` | Carry a sapling to a depleted ground tile and plant it |
| `gather` | Move to a resource (tree/rock/MSM), collect, return |
| `hunt` | Chase nearest animal; kill and carry meat back |
| `fight` | Engage enemy native in melee during a faction dispute |
| `burn` | Set a hut from the enemy faction on fire |
| `peacemaker` | Attempt to de-escalate an active dispute |
| `patrol` | Walk a route around the player if loyalty ≥ 3 |
| `msm_fetch` | Pick up MSM crystals and deliver to the player |
| `trade` | Walk to a road-connected town and exchange goods |
| `road_build` | Lay road tiles one at a time toward a target town |
| `chase` | Follow the player when loyalty is very high |

### Faction Disputes
When tree resources fall below 8 in a dimension, dispute risk increases. A dispute splits natives into Faction A and Faction B, entering a fight → burn → peace cycle. Disputes also increase **Realm Tension** (see Section 25).

### Native Personalities
Each native has one of several personalities: `brave`, `cautious`, `wanderer`, `industrious`, `social`, `faithful`, and others. Wanderers will voluntarily **migrate** to a different dimension when their current realm has fewer than 5 trees.

### Loyalty System
Loyalty (0–5) is built through player interaction. Effects by threshold:
- **Loyalty 1** — Native begins recognizing the player.
- **Loyalty 2** — Deposits Silverstone at the Obelisk.
- **Loyalty 3+** — Joins the player's escort ring. Army command from Shrine/Chambers.
- **Loyalty 5** — Full alliance; actively attacks enemies on sight.

Team status transitions: `neutral` → `player` (through loyalty) or `neutral` → `enemy` (through Goo corruption or losing a faction war). The **Militarize** Edict temporarily lowers the army loyalty threshold from 3 to 2.

---

## 14. Genetics & Reproduction

ANSELM features a complete hereditary genetics system governing native reproduction.

### Traits
Every native carries 10 genetic traits:

| Trait | Type | Effect |
|-------|------|--------|
| agility | Binary (0/1) | 1.8× movement speed; Crystal Gene flip rate 18% vs 10% |
| work | Binary | Gathering priority |
| social | Binary | Trade/dispute mediation frequency |
| faith | Binary | Scripture study acceleration |
| resilience | Binary | Combat HP recovery |
| curiosity | Binary | Exploration radius |
| hairColor | 0–5 | Visual appearance |
| skinColor | 0–3 | Visual appearance |
| dimension | 0–4 | Home dimension (affects mating compatibility) |
| gender | 0/1 | Required for mating (male + female only) |

### Mating Rules
Two natives can mate if:
1. They are different genders.
2. They are not from the same hut.
3. They do not share parents (inbreeding prevention).
4. They share enough trait matches — threshold = 4 for Gen 0 pairs; 5 for later generations, **reduced by the DIM_AFFINITY bonus** between their home dimensions.

### Offspring
Children inherit traits 50/50 from each parent with a **15% mutation chance** per child (one random trait flips). Cross-dimensional children have their `dimension` trait set to the rounded average of their parents' home dimensions. The **Crystal Gene** is inherited with 50% probability if either parent carries it.

### Crystal Gene
A rare heritable trait that appears post-Nexus-destruction. Crystal-gene natives:
- **Flip terrain tiles** — their tile is replaced by the matching tile type from a random other dimension (10% chance per tick, 18% with agility trait). After 10 accumulated flips, the dimension enters permanent **Dim Fusion**.
- **Channel the Nexus Remnant** — can spontaneously spawn small Goo patches (rare, on a 60-second cooldown per dimension).

Crystal-gene natives display a distinctive **purple-cyan shimmer** effect that marks their bloodline visually.

### Hybrid Score & DNA Diversity
The game calculates a `dnaScore` (0–1) based on trait variety and cross-dim representation. The `hybridBonus` (capped at 2×) rewards cross-dimensional mating, boosting civilization XP accumulation.

---

## 15. Boss Monsters

Each dimension has a unique Boss Monster — a large 64-pixel creature with distinct art, lore, and mechanics.

### Boss Names
- Dim 1 (Ashveil): *Void Leviathan*
- Dim 2 (Thalassorn): *Crystal Behemoth*
- Dim 3 (Verdekai): *Storm Wyrm*
- Dim 4 (Solenthar): *Bone Colossus*
- Dim 5 (Umbrakor): *Shadow Titan*

### Boss Behavior

**1. Spawn.** 45 seconds (2700 ticks) after a dimension is first visited. Spawns in the top-right corner.

**2. Hunting.** Every 5 seconds the boss searches for prey. 30% chance to aggro at the player; otherwise finds the nearest native or animal.

**3. Devouring.** Within 2.5 tiles of prey, the boss devours them instantly.

**4. Crystal Shield.** Each boss starts with a crystal HP shield that must be drained before actual HP can be reduced. **When the shield breaks**, the boss becomes **Enraged** — gaining +1.0 speed, triggering a screen shake, and displaying a warning message.

**5. Proximity Audio.** Heartbeat effect pulses faster and louder as the boss closes in, from 40 BPM at max hearing distance to 120 BPM at contact range.

**6. Crystal Ammo Bonus.** Crystal Ammo boosts damage to 1.4× (or 2× with overcharge), consuming one crystal per shot.

### Killing a Boss
The player must be within 4 tiles and carrying the dimension's weapon (or ultimate weapon). On death: multi-wave explosion with dimension-unique colors, loyal natives cheer, and a **Bone Fragment** is added to the inventory. The boss never respawns.

---

## 16. Shrines, Obelisks & Sacred Structures

### Shrines
Shrines are placed automatically adjacent to a Gateway each time the player leaves a dimension.

- **Mating encouragement** — The player can manually trigger 3 bonus mating attempts while standing within 5 tiles.
- **Gift-giving** — Natives near shrines periodically offer gifts (rings, shoes, accessories) that grant passive effects.
- **Scripture acceleration** — Each shrine reduces the scripture study interval by 400 ticks.
- **Army command** — Loyalty 3+ natives near shrines are assigned patrol routes and escort duty.

### Gift Effects (Stackable)
| Gift Type | Effect |
|-----------|--------|
| Speed shoes | +45% first pair, +20% each additional (capped at 2.2×) |
| Vision accessory | Wider viewport / vision radius |
| Regen ring | +1 HP per 120 ticks |
| Regen2 ring | +2 HP per 120 ticks |
| MSM ring | +1 MSM per mine |
| Goo Resist accessory | Halve Goo/Boss damage |
| Affinity accessory | Double gift rate from natives |

### Obelisk
Placed near the Forge after the first weapon is forged. Operates on a 10-second tick:
- Loyalty-2+ natives with a 40% chance each cycle deposit Silverstone (up to a stock of 50).
- When raw stock is present, 5:1 refining occurs automatically.
- The Obelisk's presence contributes -600 ticks to the scripture study interval.

### Chambers of Divine Enlightenment
The player's sacred house (`CHAMBERS` tile, index 36), placed near the Forge. Stepping on it opens the **Daedalus Codex** (see Section 20). Functions as a spiritual anchor and enables army command for loyalty-3+ natives.

---

## 17. The Chronicle Panel

Accessible via the `C` key or the `◈ CHRONICLE` button. A right-side drawer panel labeled **"CHRONICLE — LUX IN TENEBRIS · CIVILIZATIONAL RECORD."**

Every major event is logged to `civAnnouncements[]`:
- Civilization age transitions.
- Nexus destruction events.
- Lux chapter study milestones.
- Dimensional contact events.
- New dimension seedings.
- Philosophical schisms.
- Boss deaths.
- **War escalations and peace declarations (v3.7.0).**
- **Alliance / Rivalry formations (v3.7.0).**
- **Age 9 war resolution (v3.7.0).**

### Universe Panel
Accessible via `U` or `⊕ UNIVERSE`. Shows a full-screen cosmological overview of all five dimensions — their current ages, XP, active bosses, Goo coverage, and inter-dimensional contact status.

---

## 18. Save System

### Save Storage
Saves are written to `localStorage` under a fixed slot key.

### Auto-Save
Every 5 minutes (tick % 18000 === 0), the game auto-saves silently. A `#save-toast` notification ("● SAVED") briefly appears in the HUD.

### Manual Save
The Character Panel (P key) has a **"● SAVE JOURNEY"** button.

### buildSave() / applySave()
The save system serializes the full game state:
- Maps are stored as `seed + diffed tile array`.
- Goo `Set` objects are converted to arrays for JSON compatibility.
- `roadConnections` Sets follow the same pattern.
- On load, `waterMask` and `gooMask` Uint8Array caches are rebuilt from scratch.
- `chamberMode` is explicitly cleared to prevent a post-load player-vanish bug.
- **v3.7.0:** `dimWarLevel`, `dimTension`, `dimRelations`, and `dimRaidParties` are all included in save/load cycles.

### Export / Import
**Export:** Downloads the save as a JSON file. **Import:** An `<input type="file">` reads the JSON and restores state.

### Meta-Persistence (separate from the main save)
Three additional `localStorage` keys persist independently of the save slot (see Section 26):
- `anselm_ancestors` — top 3 native trait records.
- `anselm_heaven_dims` — which dimensions reached Heaven.
- `anselm_codex` — Lux in Tenebris chapter progress.
- `anselm_artifacts` — named forged weapons.

---

## 19. Chamber Room — Pre-Game Tutorial

**New in v3.7.0.** Before entering Ashveil, the player begins in a small **11×15 tile interior room** — the Chamber of First Light.

### Room Layout
The Chamber uses a simplified tile vocabulary:
- **Stone walls** border the entire room.
- **Floor** tiles fill the walkable interior.
- **Two wall torches** flicker on the left and right walls (rows 4 and 9).
- **Silverstone deposits** (2 tiles) in the lower-left area.
- **A Pink Crystal** in the lower-right area.
- **The Daedalus machine** — a tall, illuminated device centred at the top of the room.
- **Exit door** — at the bottom centre, locked until the Codex preamble is completed.

### The Daedalus Machine
The Daedalus is a 1-tile-wide, 2-tile-tall procedurally-drawn machine that dominates the top of the room. It must be **powered** before the Codex can be read:

1. Collect the Pink Crystal from the lower-right deposit.
2. Tap the Daedalus — it consumes one Pink Crystal MSM from inventory and activates.
3. A green glow pulses from the powered Daedalus (animated concentric rings).
4. Tapping it again opens the **Codex Preamble** briefing.

If the player taps the Daedalus without a Pink Crystal: `"Bring a Pink Crystal to power the Daedalus"`.

### Codex Preamble
A six-screen briefing sequence delivered through the Daedalus Codex panel:

| Screen | Title | Sub-title |
|--------|-------|-----------|
| 1 | TRAVELLER DETECTED | INITIALIZING BRIEFING PROTOCOL… |
| 2 | THE GOO | NEXUS CORRUPTION REPORT |
| 3 | THE NATIVES | INDIGENOUS CIVILIZATION STATUS |
| 4 | THE THREAT | HAZARD ASSESSMENT |
| 5 | THE MISSION | PRIMARY DIRECTIVE |
| 6 | YOUR JOURNEY BEGINS | END OF BRIEFING |

Tap to advance each screen. The final screen unlocks the exit door.

### Ambient Music — Fairy Fountain
The Chamber uses a fully procedural recreation of the **OoT Fairy Fountain theme** as its ambient music. Architecture:
- **Harp arpeggios** — 6-note D-major sweep per bar, bars 1–8.
- **Main melody** — the iconic 2-bar motif, ocarina-style sine with vibrato.
- **Sparkle chimes** — fast descending triangle-oscillator glitter run on bars 2 and 6.
- **Harmony pads** — D+F#+A sustained chord, detuned pair, soft shimmer.
- **Bass** — D2 root pulse, A2 fifth answer on beats 1 and 4.
- **Room echo** — every note spawns a 190ms delayed duplicate at 30% volume.

Key: D major. Time: 6/8, 52 BPM. 8-bar looping phrase. All synthesized with Web Audio API oscillators — no audio file required.

### Exiting the Chamber
Once `chamberPreambleDone` is true, the exit door opens (rendered as a lit archway). The player walks out, the Chamber canvas hides, and `enterDimension(0)` is called — the Goo is activated, Ashveil loads, and the main game loop begins.

---

## 20. Daedalus Codex

**New in v3.7.0.** The Daedalus Codex is an in-game encyclopedia accessible from the Chambers tile (stepping on it pauses gameplay and opens the panel) and from the title screen via a dedicated **CODEX ✦** button.

### Codex Structure
The Codex is a full-screen panel rendered over the game canvas. It has two access modes:

**In-Game Codex (Chambers tile).** Opened by `openCodex()`. Displays a menu with seven sections, each navigable:

| Section | Contents |
|---------|----------|
| NATIVES | Loyalty tiers (Neutral / Ally / Devoted), how to earn trust |
| DIMENSIONS | All five realms, animals, hazards, bosses, weapon. Locked until visited. |
| FORGE | Weapon recipes with live FORGED / NOT YET FORGED status per weapon |
| MINING | Silverstone ore tile, Obelisk refining, MSM crystal costs, Time Crystal planting, Ring of the Grid |
| GENETICS | All 10 traits, 50/50 inheritance, 15% mutation, Crystal Gene, Dim Fusion, Generations |
| CIVILISATION | Current age display, Ages 0–4, Extended Ages 5–10 with landmark building names, LUX study speed formula, Hyper-Holo-Grid |
| LUX IN TENEBRIS | **Full text of all 10 chapters** with ‹ PREV / NEXT › navigation. Each page shows the chapter number, title, complete body text, and theme tag. A live counter shows how far the current dimension has studied. |

**New in v3.7.0-1 — Forge live status.** The FORGE page now renders each weapon entry as `codexState.mode==='forge'` and checks `inventory.weapons[i]` to colour-code each entry **FORGED** (green) or **NOT YET FORGED** (amber), giving the player an at-a-glance weapon checklist.

**New in v3.7.0-1 — Full Lux Reader.** The LUX IN TENEBRIS page is no longer a simple chapter list. It renders the full body text of whichever chapter `codexState.luxChapter` is set to, using the game's own `LUX_CHAPTERS` array. Prev/Next buttons step between chapters; a MENU button returns to the top-level menu. The chapter counter ("Realm N has studied up to Chapter M") is shown at the top of every page.

**Title-Screen Codex (`openTitleCodex()`).** Reads `anselm_codex` (chapter progress), `anselm_artifacts` (named weapons) from localStorage and displays:
- Unlocked Lux chapters (gold; locked chapters show "??? Chapter N").
- The last 6 named artifacts forged, listed as `"◆ [Name]s [Weapon]"`.

### Rendering
The Codex panel uses its own CSS class set (`codex-*`) with teal-green color palette (`rgba(0,200,180,…)`) distinct from the main game UI. All navigation uses touch-safe event binding (`ontouchend` + `onclick`) to prevent ghost-touch issues on mobile.

---

## 21. World Events

**New in v3.7.0.** Every dimension has a stochastic world event timer that fires every 9,000–18,000 ticks (2.5–5 minutes). When it fires, a 40% chance triggers a random event from four types.

### Event Types

| Type | Effect |
|------|--------|
| **Meteor** | Announced 3 seconds before impact. A 3×3 area of rock tiles forms at a random map location, with a Silverstone deposit at the centre. Nearby natives flee the blast zone. Screen shake on impact. |
| **Storm** | `_dimStormActive` flag set for 600 ticks (10 seconds). Grants bonus civilization XP each tick (+10 per tick, ×1.75 in the Purple Realm). Ends with a "Storm subsides" message. |
| **Migration** | Calls `tryMigrateNative()` on the dimension. Wanderer-personality neutral natives begin crossing to other realms. Logged in the Chronicle as "The Great Migration." |
| **Eclipse** | `_dimEclipseActive` flag set for 600 ticks. All animals in the dimension enter flee mode. Ends with "light returns" message. |

World events begin triggering only after tick 3,600 (~1 minute into a dimension's life). The timer resets after each roll whether an event fires or not.

---

## 22. Edict System (E Key)

**New in v3.7.0.** The Edict System allows the player to issue dimension-wide royal decrees. Accessed via the **E** key or a dedicated panel button.

### Requirements
- **20 loyalty points** — The sum of all `loyalty` values across `team:'player'` natives in the current dimension.
- Only one active Edict per dimension at a time.
- Edicts last **5 minutes (18,000 ticks)**.

### Edict Types

| Type | Label | Effect |
|------|-------|--------|
| `build` | BUILD DECREE | Forces a construction action every 300 ticks for 5 min |
| `scripture` | STUDY DECREE | Doubles Lux scripture study speed for 5 min |
| `militarize` | MILITARIZE | Lowers army loyalty threshold from 3 to 2 for 5 min |
| `expand` | EXPANSION EDICT | Triples native migration speed for 5 min |

The active Edict and its remaining time display in the panel with a **CANCEL** option. Cancelling an active Edict clears it immediately.

---

## 23. Town Founding (F Key)

**New in v3.7.0.** The player can directly found new settlements anywhere in the active dimension.

### Requirements
- Player must be standing on a GROUND tile.
- Must be at least 20 tiles from any existing settlement.
- Nearby loyal natives (within 6 tiles) must collectively carry at least **30 wood + 20 stone**.

### Process
When requirements are met, the player's loyal natives pool their carried resources (30 wood + 20 stone consumed). A new town spawns:
- A **3×3 Great Hall** placed at the player's current position.
- Four **Huts** placed in cardinal directions (3 tiles out).
- Four pairs of native settlers spawn around the new Great Hall.
- The town is assigned a random specialty (Farming, Mining, Trade, or Craft).
- A Chronicle announcement logs the founding.

If resources are insufficient: `"Need 30 wood + 20 stone from loyal natives (have Xw Ys)"`.

### Animal Pen System (v3.7.0-1)
Towns with **10 or more hut positions** automatically attempt to build an animal pen every 40 seconds (`tick % 2400`). The process:

1. The town's neutral natives must collectively carry at least **8 Silverstone** — consumed on build.
2. The system searches for a **6×6 clear ground area** between 4 and 14 tiles from the Great Hall.
3. A fence perimeter is placed (all edge tiles of the 6×6 become `TT.FENCE`). The interior 4×4 is walkable.
4. The town records `hasFence: true`, `penCentre`, and `penBounds`.
5. Up to **3 wild animals** are scooped from the nearest wild animals in the dimension and teleported inside. Any shortfall is filled with freshly spawned animals.
6. Penned animals have `penned: true`, a halved speed, and bounded AI that keeps them within the pen interior.

The message `"Natives built an animal pen!"` appears when the pen is placed in the player's current dimension.

---

## 24. Diplomacy System (D Key)

**New in v3.7.0.** After two dimensions establish **Dimensional Contact** (Age 8+), their relationship begins evolving.

### Relationship States
- **Neutral** — Default. No special effects.
- **Allied** — Bonus effect: a 35% chance every 30 minutes that both allied dimensions trigger `tryMigrateNative()`, spreading high-generation natives between realms.
- **Rival** — When rivalry strength ≥ 80, spawns 3 cross-dimensional Raider natives in the player's current dimension every 2 hours.

### Formation
Every 3,600 ticks (~1 minute), `updateDiplomacy()` checks all contacting dimension pairs. If affinity is > 0.5, strength accumulates toward alliance (threshold: 60). If affinity < 0, strength accumulates toward rivalry (threshold: 60).

### Player Actions (Diplomacy Panel, D key)
Available once contact exists:

- **BROKER PEACE (10 refined Silverstone)** — Resets a Rival relationship to Neutral.
- **ALLIANCE BONUS (5 refined Silverstone)** — Grants +5,000 XP to both allied dimensions.

### Raiders
When a Rival escalates to spawning Raiders, 3 enemy-team natives appear at the centre of the player's current dimension with the name "Raider." They have an agility-1 trait and hostile AI — they chase the player and attack loyal natives.

---

## 25. Realm Tension & War Escalation

**New in v3.7.0.** Each dimension tracks a **Tension** meter (0–100) that drives internal war escalation.

### Tension Sources (checked every 600 ticks)
- Tree count < 20 in the dimension: **+3 tension/cycle**
- Silverstone/Rock count < 8: **+2 tension/cycle**
- Active faction dispute: **+4 tension/cycle**
- Goo coverage > 50 tiles: **+2 tension/cycle**
- Passive decay: **−1 tension/cycle**

Tension is locked to 0 while `dimTensionLockUntil` is active (peace cooling-down period).

### War Levels

| Level | Name | Effect |
|-------|------|--------|
| 0 | Peace | No special effects |
| 1 | Skirmish | Minor dispute frequency increase |
| 2 | Conflict | Increased dispute casualties |
| 3 | War | Raid parties spawn every 2 hours |
| 4 | Total War | Raid parties may carry bombs |

War escalates when Tension > 70 (checked every 1,800 ticks). Each escalation step logs a Chronicle announcement.

### Raid Parties
When War Level ≥ 3, `spawnRaidParty()` is called: a group of enemy-team natives with the origin dimension's traits spawn at the map edge and march toward the player's Forge. At War Level 4, there is a 50% chance each raid party carries bombs (`hasBombs = true`).

### War Resolution
- Reaching **Age 9 (Hyper-Holo-Grid)** automatically ends all wars in that dimension, setting War Level and Tension to 0.
- Peace can also be enforced via the Diplomacy Panel for rival cross-dimensional relationships.

---

## 26. Meta-Progression

**New in v3.7.0.** ANSELM now has four `localStorage` keys that persist independently of the main save slot, creating continuity across separate runs.

### Ancestor Memory (`anselm_ancestors`)
On save, `saveAncestors()` identifies the top 3 natives by generation number across all five dimensions and serializes their traits, name, generation, and bloodline. On new game start, `loadAncestors()` applies these trait sets to random starting natives in Dim 0 — "Ancient bloodlines remembered" message fires if any ancestors exist.

### Heaven Memory (`anselm_heaven_dims`)
`saveHeavenDims()` records which dimensions reached Heaven (Age 4) or higher. `applyHeavenMemory()` on new game start grants bonus starting XP to those dimensions and spawns 2 "Returner" natives (wanderer personality, Gen 1, high curiosity/social traits) per remembered Heaven dim.

### Codex Progress (`anselm_codex`)
A boolean array of 10 values tracking which Lux chapters have been studied in any previous session. Displayed in the title-screen Codex panel. Only unlocked chapters show their titles and themes; locked chapters are obscured as "??? Chapter N."

### Named Artifacts (`anselm_artifacts`)
When a weapon is forged with Generation 3+ natives present, a native's first name is prepended to the weapon name (e.g., "Greyonnen's Tidecaller") and appended to the `anselm_artifacts` array. These persist across sessions. The title-screen Codex shows the last 6 artifacts. The in-game Codex shows the full list.

---

## 27. Native Quests

**New in v3.7.0.** Every 4,800 ticks (~1.3 minutes), `updateNativeQuests()` checks each dimension. If no quest is active, a random neutral named native may offer one of three quest types.

### Quest Types

| Type | Requirement | Reward |
|------|-------------|--------|
| `stone` | Bring 5 Silverstone to the quest-giver native | Town happiness +15, native loyalty +2 |
| `escort` | Stay within 3 tiles of the native for 1,800 ticks (~30 seconds) | Native permanently joins your team at loyalty 3 |
| `travel` | Leave the dimension and return while the native is present | Native becomes a trader; dimension gains +20 XP |

Quests are announced when the player is in the relevant dimension: *"[Name] has a request — find them in Realm N."* Only one quest per dimension can be active at a time. The quest-giver native must be a non-Goo-corrupted neutral with a name of at least 3 characters.

---

## 28. Farm Growth System

**New in v3.7.0-1.** Farms now track explicit growth state via `farmGrowthTimers` — a per-dimension object mapping tile index → due tick.

### Growth Cycle
1. When a farm tile is placed by `tryBuildFarm()`, a timer is set: `tick + 7200 + random(0–3600)` (~2–3 min first crop).
2. `updateFarms()` runs every 600 ticks (~10 seconds). When the due tick is passed, the harvest cycle fires:
   - The town's `fedUntil` counter is extended by 3,600 ticks — reducing the next dispute chance.
   - The timer resets to `tick + 1800 + random(0–3600)` (~30 sec–2 min between subsequent harvests).
3. Mills accelerate the food chain: each 60-second mill tick converts 1 unit of `meatStock` to `fedUntil2` (fed for 7,200 ticks per unit), with `meatStock` produced by farms at a rate scaled by `farmLevel`.

### Farm Construction
`tryBuildFarm()` checks every 50 seconds. Requirements:
- Dimension at Settlement age (1+).
- Town has at least 4 hut positions.
- Nearby neutral natives collectively carry ≥ 5 wood (consumed).
- A clear GROUND tile exists within 3–12 tiles of the Great Hall.

A town can have exactly one farm. The farm tile index is stored as `town.farmIdx` for the mill and update systems to reference.

---

## 29. Technical Architecture

### Single-File Design
The entire game — HTML, CSS, JavaScript (16,193 lines in v3.7.0-1), and all assets — lives in a single `.html` file. There are no external dependencies except two Google Fonts (Cinzel serif + Share Tech Mono) and no file reads at runtime.

### Canvas Rendering (2D)
- **Primary canvas** (`#gameCanvas`) renders the full game world.
- **Chamber canvas** — a second full-screen canvas overlaid during the pre-game Chamber Room sequence.
- **Camera** tracks the player with a smooth lerp.
- **Zoom** uses 5 snap levels (0.25×, 0.5×, 1×, 1.5×, 2×), interpolated each frame.
- The minimap (M key) uses a second canvas overlay.
- **Tile rendering** draws directly with `ctx.fillRect()` calls — no sprite sheets, no images.

### Performance Optimizations (v2.3.5+)
- Removed per-tile `createLinearGradient()` calls — replaced with simple alpha strips.
- `waterMask` and `gooMask` are `Uint8Array` caches on the map object.
- `ROAD_SET` constant defined outside `roadNeighbours()` (no Set allocation per call).
- `respawnMap` and `townSpecMap` pre-built Maps before the tile loop each frame.
- `countTrees()` is frame-cached — no 4800-tile scan per frame.
- Frame time capped at 50ms max delta + `visibilitychange` pause.

### Web Audio API (Procedural Sound)
There are **zero audio files**. Every sound is synthesized in real time.

**ANSELM Title Theme (v3.7.0-1 — fully recomposed).** The landing-screen and title-screen theme is now a complete composed piece:

- **Key / tempo:** A minor (root A3 = 220 Hz), 56 BPM, 4/4 time.
- **Structure (5 parts + tail, ~2.5 min looping):**
  - **Part A (bars 0–7):** Lone bell tolls A3 twice; solo ocarina states the 6-note ANSELM motif (A4→G4→E4→C5→B4→A4 = semitones 12,10,7,15,14,12). No bass.
  - **Part B (bars 8–15):** Bass drone enters (A2 root, E2 fifth, D2 IV colour). Motif repeats with an answering phrase.
  - **Part C (bars 16–23):** Harp arpeggios bloom; harmony pads swell; a hope arc rises through C major colour.
  - **Part D (bars 24–31):** Full texture — all six voices simultaneously. Counter-melody weaves above the bass. Peak emotional moment.
  - **Part E (bars 32–39):** Strip back — pads fade; motif alone again; quiet ending.
  - **Tail (bars 40–43):** Final A3 bell toll, silence, seamless loop.
- **Six voice timbres:** Bell (sine, fast decay, light reverb), Ocarina (pure sine, slow vibrato), Bass pad (two detuned sines, very low, slow swell), Strings (sawtooth through lowpass, warm chords), Harp (triangle plucks, fast decay, short echo), Counter (sine one octave above bass, weaving line in Part D).
- The ANSELM motif is defined as `const MOTIF = [12, 10, 7, 15, 14, 12]` and forms the thematic spine of the entire piece.

**Chamber Fairy Fountain Ambient** — unchanged from v3.7.0: D major, 6/8, 52 BPM, OoT-accurate, five voice layers with 190ms room echo.

All other sounds remain: mining tinks, forge clang, gateway whoosh, boss heartbeat, heaven transition swell, civilization age-up fanfare, ambient pad, and water ambience.

### Game Loop
`requestAnimationFrame` drives the loop. Each frame:
1. `renderMap()` — draw all visible tiles (including the six new landmark tiles via their own draw functions).
2. `drawPlayer()` — drawn immediately after map, before AI updates.
3. All AI update functions (natives, goo, bosses, vehicles, animals, particles, etc.).
4. `updateTimeCrystalPlants()`, `updateTension()`, `updateWarEscalation()` — v3.7.0 additions.
5. `updateFarms()`, `tryBuildFence()` — farm growth cycles and animal pen construction checks (v3.7.0-1).
6. Draw particles, projectiles, campfires, atmospheric overlays.

### Map Generation
Each map is generated from a seeded LCG random number generator (`makeRNG(seed)`). The generator deterministically places: water bodies, forests, rock clusters, MSM veins, Silverstone deposits, towns, Forge, Gateway, Shrines, and post-Heaven structures. Seeds are stored in saves for deterministic map regeneration.

### Input
- **Tap/click** — Pathfinding to target tile.
- **Keyboard** — U (Universe), C (Chronicle), P (Character Panel), E (Edicts), D (Diplomacy), F (Found Town), +/- (zoom), 0 (reset zoom), M (minimap).
- **Mouse wheel** — Zoom.
- **Pinch-to-zoom** — Two-finger distance delta triggers zoom steps.

### CSS
- Root color variables for the five dimensions (`--dim1` through `--dim5`).
- Cinzel serif for all titles and UI headings; Share Tech Mono for data and labels.
- The Daedalus Codex panel uses its own `codex-*` CSS class hierarchy with teal-green palette.
- Screen system uses `opacity:0 / pointer-events:none` for inactive screens.

---

*Report compiled from source analysis of ANSELM v3.7.0-1 (16,193 lines), dated 2026-04-04.*
