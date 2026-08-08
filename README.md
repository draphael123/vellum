# VELLUM

**You are writing the bestiary you are standing in.**

VELLUM is a hand-drawn browser action game set inside a living manuscript. Every page you choose strengthens the Binder and writes a new law into the creatures hunting them. Survive the Rotting Orchard, learn its landmarks, shape a build, and finish the Orchard-Warden's final clause.

**[Play VELLUM](https://vellum-five-gules.vercel.app/)**

## What is in the current build

- One complete eight-minute combat zone with six visual districts, six interactive landmarks, authored formations, rare elites, a wandering Scribe, the Rubric Procession encounter, and a two-phase boss with five attack forms.
- Nine regular enemy families with distinct roles: pursuit, ranged pressure, support calls, splitting broods, lunges, and protective formations.
- Twelve manuscript pages. Each page grants a gift, writes a curse into the horde, and opens a material line for the Forge.
- Three implements with different relationships to cover: Greatsword cleaves, Longbow pierces, and Censer lobs over obstacles.
- Two active inscriptions per implement. The alternate Greatsword, Longbow, and Censer skills open through field mastery; no additional weapon was added in this release.
- Three pre-run Binding Conditions that trade increased danger for richer material impressions.
- A Living Orchard simulation: telegraphed press slams, weapon-felled deadfalls, root snares, flammable ink channels, Bell Trees, tearing margins, Warden shrine consequences, and four weather impressions.
- A persistent Scriptorium containing the Ink-Forge, Orchard Gate, safe Proofing Yard, unlock monument, page ledger, creature bestiary, and local run ledger.
- Two CC0 musical loops, a full procedural sound-effect layer, page-turn transitions, hit weight, contextual defeat advice, touch controls, and persistent reading settings.
- Authored animation and audio beats for inscriptions, elites, low health, forging, the Warden's second impression, and victory.
- A two-charge Ink-Step with perfect-dodge rewards, enemy break states, rubric executions, crowd collisions, weapon-specific movement, clearer attack tells, Censer reactions, pinned quills, combat composition bonuses, and adaptive musical intensity.
- A presentation pass with district printer seals, drifting marginalia, off-page threat quills, grouped manuscript HUD slips, and an animated, inhabited Scriptorium.
- Versioned feedback links and an exportable, browser-local playtest report for balance feedback without analytics or accounts.

## Controls

| Action | Keyboard and mouse | Touch |
| --- | --- | --- |
| Move | `WASD` | Drag on the left side |
| Aim / attack | Mouse | Drag on the right side |
| Active inscription | `Q` | Tap the inscription plate |
| Equip found gear | `F` | Tap the take action |
| Interact in town | `E` | Tap a nearby site |
| Pause / return | `Esc` | Tap the pause or return control |
| Reading conditions | `O` | Open Options |

The Options page can disable sound, camera and page-turn motion, loose-ink particles, or role-based threat rings, and can enable a darker high-contrast impression. New saves also respect the device's reduced-motion preference. The first-run guide can be replayed at any time.

## The build loop

1. Enter the Orchard with the implement and gear prepared at the Ink-Forge.
2. Choose pages as the Binder levels. The boon is immediate; the curse changes the rest of that run.
3. Use trees, fallen timber, roots, and letterpress ruins to divide enemy formations.
4. Bring recovered materials and gear home. Material types correspond to the laws previously written into the bestiary.
5. Defeat the Warden to open additional weapon inscriptions and mark the town's central seal.

## Running locally

VELLUM has no build step or external runtime assets. Serve the repository with any static web server and open `index.html` through that server.

For example:

```sh
npx serve .
```

## Verification

The current game-feel build has been syntax-checked, visually exercised in-browser, and run through an automated full-zone systems smoke covering all twelve pages, all ten recorded creature species, every active inscription, the Warden's second phase, and the victory transition. The smoke harness is intentionally invulnerable; numerical difficulty still requires human playtesting.

Detailed implementation notes and playtest priorities are recorded in [HANDOFF.md](HANDOFF.md).
