# Valley — a theme for Wasteland Next

Parchment and bronze instead of amber on black. It dresses the whole
application, and it dresses the game panel in particular.

## What it changes

**Everywhere.** The palette is warmer and quieter: ink-on-parchment text on a
brown-black ground, moss instead of a signal green, and the amber glow turned
most of the way down. That glow is what smears small text over a busy picture,
and a game with artwork in it has busy pictures.

**In the game.** If [Fantasy RPG](https://github.com/awakeserg/wasteland-plugin-fantasy-rpg) — or
anything else using the `scene` service — is running, three surfaces get more
than a colour swap:

- **the panel** — taller meters with their own fills, so a bar reads as full or
  nearly empty from across the desk rather than as a thin line;
- **the character sheet** — set in a serif, because the inventory, the journal
  and the quest are read as prose and a terminal face is the wrong tool for it.
  The strip above stays monospace: numbers in columns have to line up;
- **the map** — a frame and a deep inset shadow, so a drawn map looks like a map
  rather than like a screenshot of one.

Both of those open as screens rather than as windows: sized to the display
instead of to their contents, with the lists in two columns where there is room
for two and the close button at the foot rather than wherever the last list
happened to end. A dialog that is 400px tall on a day with three journal entries
and 900px on a day with twelve is a panel that moves under the reader.

## It is not only for the game

The palette applies to the whole application — the model vault, the plugin list,
the transcript, the composer. The game rules simply match nothing when no game
is running, so with the plugin absent this is an ordinary colour theme and
nothing is missing or broken.

## Installing

There is nothing to run. A theme is a manifest and a stylesheet, read by the
application itself, so it needs no approval and no rebuild.

**From a registry — the ordinary way.** In Wasteland Next: **GET PLUGINS →
REGISTRIES**, and add

    https://github.com/awakeserg/wasteland-plugin-valley-theme

Valley appears in the list under APPEARANCE; press **INSTALL**. Updates arrive
the same way — the app compares versions and offers the new one.

**From an archive**, if a registry is out of reach or a particular build is
wanted: `npm run index -- --base-url=https://example.invalid` writes one into
`dist/`, or zip `plugins/valley-theme` by hand so that `plugin.json` sits at the
root of the archive. Then **GET PLUGINS → FROM FILE…** and pick it.

Either way: restart, then **INTERFACE → THEME → Valley — parchment**.

Needs a build with plugin API 2 or newer, which is every build that has themes
at all.

## How this is published

The repository is its own registry. `index.json` at the root of `main` is what
the app reads: it names the exact archive and its checksum, and without matching
checksums the app installs nothing.

- `plugins/valley-theme/` — the theme itself, exactly what goes into the archive.
- `scripts/build-index.mjs` — packs the directory and writes `index.json` with the digest.
- `scripts/prune-release.mjs` — takes the archives the index no longer names off the release.
- `.github/workflows/release.yml` — on a push to `main` touching `plugins/**` or
  `scripts/**`: runs the tests, packs, uploads the archive to the release tagged
  `plugins`, commits the updated `index.json` and prunes the old archives.

The scripts and the workflow are copied unchanged from the other published
Wasteland Next plugins; only the id in the `concurrency` group differs. So a fix
to any of them can be carried across the same way.

To release a version: raise `version` in `plugins/valley-theme/plugin.json`,
describe the change below, and push to `main`. The workflow does the rest. An
archive uploaded by hand with no regenerated index is invisible to the app.

One trap worth knowing, because it cost an afternoon on the game's repository:
the workflow's path filter has nothing to compare against on the push that
creates a repository's first branch, so the first push of a brand new repository
fires nothing at all. `workflow_dispatch` is the way out — Actions → Publish the
plugin → Run workflow, once.

**Everything committed here is written in English** — the README, the
stylesheet's comments, the commit messages and any screenshot.

## Layout

- `plugins/valley-theme/plugin.json` — the manifest;
- `plugins/valley-theme/themes/valley.css` — the theme;
- `plugins/valley-theme/icon.svg` — the picture on its row in the plugin list;
- `tests/manifest.test.mjs` — the manifest is true about the directory it sits in;
- `tests/prune.test.mjs` — what to take off a release and what to keep.

The theme lives in `plugins/<id>/` rather than at the root because that is the
layout the publishing scripts expect, and the directory has to be named exactly
like the manifest's `id`.

## How it is put together

The file has two halves, and the difference between them is deliberate.

**The palette is nothing but `:root` variables.** A theme is loaded after the
application's own stylesheet, so equal-specificity rules win on order alone and
no `!important` is needed anywhere. Overriding the app's selectors would break
the next time it adds a rule, so this half never does.

**The second half overrides selectors, and only these:** `.scene`, `.sheet-*`
and `.board-*`. Those arrived with the game and are the whole reason for a theme
aimed at playing rather than at reading a terminal. That is the one bargain
being struck, and it is worth knowing about if a future version of the
application moves them.

Nothing here sets `display` on anything the application toggles with `hidden` —
`.scene`, `.scene-actions`, `.modal`, `.player`. The weakest author declaration
outranks the whole user-agent sheet, so a `display` on one of those would pin it
open forever. Wasteland Next's own stylesheet carries a comment about that trap,
which is how it is known to be one.

## Two things found while making it

Both were caught by rendering the real markup offscreen and looking at it, which
is the only way this kind of thing can be caught.

**The bars read as empty.** A meter's fill is the body text colour, and on a
parchment palette that is a pale cream on a dark ground — nearly invisible. The
fills are set explicitly now, and the three tones keep their meanings.

**The last move fell off the row.** Roomier buttons need more room: the
application caps the height of the action row so that a game offering ten moves
cannot push the composer off a short window, and that cap has to move with the
padding or the last row ends up behind a scrollbar.

## What changed in 1.2.0

- **Published from this repository.** It used to be an archive handed over by
  hand, which means no update ever arrives on its own. The registry, the release
  workflow and the checksum that ties the index to the bytes come from the same
  scripts the other plugins use.
- **An icon**, so the row in the plugin list is something to recognise rather
  than something to read.

## Licence

Apache 2.0, the same as Wasteland Next.
