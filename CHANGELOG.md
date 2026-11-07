# Changelog

All notable changes to this project are documented here.

## [2.2.0] — Unreleased

### 📺 Overlay

- **Try the demo.** A new button on the Overlay page opens the overlay in your browser, playing a short made-up
  battle in the saved look (a miss, a sunk destroyer, the Admiral finding your cruiser, the enemy fleet sunk),
  without Streamer.bot. With `?demo=1` on the overlay's address, OBS plays it the same way, to place and size the
  source.
- **Show a sample round.** The button beside it plays that battle once on every overlay that is open, the one in OBS
  included, so you can check where it sits and how it looks; then the overlay goes back to no battle. Not while a
  real battle runs. The overlay is version 17 and replaces an older copy on the next command, keeping that one as
  a `.bak`; an overlay already open in OBS reloads itself within ten seconds.

### 🆓 Free game from chat

- **Two moderator commands.** `!SetFreeBattleship` turns Free game on and `!EndFreeBattleship` turns it off (they
  come with the import, disabled and set to the Moderators group). The switch is written into the settings file, so
  the window's pill shows it, and chat gets the new Started and Ended messages (Messages page, Free game). An action
  of your own can do the same with an Execute C# Method on `StartFreeGame` or `EndFreeGame` of the game action.
- **Two more custom triggers.** `BSG: Free Game Start` and `BSG: Free Game End` fire whenever Free game is switched
  that way.

### 🪟 Settings window

- **The settings action is UIFramework's stub 0.2.0.** When the game action hangs, the settings window waits 30
  seconds for its answer, then carries on and can be closed; before, it waited for good. A game that has not
  described itself for a minute no longer locks the settings until Streamer.bot restarts: the next run opens the
  window.
- The host block is UIFramework's current one (0.10.0). It names every type in full, so it no longer depends on the
  action's `using` lines, and it can declare a tour of the window. Nothing changes in the window.

## [2.1.0] — Unreleased

### 🔔 For your own actions

