# Frequently Asked Questions

## Playing

### How long does a battle take?
On the default Quick preset (8×8, 3 ships) a battle runs about 30–36 shots. Since each `!fire` is a
full exchange — you shoot, the Admiral shoots back — that is 30–36 chat commands, or roughly five
minutes of stream. Classic (10×10, 5 ships) runs closer to 60. If that is too long for your chat,
switch to **Blitz** (6×6) on the Fleet page.

### Can I place my own ships?
No — fleets are always placed at random. Chat is public and so is the overlay, so any command that
positioned ships would broadcast their positions. `!fleet` rerolls your layout before your first shot
if you dislike it, which leaks nothing because positions are never displayed.

### How do I know where my own ships are?
The overlay draws your fleet in full on the left-hand board. Optionally the game also whispers you the
coordinates when the battle starts. Both are safe: the Admiral reads neither chat nor the overlay.

### Why does the overlay show my ships but not the enemy's?
Because there is no one to hide them from. The AI is not reading the screen. Hiding your own fleet
would only hide it from you and your viewers.

### I typed a square I had already fired at and nothing happened.
That is deliberate. A repeat shot is a misclick, not a move, so it is rejected without costing your
turn or resetting your clock. Pick another square.

### Can more than one person play at once?
Yes. Each player has their own battle. Only one can be on the overlay at a time — the first one to
start holds it and hands it over when it ends — but everyone gets full play in chat regardless.

---

## Points

### How much does it cost to play?
Nothing. There is no bet and no entry fee. You earn by sinking the Admiral's ships and you pay when
you lose your own, at the value configured for each ship class.

### Why do I lose points during the battle rather than at the end?
So that leaving early cannot save you anything. If losses only landed at the end, the correct play
would be to walk away the moment a battle turned — which is not much of a game. Settling as it happens
means by the time it is going badly, you have already paid.

### I timed out with a badly damaged ship and was still charged for it.
That is the **Charge for damaged ships when a player runs out of time** setting, on by default.
Without it, walking away one hit before a Carrier sank would dodge its entire value. A Carrier at 4 of
5 hits costs 4/5. It only applies when you time out, never during normal play. You can turn it off on
the Game page.

### Can I go into negative points?
Not by default — losses are capped at what you actually have. Turn off **Never let a balance go below
zero** if you want debt to be possible.

### Why can't I start a battle?
Most likely you are below the minimum balance. By default that is the total value of your own fleet,
so that nobody plays a game they cannot lose. It is configurable, including to zero.

### Is this a points faucet?
Much less than a no-stake game normally would be. Because sinking and being sunk are symmetric and
the Admiral is a real opponent, an evenly matched setup is roughly break-even. If you want a hard
ceiling anyway, there is a per-player daily winnings cap.

### What happens if I never fire at all?
The battle closes after 60 seconds and nothing is charged, nothing is credited, and it is not
recorded as a battle played.

---

## The Admiral

### Does the AI cheat?
No. It is handed only the squares it has fired at, what each turned out to be, and the lengths of the
ships still afloat — which is public information, since sinkings are announced. It is never given the
board it is firing at. The test suite asserts this by checking it can never win in fewer shots than
there are hull squares.

### The Admiral is too hard / too easy.
Change the skill level under **The Admiral** on the Game page. Ensign fires at random, Captain hunts,
Admiral hunts efficiently. For finer control, the **blunder chance** makes any level fire blind a
percentage of the time — at 100% even the Admiral plays like an Ensign.

### Why does the Admiral pause before returning fire?
So the overlay reads as an exchange rather than one simultaneous flash. Configurable from 0 to 5000ms
under The Admiral on the Game page.

---

## Timers

### What are the warnings?
Mid-battle you get a turn clock, with a chat warning and an amber countdown ring at 30 seconds left,
then a second warning and a pulsing red ring at 15. Both thresholds and the clock itself are
configurable.

### I set a warning longer than my turn timer and it never fires.
Correct — a 30-second warning inside a 20-second turn would fire the instant the turn began, so it is
skipped instead. The battle clock on the Game page shows the resulting sequence as a sentence and
flags any threshold it will ignore.

