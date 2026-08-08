# VELLUM — handoff

A survivors-like with an ARPG loot/crafting layer. Single file, zero dependencies,
zero assets. Everything is `index.html`.

**Live:** https://vellum-five-gules.vercel.app
**Repo:** https://github.com/draphael123/vellum
**Local:** `python -m http.server 5798 -d .` → http://localhost:5798

---

## The one thing that must not be broken

The design has a single spine, and every system is a link in it:

```
card  →  horde trait  →  material  →  affix keyword  →  Doctrine
```

You take a level-up card. Its **second half writes a trait onto the horde**. That
trait is the *only* thing that makes its material drop. That material crafts gear
whose affixes carry a keyword. Three of one keyword fires that Doctrine's capstone.

So "make the run more dangerous in this specific direction" **is** the farming
decision. If a change severs any link in that chain — e.g. materials start
dropping without the matching card, or affixes stop carrying keywords — the game
loses its entire reason to exist. Everything else is negotiable.

Three rules that follow from it, and should be held:

1. **Affixes are behavioural, never statistical.** `+7% attack speed` is invisible
   with 300 husks on screen. `third swing knocks back`, `pools chain`, `crits
   fork` are visible. Every affix must change a shape on screen.
2. **No in-run inventory.** Drops auto-compare against the equipped slot, `F`
   equips, everything else shatters to materials on the spot. Bags exist only in
   the Forge. A pause to manage a bag leaks the tension the horde built.
3. **One ritual, not two.** Loot is the in-run reward, crafting is the between-run
   meta, and the level-up draft is *only* the bestiary half. Don't add a second
   competing progression system.

---

## Code map (`index.html`, top to bottom)

| Section | What's in it |
|---|---|
| palette / tuning | `ZONE_TIME` (480s to the Warden), `WORLD`, `MAX_ENEMIES` |
| **ART ENGINE** | `mulberry32`, `roughShape`, `hatch`, `limb`, `bakePaper`, `bake*` |
| GEAR SYSTEM | `IMPLEMENTS`, `AFFIXES`, `RARITY`, `MATERIALS`, `makeItem` |
| doctrines | `DOCTRINES`, `computeDoctrines` |
| BESTIARY | `CARDS` — the six pages, each with `boon` / `curse` / `trait` / `mat` |
| SAVE | `localStorage` key `vellum_save_v1` |
| spawning / boss | `spawnRate`, `ENEMY_KINDS`, `spawnBoss`, `updateBoss` |
| combat | `fireWeapons`, `damageEnemy`, `killEnemy` (**the material drop table**) |
| update | `step(dt)` — the whole sim |
| draw | `draw()` — dispatches by `G.mode` |
| UI | `drawHud`, `drawLevelUp`, `drawForge`, `drawMenu`, `drawEnd` |

**Art is baked once at load** into offscreen canvases (`bakeArt()` after
`resize()`), so a 400-strong horde costs one `drawImage` each. If you add a
creature, add a `bake*` for it and register it in `bakeArt`, including a
`SPRF` flash variant.

---

## Test surface (no build step, just the console)

```js
G                      // whole game state
step(dt)               // one tick
sim(seconds)           // run N seconds headless, returns a summary
VELLUM.startRun()
VELLUM.takeCard(card)
VELLUM.makeItem(slot, rarity, keyword, baseKey)
VELLUM.equip(item)
VELLUM.aimAt(worldX, worldY)   // ← REQUIRED for any headless test
VELLUM.spawnBoss()
VELLUM.reset()         // wipes the save
```

**`VELLUM.aimAt()` is not optional.** `mouse` is module-scoped, so without it a
headless bot's aimed auto-fire all goes at the screen corner and every kill
number you measure is garbage. This cost a full wasted test round.

Screenshots: `computer{screenshot}` wedges on this canvas. Use
`document.getElementById('c').toDataURL('image/jpeg', 0.9)` and POST it to a
local file-writing server instead.

---

## State of things

**Verified working.** Doctrines compute; skeleton cap holds (6 raised from 40
kills); the Warden spawns, cycles all four attack states, phases at 50%, and
dies; `draw()` is clean in all seven modes; ~1.2–1.5 ms/frame at 420 enemies, so
there is a lot of perf headroom.