- **Custom triggers.** Every finished battle fires a trigger (Custom > Chat Game > Battleship Game): `BSG: Battle
  Won`, `BSG: Battle Lost`, `BSG: Battle Timed Out` or `BSG: Battle Stopped` for a solo battle, `BSG: Chat Battle
  Settled` for the chat's battle, `BSG: Duel Settled` for a team duel, and `BSG: Battle Settled` for every one of
  them. The trigger carries the battle as arguments: `battleOutcome` (the log's result: WIN, LOSS, TIMEOUT, NO-SHOW,
  STOPPED), `battleMode` (solo, chat or duel), `battleUser`, `battleUserId`, `battlePlatform`, `battleTrigger`
  (fire, timeout or stop), `battleBoard`, `battleShots`, `battleHits`, `battleAccuracy`, `battleShipsSunk`,
  `battleShipsLost`, `battleNet` (the battle's total, or a duel's pot), `battleWinner` (a duel's winning side) and
  `battleCurrency`. `GetLastBattleResult`, run with an Execute C# Method sub-action on the game action, hands the
  same arguments to any other action.

### 🪟 Settings window

- **German.** With the window's language set to `de` (the pencil, then *Sprache*), every setting, section, page
  and message of the window reads in German. The help and about pages, the summaries and the presets' status
  line stay English.
- **The settings action is UIFramework's generic stub.** `[BG] - .Settings` carries no layout and no code of its
  own any more: the game action declares its window (pages, the log, help and about, the cooldowns of
  `!battleship`) beside its settings table and answers the window itself. *Reset the leaderboard now*, *Set the
  overlay up in OBS* and *Add to OBS* now answer with what happened instead of "queued", and a save puts the
  Click to Fire settings on a live overlay as before. Needs UIFramework 0.9.2 or newer; the stub fetches it.

## [2.0.0] — 2026-11-03

The settings window moved into [UIFramework for streamer.bot](https://github.com/aaskjer/UIFramework-Streamer.bot),
one window shared by all of these chat games. Battles, the chat's battle and the duel play exactly as before;
what changed is how the settings are shown and stored.

### 🪟 One window, downloaded on first open

- **`[BG] - .Settings` is a small stub now.** The first time you open it, it asks whether to download
  `UIFramework.dll` from GitHub (about 200 KB, checked against its published SHA-256) or to show the folder it
  belongs in, `UIFramework\lib\<version>\` next to Streamer.bot. Nothing is fetched without that click. When a
  newer UIFramework is published, opening the window offers it once: *Yes* downloads, *No* asks again next time
  and shows a pill in the header, *Cancel* skips that version.
- **The window is built from the game's own settings table.** The game declares every setting once, the
  scoreboard carries the same table, and the window asks the game for it, so the window and the actions can never
  disagree about a key, a default or a range again.
- **The Fleet page keeps its presets and its summary.** Blitz, Quick and Classic fill in the board and the ships
  as before, and the line under the ships says how many squares the fleet takes and about how many shots a battle
  needs, as the values stand. The battle clock has the same kind of summary on the Game page.
- **The Clicks page shows the broadcaster account** Streamer.bot is connected to, and still adds Heat's click
  markers to OBS with one button.
- **A save that would not do what it looks like asks first**: Free game off without a points variable, no ship
  sailing, a ship longer than the board, a warning that is not shorter than a turn, Click to Fire without a
  WebSocket address.
- **Customise mode.** The pencil at the bottom left lets you rename, reorder, hide and move pages and fields by
  drag and drop or from the context menu, and pick the accent colour. It lives in `uiframework.layout.json` next
  to `BSG-Settings.json` and survives every import of the game.
- The footer counts the unsaved changes; a third button resets only the page in front, after asking, and nothing
  is written until you save; Cancel and the title bar's X ask "Discard N unsaved changes?" before dropping
  anything; a save that fails says so in a message box; a recompile or shutdown of Streamer.bot closes an open
  window on its way out.

### ⚙️ Settings

- `BSG-Settings.json` keys are camelCase now (`freeGame`, `gridSize`, `overlayHitColour`, `heatEnabled`), the
  percentages (the Addicted cut, the overlay scales and opacity, the blunder chance, the Heat area) are stored as
  percent instead of fractions, and "Battles per hour" stores 0 for no limit. An existing file keeps working: the
  old names are read through aliases and scaled on the way, and the first save writes the new ones.
- The cooldown messages use `{seconds}` and `{user}` instead of `{0}` and `{1}`; a text that still has the
  numbers keeps working.
- Light/dark lives in `uiframework.layout.json` and is shared with every other window built on UIFramework. An
  existing `DarkMode` is taken over on the first open.
- Values typed out of range are pulled back into range when the game reads them, not only when the window saves.
- The fleet whisper switch sits on the Access page under Platforms, next to the other Twitch-only notes.

### 🗑️ Removed

- The window's own WPF code and update check (about 4,500 lines) left the import; the DLL carries them now. The
  rainbow mode and the Egg are the framework's now.

## [2.0.0-pre-release.2] — 2026-10-27

### 🎨 A new colour picker

- A click on the swatch next to an overlay colour opens the picker right under the field, instead of a
  separate window with a colour wheel: a field for saturation and brightness and a hue bar, both to click
  or drag, sliders for red, green and blue (the mouse wheel nudges them by one), and a row of ready
  colours. Every change goes straight into the hex box, and typing a hex code moves the picker along.
  Done or a click elsewhere closes it.

## [2.0.0-pre-release.1] — 2026-10-26

### 📦 Import

- The import now brings all eleven commands, each wired to its action: `!battleship`, `!fire`, `!fleet`,
  `!board`, `!seeschlacht`, `!duel`, `!accept`, `!team`, `!stopbattle` (moderators only), `!bstats` and
  `!ballstats`. 1.0.0 shipped without `!board`, although the game answers it.
- The settings window sets the cooldowns on the `!battleship` command that comes with the import. In
  1.0.0 it set them on a command id that does not exist, so they never took effect.
- The .NET references are written so they hold wherever Streamer.bot is installed (repaired with
  SBRefFix).

### 🎨 Settings window in the Chat Games look

Rebuilt in the design of Classic Chat Gamble and the Slot Machine Game.

- A rail of ten pages — Game, Fleet, Modes, Overlay, Clicks, Access, Messages, Stats, Log, About
  (Ctrl+1 to Ctrl+0) — with pills in the header for Free game and the game modes that are on, and a
  footer with the theme and reset buttons, Cancel, Save and Save & Exit. Switches, segment bars and
  name chips instead of tick boxes and comma lists. Shift on the theme button: rainbow, for one opening.
- One Messages page instead of four: pick any of the 73 messages, insert placeholders from chips and
  see it in a chat preview with that message's own senders (bot account, /me, the fleet whisper).
- Ship lengths and values side by side on the Fleet page; the economy, the Admiral and the battle clock
  on the Game page; the cooldowns and the blocked and addicted lists on the Access page.
- The update check runs while the window is open and shows a red pill only for a newer release. It
  used to hold the window up for up to 3 seconds, counted any other tag as an update, and its answer
  never reached the window.
- A damaged `BSG-Settings.json` is reported and kept as `.broken`, instead of being replaced with the
  defaults on the next save without a word. Saving is atomic, so the game never reads half a file,
  and every save leaves `BSG-Settings.json.bak` behind — the file as it was before that save. A
  damaged file is read back from there, and only falls to the defaults when there is no readable
  backup either; a file that does not parse itself never becomes the backup, so a broken one cannot
  overwrite the last good copy.
- A `BSG-Settings.json` that cannot be opened at all — locked by something else, or not readable for
  want of rights — says so and does not open the window. It used to count as damaged and show the
  defaults, where a save would have written them over a perfectly good file.
- Reset puts every setting back to its default (the theme stays). Closing with unsaved changes no
  longer asks; the footer says "Unsaved changes" instead, and the theme button still asks.
- The Log page reads the log files on its first visit, and filters by Stopped too.
- "Reset the leaderboard now" clears what the scope picked in the window says, not the one saved when
  it opened.
- A ship switched off may be longer than the board, and a ship switched on with length 0 no longer
  counts as sailing — the game drops it, so such a fleet could be saved and never start.
- Texts pointed to "Platforms ▸ OBS"; Streamer.bot has OBS under Stream Apps ▸ OBS Studio. The About
  page no longer offers Twitch channel points as a points system: the game does not use them.

### ⏳ Cooldown messages

- A cooldown message with any brace other than `{0}` or `{1}` — `{user}`, say — was never sent: the
  game formatted it with `string.Format`, which threw, and only the log said so. The placeholders are
  now filled in by hand, as in the other Chat Games: `{user}` works in both cooldown messages, `{{` and
  `}}` still stand for a brace, anything else goes out as written, and an empty message sends nothing.

### 🌐 Platforms

- The platform selection is gone from the settings window, the game and the stats action. Twitch,
  YouTube and Kick always play; a platform is left out by unticking it on the commands in
  Streamer.bot. Settings files that still carry the switches are read without them, and the "not
  enabled on this platform" message is gone.

### 🔧 Developer tooling

- `tools/gui-shots.ps1` renders every page of the real window offscreen, or with `-Test` drives it
  through 80 behaviour checks; `check-all.ps1` runs it.
- `check-compile.ps1` makes the whole rewrite Streamer.bot makes before compiling, including the
  `override` it puts on Execute, Init and Dispose by name — a hand-written one fails there with CS1004,
  so the actions declare them plain `public`, as the docs show.
- `RoslynHost` compiles with overflow checks, as Streamer.bot does, and like Streamer.bot only with the
  references an action lists (`-exclude`).

### ⚔️ Team duel

The third game mode: one chat team against another.

- **Challenge, accept, sign up.** `!duel @name` challenges a viewer (no name: anybody) and captains
  Team A; `!accept` captains Team B. Then everyone picks a side with one command, `!team`, read
  dynamically: `A`/`B`, a player's name to join their side, or nothing to fill the smaller side.
  An optional limit keeps the sides within a set number of players of each other.
- **Entry fee and pot.** Every member pays the entry fee; the winning side shares the whole pot
  equally. Each winner gets their own entry back whole, and only the winnings on top take the addicted
  cut and the daily cap. A challenge nobody accepts, and a duel nobody fires in, refund everyone.
- **Alternating vote rounds.** The sides take turns; each turn is one voting round for that side's
  members only (`!fire B7` or a linked click), and the most-voted square is fired at. Both fleets
  stay hidden until the end. A side that lets its turn pass too often in a row gives up.
- Anonymous Heat clicks never count in a duel — one could be the other side spending this side's turn.
  Members' own clicks are passed on ahead of the crowd's, so a busy stream cannot crowd them out.
- Overlay v14: Team A left, Team B right, the side whose turn it is highlighted, votes on the waters
  under fire, and a winner badge. Each member's record counts the duel as won or lost.
- Nothing else can start from the challenge to the last shot. An `!accept` or `!team` that arrives
  after its clock has run out is refused, and with no ships enabled `!duel` is refused before anybody
  pays.

### 🛑 Moderator stop

- `!stopbattle` ends whatever is running — solo battles, the chat's battle, a duel or a duel still
  signing up — and pays nothing more out: no win bonus, no pot, no charge for damaged ships. A duel's
  entries are refunded. Who may use it is set on the command's permissions in Streamer.bot.
- A shot can no longer slip in while the stop is being announced, and a battle stopped as it starts
  never announces itself.

### 🐛 Fixed

- A payout, refund or charge never writes over a balance that could not be read. Before, a failed read
  counted as 0, so paying a sinking or a share could replace a viewer's whole balance with just that
  amount. Now the balance is left alone and the amount owed is logged, to be paid by hand.
- Replies to a vote in the chat's battle or a duel, and to `!board`, go to the asker's own chat, not to
  the platform the battle was started from.
- Closing Streamer.bot — or saving an edited version of the action — in the middle of a game no longer
  strands anything: every duel entry is refunded, each battle ends as stopped (without chat messages,
  and without counting as an abandoned game on anybody's record) and the overlay fades out instead of
  freezing on the last shot. A board left frozen by a crash is cleared the next time the action's code
  loads. A battle that fails to start is ended the same way, entries refunded.
- A charge that could not be written is no longer counted in a battle's net, and a payout that failed no
  longer uses up the player's daily winnings cap.
- The settings window could open over and over during a chat battle: if anything pointed the overlay's
  clicks or a game command at the settings action, every vote opened another window. The settings
  window now refuses game traffic (and says so in the log, once a minute), only one window opens at a
  time, and the game only learns which action the overlay should call from a real chat command.

### 👥 Chat vs the Admiral

A second game mode: the whole chat plays one fleet against the Admiral.

- **Voting rounds.** Each viewer has one vote per round — a click on enemy waters or `!fire B7` — and
  the square with the most votes is fired at when the round ends (15 s by default). The overlay shows
  the round's votes on the board, with a countdown ring and the number of voters.
- **Played for fun.** No points to win or lose: nobody pays and nobody is paid, so every viewer's
  click counts the same — logged in to Twitch or not — with nothing to link first. The win names how
  many viewers voted. (A logged-out viewer can vote again by reloading; it does no harm here. In a duel
  their clicks never count.)
- Started with its own command, only when no other battle is running; broken off when nobody votes
  for as long as the turn clock allows. Solo and chat battles can each be switched off.
- Switched on and set up on the **Modes** settings page; its messages are on the Messages page.

### 🖱️ Click to fire with Heat

Optional, Twitch only. With the free [Heat](https://dashboard.twitch.tv/extensions/cr20njfkgll4okyrhag7xxph270sqk) extension on the channel, the
player whose battle is on screen can fire by clicking a square in enemy waters right on the stream,
instead of typing `!fire`.

- The overlay turns the click into a square — it is the only thing that knows where each one is drawn —
  and hands it to the game through the Streamer.bot WebSocket server, where it takes exactly the path
  `!fire` takes: the same chat messages, the same rules, the same clock.
- Heat sends nearly every click under an anonymous id — of 163 clicks sampled across 30 live channels,
  2 carried a Twitch ID — so the game works out whose clicks are whose itself:
  - **Automatically** (on by default): the first click on enemy waters in the battle of a player
    without a link is taken as theirs, for that battle. It holds back when another viewer clicked
    there in the last 20 seconds, and never hands a battle to someone linked as a different player.
  - **`!fire link`**: click any square in enemy waters, then type it. Refused when more than one
    viewer clicked in the last 20 seconds; fires the click that made the link; kept in a user
    variable for later battles; overrides an automatic link a faster viewer took.
  - A player who did share their Twitch ID with Heat needs neither.
- Only the player's clicks fire. Other viewers' clicks on enemy waters reach the game only so it can
  make links — at most once every 0.7 s per viewer and ten a second in all — and the game decides
  everything rather than trusting the overlay.
- The same click heard by two copies of the overlay fires once.
- A **Clicks** settings page: the switch, a setup checklist, the connection (channel id detected from
  Streamer.bot, WebSocket address and optional password) and where the browser source sits on the
  canvas.
- An alignment test draws every click on the overlay, with a status box for Heat and Streamer.bot.
- An **Add Heat's click markers to OBS** button puts Heat's own Markers overlay — a marker where each
  viewer last clicked — into the Battleship scene at the size of the canvas. Optional: the Battleship
  overlay listens to Heat by itself.
- A chat hint tells a player who has not linked yet how to, when their battle comes on screen —
  editable on the Messages page, and off when emptied.
- Overlay v12. It reads its Click to Fire settings from `battleship_heat.json`, which is only created
  once the feature is switched on.
- Signing in to a password-protected Streamer.bot WebSocket server hashes in plain JavaScript: OBS
  serves a local browser source from `http://absolute/`, which is not a secure context, so the
  browser's own `crypto.subtle` is not there to use.
- The overlay notices when the game has installed a newer version of it and reloads itself, so an
  update no longer waits for someone to refresh the browser source in OBS. The alignment test's status
  box names the version that is running.

### 📺 OBS Auto-Setup

- The browser source is created at the size of your OBS canvas rather than 1280×720, so it covers the
  whole picture — which Click to Fire relies on — and renders at full resolution.
- The source name is checked against every source in OBS rather than only the target scene. OBS names
  are global, so a name already used in another scene could never have been created there.

### 🐛 Fixes

- A battle starting while another was being played could take the overlay off it: the "oldest live
  battle" was really whichever came first in a dictionary, and a finished battle's freed slot put the
  newest one first. The battle on screen now keeps the overlay until it ends, then hands it to the one
  that has waited longest — and never to a battle still being set up.

---

## [1.0.0] — First release

Battleship for chat, built on the framework behind the Blackjack Game and played on an OBS overlay
rather than in chat text. Chat carries terse commands and one-line results; the overlay carries the
grids, the pegs and the fleet roster.

---

### 🚢 The game

A viewer takes command of a fleet and duels a CPU opponent, the **Admiral**. Both fleets are placed
at random and never revealed to the other side. Every `!fire` is a full exchange — the player shoots,
then the Admiral shoots back — which halves the number of chat commands a battle costs.

**Commands**

| Command | Description |
| --- | --- |
| `!battleship` | Put a fleet to sea and start a battle. |
| `!fire B7` | Fire at a square. Accepts `b7`, `B 7`, `b-7`. |
| `!fleet` | Redeploy your ships at random. Only before your first shot. |
| `!board` | Repost shots, accuracy, ships remaining and your running total. |

**Board presets** — Blitz (6×6, 3 ships), Quick (8×8, 3 ships, the default), Classic (10×10, 5 ships),
or a custom fleet on any grid from 5×5 to 12×12. An optional no-touching rule keeps a one-square gap
around every ship.

Firing at a square twice is rejected without consuming the turn or the clock — a misclick should not
cost a shot.

There is no command to leave a battle. Walking away is what the turn clock is for: it runs out,
whatever has already sunk is settled, and damaged-but-floating ships are charged pro-rata if that
setting is on.

---

### 🎯 The Admiral

Three skill levels, plus a blunder slider that handicaps any of them by making the AI throw away its
plan and fire blind some percentage of the time.

| Level | Behaviour |
| --- | --- |
| **Ensign** | Fires uniformly at random, never repeating a square. |
| **Captain** *(default)* | Hunts: after a hit it works the neighbouring squares, and once two hits line up it follows that axis to both ends. |
| **Admiral** | Adds lattice hunting spaced to the shortest ship still afloat, and skips gaps too small for anything still floating. |

Measured over 10,000 games per difficulty on the Quick preset: roughly 59, 36 and 30 shots to clear
the board. On Classic: roughly 96, 62 and 52.

**The AI never sees the board it is firing at.** It is given only the squares it has fired at, what
each turned out to be, and the lengths of the ships still afloat — which sinking announcements make
public anyway. This is enforced by the type it is handed, not by convention.

---

### 💰 Economy — live, symmetric, per ship

There is **no entry fee and no stake**. Each ship class carries a configurable value, and every
sinking is a points event applied immediately:

- Sink one of the Admiral's ships → **credited** on the spot.
- Lose one of your own → **debited** on the spot.
- Win the battle → an optional flat bonus on top.

Settling live is what closes the rage-quit hole: by the time a battle is going badly the player has
already paid for everything they lost, so walking away saves nothing.

**Charge for damaged ships on abandon** (default on) closes the remaining ship-granularity gap, where
a player could walk away one hit before a Carrier sank. Timing out charges pro-rata for
hit-but-floating ships — a Carrier at 4 of 5 hits costs 4/5 of its value. It never applies during
normal play.

**Guard rails:** a minimum balance to play, defaulting to the value of the player's own fleet so
nobody plays a game they cannot lose; balances that clamp at zero; an optional daily winnings cap
that rolls over on its own; per-user and global cooldowns; and Max Battles/Hour.

---

### ⏱️ Automatic timers

Two clocks, both running on their own so an abandoned battle closes itself.

- **Deploy — 60s.** Started a battle but never fired. Nothing has sunk, so nothing is charged and it
  is not recorded as a battle.
- **Turn — 90s.** Chat warnings at **30s** and **15s** remaining, with the overlay countdown ring
  turning amber and then pulsing red at the same moments. Running out ends the battle and settles
  whatever has already sunk.

Both durations and both thresholds are configurable. A warning threshold that would not fit inside
the turn clock is skipped rather than fired instantly, and the settings page renders the resulting
sequence as a sentence so a misconfigured clock is visible at a glance.

---

### 📺 OBS Overlay

A self-contained browser source, installed automatically into the overlay folder — no manual file
copying. The background is transparent.

- Two grids side by side: **your fleet** drawn in full, **enemy waters** fogged until something is
  hit. Revealing your own ships gives nothing away, since the AI reads neither chat nor the overlay.
- Miss pegs, impact bursts, revealed silhouettes for sunk ships, and a marker on the square that was
  just hit.
- Fleet roster under each board with per-ship damage pips and struck-through sinkings. Your roster
  and the Admiral's are toggled separately — the enemy one is what tells you which of their ships
  your hits have landed on. Hiding one keeps the two boards level rather than shunting one up.
- Countdown ring matching the chat warnings, running smoothly between updates rather than ticking
  only when a shot lands.
- Running points total and live accuracy, then a result badge that holds and fades —
  **ENEMY FLEET SUNK**, **FLEET LOST**, **ABANDONED** or **STOOD DOWN**, each one saying whose fleet
  it was.
- Scale, font scale, every colour, and per-element toggles, all configurable.
- Numbers use the separator configured in the settings window rather than the browser's locale, so
  the overlay and chat always agree.
- Version-stamped and self-upgrading, preserving your previous file as a `.bak`.

**It holds together at every board size.** The middle column keeps a fixed share of the width and
refuses to be squeezed by the grids; the cell size gives way instead. The badge measures itself and
steps its size down until it fits, so no wording can overflow whatever the board size and scale —
whole words, always inside the box. Checked across every grid size from 5 to 12, at 100%, 125% and
160% scale, against all four badges: 48 combinations, none clipped, none overlapping, no page
overflow.

**OBS Auto-Setup** — one button creates the scene and the browser source for you, pointed at the
overlay file, over Streamer.bot's own OBS connection. Configurable connection number, scene name and
source name. If the overlay file is not on disk yet, the setup runs the game action once to install
it, so a first-time setup works without anyone having played a round. It refuses to touch a source
that already exists rather than duplicating it.

---

### 🔎 Whispered fleets

Optionally whispers each player the positions of their own ships when a battle starts, and again
after a `!fleet` reroll.

Twitch only — Streamer.bot has no whisper API for YouTube or Kick, and Twitch itself drops whispers
from accounts without a verified phone number. Best-effort by design: a failure is logged and play
continues, because the overlay already shows the player their own fleet.

---

### 🏆 Leaderboard & Statistics

Every tracked value is written to a persisted Streamer.bot **user variable** named `bsg_*`, so any
other action can read a player's record — `bsg_shipsSunk`, `bsg_netProfit` — without involving this
game at all.

Tracked: battles played, won, lost and abandoned, shots fired, hits, misses, ships sunk and lost,
flawless victories, fewest-shots win, points won and lost, net, and current and best streak.

**`!ballstats`** ranks on **net** by default — the honest measure now that losing ships costs points —
and can also rank by points won, ships sunk, or accuracy. Accuracy requires at least 25 shots to
qualify, so one lucky shot is not a record.

🥇🥈🥉 medals for the top three, 1–10 places, and optional hiding of the broadcaster and bot account
matched by user id so a name change cannot put them back on the board.

**`!bstats`** reports a player's own record, and accepts a name to look someone else up.

**Resets** — on demand from the settings window with a confirmation spelling out exactly what will be
cleared, or automatically weekly or monthly for running seasons. Two scopes: running totals only, or
every statistic. Enabling automatic resets never wipes anything immediately; the current period is
recorded first and the reset happens at the next boundary.

---

### 📜 Action log

Every battle is written to a daily file as a structured line, and the settings window reads those
files back as one searchable table.

- Every daily file is read as one list — no picking a day from a dropdown.
- Sortable columns: time, result, user, platform, trigger, board, shots, hits, accuracy, change and
  currency. Click a heading to sort by it.
- Filter by result, by platform, by age (today, last 7 days, last 30 days) and by free-text search
  across every column, with a count of what is showing.
- Selecting a row shows the raw line underneath.
- A **Write log file** tick box on the General page turns the whole thing off.

---

### 🎨 Settings window

Searchable page navigation and an unsaved-changes counter with a prompt on close.

```
SETUP        ⚙️ General · 📺 OBS Overlay · ⏱️ Timers & Cooldowns
BATTLESHIP   🚢 Board & Fleet · 🎯 The Admiral · 💰 Ship Values & Economy
MESSAGES     🎬 Play · 🏆 Result · ⚠️ Errors · 🔔 System
TRACKING     🏅 Leaderboard · 📜 Action Log
HELP         📖 How To Play · ℹ️ About & Links
```

- **Board & Fleet** leads with the three preset buttons and shows a live summary —
  "8×8 · 3 ships · 10 of 64 squares occupied · ~36 shots per battle" — plus warnings if a ship no
  longer fits the board or every ship is disabled.
- **Timers & Cooldowns** renders the warning sequence as a sentence and flags thresholds it will
  ignore.
- **Ship Values & Economy** keeps the earn and lose sides together, since they are the same number.
- Every message field lists the placeholders it understands, both in the tooltip and below the box.
- **Clickable placeholder chips** — the `{user}` and `{coord}` chips under each message box insert
  themselves at the cursor instead of only documenting themselves.
- **Platform selector** — a bordered 2×2 grid with the Twitch, YouTube and Kick logos, and the whole
  cell toggles, not just the tick box.
- Saving refuses rather than silently mangling a fleet that cannot fit the board.

**Theme, colours and contrast**

- **Theme switch** — a ☀️ / 🌙 button in the bottom bar flips between light and dark, taking effect
  at once. Whatever is typed is saved first, then the window is rebuilt against the other palette,
  so nothing is lost.
- **Colour wheel picker** — every colour field opens a proper picker: hue around the wheel,
  saturation out from the middle, brightness on a slider, a live preview and the hex code.
- **Readable dropdowns** — the stock WPF combo box keeps its own light chrome no matter what
  background is set on it, which would leave white text on a white button in dark mode. Every
  dropdown is drawn from the theme instead: the collapsed box, the popup, and the hover and
  selection states.
- **Accent contrast** — the pastel page accents are meant for a dark panel, so on the light theme
  section headings take them down a couple of stops, and the folder buttons carry the accent as a
  fill behind near-black text.

**About tab** — built around the project logo and a credits block, with the prerequisite and social
links as a two-column grid of buttons rather than a list of raw URLs. Each button carries the site's
own favicon and a readable name, and wears that site's colour — read out of the favicon itself,
which lands exactly on the published brand hex for Twitch, GitHub and Discord. The label flips
between near-black and white depending on what the fill can carry, so GitHub's black and Discord's
blurple stay as readable as the lighter ones. The full URL is on the tooltip. Buttons size to their
own content.

Icon downloads negotiate TLS 1.2 explicitly. .NET Framework still defaults to SSL3/TLS 1.0, which
GitHub refuses outright, so without it the logos silently never arrive.

---

### 🆓 Free Game mode

Battles for bragging rights, with a dedicated set of chat messages so nothing refers to points that
were never at stake. Ships still sink and the overlay still runs; nothing is credited or debited.

---

### 🔧 Developer tooling

`tools/` holds the checks used to build this, all driven by the Roslyn assemblies that ship inside
Streamer.bot — no .NET SDK required.

- `run-engine-tests.ps1` lifts the pure-logic region straight out of `BattleshipGame.cs` and runs
  ~100 assertions against it, including 10,000 self-games per difficulty per preset. What is tested
  is the exact source that ships, not a copy that can drift.
- `check-compile.ps1` compiles all three actions the way Streamer.bot does, including the class
  rewrite and injected usings.
- `check-settings.ps1` catches drift between the three `Settings` classes and control tags naming a
  property that does not exist — both of which fail silently at runtime.
- `sync-overlay.ps1` makes `battleship_overlay.html` the single source of truth and the embedded C#
  copy a build product, bumping the version stamp when the content actually changes.

---

**Platforms:** Twitch, YouTube, and Kick, each individually toggleable.