### What happens when the clock runs out?
The battle ends and settles whatever has already sunk, including the damaged-ship charge. This is the
only way out of a battle once it has started — there is no command to concede, because conceding and
walking away would be worth precisely the same anyway.

---

## Overlay

### The overlay is blank.
Check that the Browser Source points at `battleship_overlay.html` in the folder shown on the Overlay
settings page — there is a button there to open it. The overlay only draws while a battle is
live; between battles it is intentionally invisible.

### Numbers on the overlay don't match chat.
They should — both use the **Numbers** separator on the Messages page. If they disagree you are looking at a
stale overlay file; the game rewrites it automatically when the version changes, keeping your old one
as a `.bak`.

### Can I hide the fleet roster?
Either one, separately, on the Overlay page. Hiding the Admiral's roster is the one that changes
play: its damage pips are what tell you which enemy ship your hits have landed on. Whichever is
hidden, the two boards stay level with each other.

### Can I move the overlay folder?
Yes. The Overlay page accepts either a subfolder name or a full path on any drive.

### I edited the overlay HTML and my changes were overwritten.
The game reinstalls the overlay whenever its embedded version is newer than the file on disk, keeping
your copy as a `.bak`. To make changes stick, edit `battleship_overlay.html` in the project and run
`tools/sync-overlay.ps1` — never edit the C# string, which is generated.

---

## Click to Fire

### What do I need for Click to Fire?
The free Heat extension installed on your channel and activated as an Overlay, Streamer.bot's WebSocket
server running, and the switch on the **Clicks** settings page turned on. The Setup Guide walks
through it.

### A player clicks and nothing happens.
In order of likelihood:
- Someone else's click came first. Heat sends almost every click anonymously, so the first click on
  enemy waters in a player's battle is taken as theirs — if another viewer was quicker, that viewer
  took control of this battle. The player clicks once and types `!fire link` to take it back; that link
  is remembered for later battles. With automatic linking switched off, `!fire link` is always needed
  once. The alignment test's status box says whose clicks currently count.
- Their battle is not the one on screen. Only the battle on the overlay can be clicked; a second player
  waits for the overlay to come free and gets a chat hint when it does.
- They are on a phone. Twitch does not show overlay extensions in its mobile app.
- Streamer.bot's WebSocket server is not running, or asks for a password the settings do not have. The
  alignment test's status box says which.

### Can other viewers fire for the player?
Only by clicking before the player does, while automatic linking is on: the first click on enemy
waters in a battle without a link takes control of it. It holds back when another viewer clicked there
in the last 20 seconds and never hands a battle to someone linked as a different player, and the
player takes it back with one click and `!fire link`. A player with a saved link cannot be taken over
at all: a click only fires when it comes from the anonymous Heat ID they linked, or from their Twitch
ID if they shared it with Heat. `!fire link` itself only works when exactly one viewer clicked in the
last 20 seconds, so somebody clicking along can hold a link up but never take it over. Switch
automatic linking off on the Clicks page if that first-click window is too open for your chat.

### !fire link says more than one person clicked.
Other viewers clicked enemy waters in the same 20 seconds, so it would be guessing. Wait a moment, click
once, then type it again.

### Clicks land on the wrong square.
The browser source does not cover the whole canvas. Either make it (set its size to your canvas, then
Transform ▸ Fit to screen) or enter where it sits on the Clicks page. The alignment test draws
every click where the overlay thinks it landed, so you can see at once whether the two agree.

### Why does the shot show up a few seconds after the click?
The shot fires the moment the click arrives. What the player sees is your stream delay — the same wait
as after typing `!fire`. The squares never move, so clicking on a picture a few seconds old is fine.

### The status box shows an old overlay version.
From v9 on, the overlay reloads itself a few seconds after the game installs a newer version. Older
ones cannot: refresh the browser source once (its properties ▸ *Refresh cache of current page*). If
the box says a newer version is installed and asks for a refresh, the reload did not take — do the
same.

### The overlay is in two scenes. Does every click fire twice?
No. Both copies hear the click and pass it on, and the game counts it once.

---

## Chat vs the Admiral

