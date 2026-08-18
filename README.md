# Valley — a theme for Wasteland Next

Parchment and bronze instead of amber on black. It dresses the whole
application, and it dresses the game panel in particular.

## What it changes

**Everywhere.** The palette is warmer and quieter: ink-on-parchment text on a
brown-black ground, moss instead of a signal green, and the amber glow turned
most of the way down. That glow is what smears small text over a busy picture,
and a game with artwork in it has busy pictures.

**In the game.** If [Fantasy RPG](https://github.com/awakeserg/fantasy-rpg) — or
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

1. Download the archive, or zip this directory so that `plugin.json` sits at the
   root of it.
2. In Wasteland Next: **GET PLUGINS → FROM FILE…**, pick the archive.
3. Restart, then **INTERFACE → THEME → Valley — parchment**.

Needs a build with plugin API 2 or newer, which is every build that has themes
at all.

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

## Licence

Apache 2.0, the same as Wasteland Next.