**Balance is UNVALIDATED and should not be tuned from a bot.** A scripted kiting
policy produced a 235–6167 kill spread across six runs. It can confirm systems
fire; it cannot tell you whether the game is fun or fair. Two fixes already made
were structural rather than numerical, and are worth knowing:

- Anything **flat-per-kill** breaks instantly — at ~6 kills/sec a 3 HP heal per
  kill outran every damage source in the game. Grave-Rite is now every 8th kill.
- **Enemy speeds must be set as a fraction of player speed (186).** They were
  under 25% at first, so a moving player outran the entire zone and never fought.
  Now husk 104 / creeper 158 / bloat 62.

### Bugs already fixed — please don't reintroduce

- Level-ups resolve at the **end** of `step()` via `p.pendingLevels`. Resolving
  inline let a level-up overwrite `mode='dead'` when a fatal hit and an XP pickup
  landed in the same frame.
- `drawLevelUp` iterates a **snapshot** of `G.offer` and breaks after a click —
  `takeCard` clears the live array mid-loop and crashed the frame.
- Scorch pools are capped at 64 by `addPool()`, which folds a new pool into the
  nearest existing one. They spawn per-death and each ticks the whole enemy list.
- Shrieks are **globally** rate-limited in `updateShrieks`. Per-enemy timers meant
  either never firing or an O(n²) sweep.
- The hit flash uses pre-baked parchment silhouettes (`SPRF`). Compositing with
  `source-atop` at draw time whitewashed a **rectangle** of the scene, because the
  operation applies to the whole canvas, not the sprite.
- Woodcut shading must stay a **narrow band on one flank**, clipped by a rect as
  well as the body. `hatch()` strokes across the entire clip region, so an
  unclipped call wraps the torso and the creature reads as bandaged. The first art
  pass shaded ~60% of every body and turned the horde into pale striped mush.
- The paper tile is **high-frequency only**. Broad tonal drifts baked into a
  256px tile read as an obvious repeating quilt the moment the camera moved.

---

## August 2026 expansion

- The static Forge is now reached through a walkable Scriptorium. Walk with
  WASD and press `E` at the Ink-Forge, Bestiary, or Orchard Gate.
- The Bestiary contains twelve pages, up from six.
- Three later-run enemy roles were added: rootlings, needlekin, and bellowers.
- Three additional Orchard roles broaden the first zone: Inkspitters telegraph
  ranged quills, Broodpods split into Rootlings, and Stumpguards act as durable
  moving walls. Their arrivals begin around minutes 2, 4.4, and 6 respectively.
- Material drops now name themselves in-world when collected from a kill.
- One fixed active ability establishes the baseline without creating another
  progression track: `Q` Margin Cut drives the surrounding crowd outward. It
  is saved as the Greatsword's active inscription; Censer and Longbow currently
  display an intentional "not yet discovered" state and have no active HUD.
- Trees, broken printer-rule walls, and local crowd separation
  now create lanes and funnels. Collision is deliberately soft to avoid jams.
- The combat HUD now uses Doctrine seals, skill cooldown plates, an in-world
  ward recharge ring, and a staged first-run tutorial.
- The Orchard is one mechanical zone with six visual districts, five authored
  landmarks, rotating formation waves, and rare gilded elites. This creates
  exploration and encounter diversity without adding a second material line.
- Page selection now resolves with a short transformation stamp showing the
  horde trait and its material colour. Player damage has an edge impression.
- Newer creature families have baked silhouette additions rather than relying
  only on runtime colour marks.
- Zone 2 and audio remain deliberately out of scope.

## Suggested next work, roughly in order

1. **Human playtest first.** The open questions are: does the curse half of a page
   read as exciting or as a tax; is 8 minutes the right zone length; does the
   Forge give a real reason to run twice. Everything below is downstream of that.
2. **Tune the new enemy arrival windows** with human play rather than simulations.
3. **Tune the six new page pairings** after testing which curse directions
   create readable pressure in a full run.
4. **A fourth implement**, and a second Doctrine tier at 7 keywords.
5. Mobile / touch controls. Currently keyboard + mouse only.

Things deliberately **not** built: enemy variety beyond three kinds, run modifiers,
meta skill tree (rejected — it would be a third progression system), any art
assets (everything is drawn in code, and should stay that way).
