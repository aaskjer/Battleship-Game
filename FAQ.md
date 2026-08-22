# Frequently Asked Questions

## Playing

### How long does a battle take?
On the default Quick preset (8×8, 3 ships) a battle runs about 30–36 shots. Since each `!fire` is a
full exchange — you shoot, the Admiral shoots back — that is 30–36 chat commands, or roughly five
minutes of stream. Classic (10×10, 5 ships) runs closer to 60. If that is too long for your chat,
switch to **Blitz** (6×6) on the Board & Fleet page.

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
That is the **Charge for damaged ships on abandon** setting, on by default. Without it, walking away
one hit before a Carrier sank would dodge its entire value. A Carrier at 4 of 5 hits costs 4/5. It
only applies when you time out, never during normal play. You can turn it off on the Ship Values &
Economy page.

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
Change the skill level on **The Admiral** page. Ensign fires at random, Captain hunts, Admiral hunts
efficiently. For finer control, the **blunder chance** slider makes any level fire blind a percentage
of the time — at 100% even the Admiral plays like an Ensign.

### Why does the Admiral pause before returning fire?
So the overlay reads as an exchange rather than one simultaneous flash. Configurable from 0 to 5000ms
on The Admiral page.

---

## Timers

### What are the warnings?
Mid-battle you get a turn clock, with a chat warning and an amber countdown ring at 30 seconds left,
then a second warning and a pulsing red ring at 15. Both thresholds and the clock itself are
configurable.

### I set a warning longer than my turn timer and it never fires.
Correct — a 30-second warning inside a 20-second turn would fire the instant the turn began, so it is
skipped instead. The Timers page shows the resulting sequence as a sentence and flags any threshold
it will ignore.

### What happens when the clock runs out?
The battle ends and settles whatever has already sunk, including the damaged-ship charge. This is the
only way out of a battle once it has started — there is no command to concede, because conceding and
walking away would be worth precisely the same anyway.

---

## Overlay

### The overlay is blank.
Check that the Browser Source points at `battleship_overlay.html` in the folder shown on the OBS
Overlay settings page — there is a button there to open it. The overlay only draws while a battle is
live; between battles it is intentionally invisible.

### Numbers on the overlay don't match chat.
They should — both use the separator from **General Settings**. If they disagree you are looking at a
stale overlay file; the game rewrites it automatically when the version changes, keeping your old one
as a `.bak`.

### Can I hide the fleet roster?
Either one, separately, on the OBS Overlay page. Hiding the Admiral's roster is the one that changes
play: its damage pips are what tell you which enemy ship your hits have landed on. Whichever is
hidden, the two boards stay level with each other.

### Can I move the overlay folder?
Yes. The OBS Overlay page accepts either a subfolder name or a full path on any drive.

### I edited the overlay HTML and my changes were overwritten.
The game reinstalls the overlay whenever its embedded version is newer than the file on disk, keeping
your copy as a `.bak`. To make changes stick, edit `battleship_overlay.html` in the project and run
`tools/sync-overlay.ps1` — never edit the C# string, which is generated.

---

## Statistics

### Where are stats stored?
In persisted Streamer.bot user variables prefixed `bsg_`. Any other action can read them directly —
`bsg_shipsSunk`, `bsg_netProfit`, `bsg_bestStreak` — without involving this game.

### How is the leaderboard ranked?
By net — everything won minus everything lost — which is the honest measure now that losing ships
costs points. You can switch it to points won, ships sunk, or accuracy on the Leaderboard page.
Accuracy needs at least 25 shots to qualify.

### Can I run seasons?
Yes. Automatic weekly or monthly resets, clearing either the running totals only or every statistic.
Turning it on never wipes anything immediately — the current period is recorded first and the reset
happens at the next boundary. There is also a Reset Now button with a confirmation spelling out
exactly what it will clear.

---

## Setup

### Which platforms are supported?
Twitch, YouTube and Kick, each individually toggleable.

### Does it work with my points system?
It auto-detects Currency Core by StreamUP and LOYALTY POINTS by tawmae. Otherwise set the points
variable by hand on the General Settings page.

### Whispers aren't arriving.
Whispers are Twitch-only — Streamer.bot has no whisper API for YouTube or Kick. Twitch also silently
drops whispers from accounts without a verified phone number. Failures are logged rather than
surfaced, and play continues, because the overlay already shows the player their own fleet. You can
turn the whisper off entirely under **Fleet whisper** on the General Settings page; its wording stays
on the System Messages page.

### Can I rename the commands?
Yes. The action routes on command IDs, not on the command text, so `!battleship` can become anything
you like as long as the ID matches the table in the Setup Guide.

---

# Is Battleship Game an AI Slop?

Partially, yes. This script was built with AI assistance and input from the Streamer.bot community. I'm not a developer in the classical sense, I used this project to actually learn, and I put a lot of time and care into it.
I understand that a lot of people (especially more tech savvy folks) will look at the game or my other projects and dismiss the project because of the AI involvement, and honestly, I get it. That's a fair position to hold.
AI-assisted code can and does introduce bugs. I've spent real time testing, breaking, and fixing this thing. If you run into something wrong, or just have feedback, please [open an issue](https://github.com/aaskjer/Battleship-Game/issues) — I'm always happy to get input :)
