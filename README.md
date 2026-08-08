# VELLUM

**You are writing the bestiary you are standing in.**

VELLUM is a hand-drawn browser action game set inside a living manuscript. Every page you choose strengthens the Binder and writes a new law into the creatures hunting them. Survive the Rotting Orchard, learn its landmarks, shape a build, and finish the Orchard-Warden's final clause.

**[Play VELLUM](https://vellum-five-gules.vercel.app/)**

## What is in the current build

- One complete eight-minute combat zone with six visual districts, six interactive landmarks, authored formations, rare elites, and a two-phase boss.
- Nine regular enemy families with distinct roles: pursuit, ranged pressure, support calls, splitting broods, lunges, and protective formations.
- Twelve manuscript pages. Each page grants a gift, writes a curse into the horde, and opens a material line for the Forge.
- Three implements with different relationships to cover: Greatsword cleaves, Longbow pierces, and Censer lobs over obstacles.
- One active inscription per implement. Margin Cut is available immediately; Redline Volley and Black Bloom open through Warden victories.
- A persistent Scriptorium containing the Ink-Forge, Orchard Gate, unlock monument, page ledger, and creature bestiary.
- Procedural manuscript audio, page-turn transitions, hit weight, contextual defeat advice, touch controls, and persistent reading settings.

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
