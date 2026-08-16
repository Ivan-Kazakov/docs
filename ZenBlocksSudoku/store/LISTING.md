# Google Play store listing — Zen Blocks Sudoku

**Last updated:** August 16, 2026

The copy submitted to Play Console, kept here so a future edit starts from what is actually
published rather than from memory. Character limits are Google's.

---

## App name (limit 30)

```
Zen Blocks Sudoku
```
*17 characters.*

---

## Short description (limit 80)

```
A calm 9x9 block puzzle: drop shapes, clear rows, columns and 3x3 boxes.
```
*71 characters.*

Alternative, if the listing needs to lean harder on the no-pressure angle:

```
Relaxing block puzzle. No timer, no lives — just clean lines and a clear head.
```
*77 characters.*

---

## Full description (limit 4000)

```
Drop blocks. Clear lines. Breathe.

Zen Blocks Sudoku is a 9x9 block puzzle with no timer, no lives and no
countdown. Nothing rushes you and nothing punishes a slow move — the only
thing that ends a game is running out of room. Play it for two minutes in a
queue or for an hour on the sofa.

HOW TO PLAY
• Drag a piece from the tray onto the 9x9 board.
• Fill a whole row, a whole column, or one of the nine 3x3 boxes, and it
  clears.
• Clear more than one at a time for a bigger score.
• When the tray runs empty, the next pieces arrive.
• The game ends when nothing you are holding fits any more.

EVERY HAND CAN BE PLAYED
Most block puzzles hand you pieces at random and let you lose to bad luck.
This one doesn't. Before a set of pieces is offered, the game checks that
they can actually be placed, one after another, on the board in front of
you. If you lose, it's because of a move you made — not because the game
dealt you something impossible.

FOUR WAYS TO PLAY
• Default — three pieces at a time, just as they come.
• With Rotation — pieces arrive at random angles, and you turn them to fit.
• More Shapes — six pieces on screen instead of three. More to work with,
  more to plan around.
• Custom — set your own rules: three to six pieces, rotation on or off.

ROTATE WITH A SECOND FINGER
There is no rotate button cluttering the screen. While one finger holds a
piece, tap anywhere with another finger and it turns. The piece stays under
your thumb the whole time.

ONE MORE GO
When the board finally jams, you can carry that game on once instead of
starting over. A few of the fullest boxes are cleared to give you real room
to keep playing.

PICK UP WHERE YOU LEFT OFF
Close the game mid-move and it waits for you. The board, the score and the
pieces in your tray are all still there when you come back.

THREE LOOKS
Zen, deep blue and warm. Paper, light and quiet. Classic, plain black and
white. They sit as three little tiles down the side of the menu, so you pick
one by looking at it — and the board changes behind you as you tap.

FITS YOUR PHONE
Plays in portrait or landscape and follows the phone as you turn it. Sound
and vibration can each be switched off. Works fully offline — no connection
needed to play.

PRIVATE BY DESIGN
No accounts. No sign-in. No analytics. Your progress and your best score
stay on your device and are never sent anywhere. Advertising is served by
Google and can be removed with a single one-time purchase — no
subscriptions, ever.
```
*≈2,100 characters.*

---

## Notes for whoever edits this next

- **"EVERY HAND CAN BE PLAYED" is a factual claim, not marketing.** `Board.getShapesSuggestions`
  simulates each candidate piece against the board state, chained across the batch, so the set
  offered really is placeable in sequence. Do not soften it into "no impossible levels" (there are
  no levels) and do not harden it into "you can never lose" (you can, by playing badly).
- **"ONE MORE GO" deliberately does not mention the rewarded ad** in that paragraph; the closing
  paragraph states plainly that advertising exists and can be removed. If Play ever asks for the
  mechanism to be explicit, add "by watching a short video, or straight away with the ad-free
  unlock."
- **Nothing here promises leaderboards, daily challenges, achievements or cloud saves.** None of
  them exist. Keep it that way unless they do.
- The game modes are named as `GameMode.TITLES` names them, so the listing and the mode screen say
  the same words. The first mode is **Default**, not "Classic" — Classic is a colour scheme, and
  using the word for both is how a player ends up looking for a colour in the mode menu.
- Feature list must stay in step with `GameMode` (four modes today) and with `ColorScheme` (three
  schemes today). "THREE LOOKS" names them; a fourth scheme makes that line wrong.

---

## Assets in this folder

All PNGs are 24-bit with no alpha channel, which Play requires. Screenshots are real renders of
`Scenes/root.tscn` at each target size, not upscales: the window is grown at run time with
`DisplayServer.window_set_size`, which — unlike the `--resolution` command-line flag — is not
clamped to the desktop, so a 2560×1440 frame is genuinely rendered at 2560×1440.

| File | Size | Where it goes |
|---|---|---|
| `icon-512.png` | 512×512 | Play Console app icon |
| `feature-graphic-1024x500.png` | 1024×500 | Feature graphic (its 1024×500 is a fixed spec of its own; the 2:1 screenshot rule does not apply to it) |
| `screenshot-1-zen.png` | 1080×1920 | Phone — Zen scheme, mid-game |
| `screenshot-2-paper.png` | 1080×1920 | Phone — Paper scheme |
| `screenshot-3-classic.png` | 1080×1920 | Phone — Classic scheme |
| `screenshot-4-colours.png` | 1080×1920 | Phone — the colour strip beside the settings |
| `tablet7-1-columns.png` | 1920×1080 | 7-inch tablet — More Shapes, column layout |
| `tablet7-2-paper.png` | 1920×1080 | 7-inch tablet — column layout, Paper scheme |
| `tablet7-3-classic.png` | 1920×1080 | 7-inch tablet — Default mode, row layout, Classic scheme |
| `tablet7-4-colours.png` | 1920×1080 | 7-inch tablet — the colour strip beside the settings |
| `tablet10-*.png` | 2560×1440 | 10-inch tablet — the same four frames at the larger size |

Notes on the sizes and the content:

- **Tablets are landscape 16:9**, which is what Google's tablet guidance asks for and what a tablet
  is actually held as. Play wants at least four screenshots per tablet slot.
- **Both tablet sizes render the same layout.** The stretch settings make the logical viewport
  1621×912 at either resolution, because both are 16:9 — the 10-inch set is the same frames drawn
  at more pixels, not a different arrangement.
- **The first two tablet frames use the More Shapes mode on purpose.** Six shapes on a turned
  screen is what puts the game into the column arrangement — score and a tray down the left, board
  in the middle, gear and the other tray down the right — which is the layout a tablet gets and the
  one that fills a wide screen. The third frame is deliberately the three-shape Default mode in the
  row arrangement, so the set shows both.
- Phone frames are 1080×1920 rather than the 810×1440 they were first captured at: 1080 on the
  short side is Google's recommended minimum, and the tablet frames would otherwise be sharper than
  the phone ones in the same listing.

The in-app launcher icon is generated from the same artwork and lives in the game repo under
`assets/launcher/`, wired into both export presets; `icon.svg` in the game repo draws the same motif
for the editor and the desktop window.