### How do I vote?
Click a square in enemy waters on stream, or type `!fire B7`. One vote per round counts; further clicks
in the same round are ignored, which is the cooldown. When the round ends, the square with the most
votes is fired at — on a tie, the one voted for first.

### Does anybody win or lose points?
No. The chat's battle is played for fun: nobody pays to vote and nobody is paid when the chat wins.
That is what lets every click count — the game never has to know whose click it was, so there is
nothing to link first.

### Who can click?
Everybody watching in a desktop browser, logged in to Twitch or not. One click per round counts; a
logged-out viewer can vote again by reloading the page, which does no harm in a battle played for fun.
Phone viewers can't click at all — Twitch's app shows no overlay extensions — so they vote with
`!fire B7`.

### The chat battle command says a battle is already running.
A chat battle only starts when no other battle is on the overlay. Wait for it to end.

---

## Team duel

### How does a duel work?
`!duel @name` challenges a viewer (or anybody, with no name) and makes you captain of Team A; whoever
`!accept`s captains Team B. Then everyone else picks a side with `!team`. When sign-up closes, both
fleets are placed at random and hidden from everyone, and the sides take turns: each turn is one voting
round for that side's members only.

### Who pays what?
Every member pays the entry fee when they join. The winning side shares the whole pot equally; each
winner gets their own entry back whole, and only the winnings on top take the addicted cut and the
daily cap. A challenge nobody accepts, a duel nobody fires in, and a duel a moderator stops refund
every entry.

### My vote didn't count.
Only the side whose turn it is votes, with `!fire B7` or a linked click. Anonymous Heat clicks never
count in a duel — one could be anybody's, even the other side spending your side's turn — so link your
clicks once with `!fire link`.

### A side stopped voting.
A side that lets its turn pass too often in a row (3 by default, on the Modes page) gives up, and the
other side wins.

---

## Statistics

### Where are stats stored?
In persisted Streamer.bot user variables prefixed `bsg_`. Any other action can read them directly —
`bsg_shipsSunk`, `bsg_netProfit`, `bsg_bestStreak` — without involving this game.

### How is the leaderboard ranked?
By net — everything won minus everything lost — which is the honest measure now that losing ships
costs points. You can switch it to points won, ships sunk, or accuracy on the Stats page.
Accuracy needs at least 25 shots to qualify.

### Can I run seasons?
Yes. Automatic weekly or monthly resets, clearing either the running totals only or every statistic.
Turning it on never wipes anything immediately — the current period is recorded first and the reset
happens at the next boundary. There is also a Reset Now button with a confirmation spelling out
exactly what it will clear.

---

## Setup

### Which platforms are supported?
Twitch, YouTube and Kick. All three always play; to leave one out, untick it on the commands'
sources in Streamer.bot (for example *Kick Message*). Click to Fire and the fleet whisper are Twitch
only.

### Does it work with my points system?
It auto-detects Currency Core by StreamUP and LOYALTY POINTS by tawmae. Otherwise set the points
variable by hand on the Game page.

### Whispers aren't arriving.
Whispers are Twitch-only — Streamer.bot has no whisper API for YouTube or Kick. Twitch also silently
drops whispers from accounts without a verified phone number. Failures are logged rather than
surfaced, and play continues, because the overlay already shows the player their own fleet. You can
turn the whisper off entirely on the Messages page: pick **Your fleet** under Whisper, and the switch
sits under its preview.

### Can I rename the commands?
Yes. The action routes on command IDs, not on the command text, so `!battleship` can become anything
you like as long as the ID matches the table in the Setup Guide.

### Nothing happens in chat after the import.
The commands arrive switched off. Right-click the command group **[BG] - Battelship Game** and choose
**Enable All**. After updating from 1.0.0 do this again: the import brings `!board` and the commands
for the chat's battle and the duel, which 1.0.0 did not have.

### An action doesn't compile after the import.
Since 2.0.0 the import's .NET references hold wherever Streamer.bot is installed, so **Find Refs** is
no longer needed. If Streamer.bot still reports a missing reference, open the action's **Execute
Code** sub-action, press **Find Refs**, then **OK**. The game needs Streamer.bot 1.0 or newer.
