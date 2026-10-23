
<p align="center"><img src="https://github.com/aaskjer/Battleship-Game/blob/main/Assets/logo_full.png?raw=true" alt="Battleship banner" width="400" height="400"></p>

Battleship Game is a chat game built for streamer.bot, supporting twitch, kick, and youtube.

![Static Badge](https://img.shields.io/badge/https%3A%2F%2Fimg.shields.io%2Fbadge%2Fany_text-Twitch%2C_Kick%2C_YouTube-white?label=supported%20plattform)
![Static Badge](https://img.shields.io/badge/https%3A%2F%2Fimg.shields.io%2Fbadge%2Fany_text-streamer.bot-white?label=tool&color=%230b73ff)
![GitHub License](https://img.shields.io/github/license/aaskjer/Battleship-Game)
![GitHub Release](https://img.shields.io/github/v/release/aaskjer/Battleship-Game)
![GitHub Downloads (all assets, all releases)](https://img.shields.io/github/downloads/aaskjer/Battleship-Game/total)

---

# Overview

- Works on Twitch, YouTube, and Kick, each individually toggleable.
- Chat takes command of a fleet and duels a CPU opponent — the **Admiral** — on a grid rendered live on your OBS overlay.
- Both fleets are placed at random and never revealed. Each `!fire` is a full exchange: the player shoots, then the Admiral shoots back.
- **No entry fee.** Every ship carries a configurable point value: sink one of the Admiral's and you are paid on the spot, lose one of yours and you are charged on the spot.
- Because it settles live, walking away from a battle that is going badly saves nothing — the losses have already landed.
- Three difficulty levels for the Admiral, plus a blunder slider that handicaps any of them without letting the AI cheat.
- **The Admiral never sees your board.** It knows only where it has fired, what it found, and which ship lengths are still afloat — exactly what a fair opponent knows.
- Board presets — Blitz (6×6, 3 ships), Quick (8×8, 3 ships), Classic (10×10, 5 ships) — or build a custom fleet on a 5×5 to 12×12 grid.
- Two self-running timers: 60 seconds to open fire, then a turn clock with chat warnings and a colour-changing countdown ring at 30 and 15 seconds left.
- Optional Twitch whisper telling each player where their own ships are.
- Free Game mode for bragging-rights-only battles, with its own dedicated chat messages so nothing refers to points that were never at stake.
- Auto-detects [Currency Core by StreamUP](https://streamup.tips/product/currency-core) and [LOYALTY POINTS by tawmae](https://tawmae.xyz/loyalty-points).
- Every chat message is editable in the settings GUI, grouped by phase with the placeholder list on every field.
- Statistics are written to persisted Streamer.bot user variables (`bsg_*`), so any other action can read a player's record — `bsg_shipsSunk`, `bsg_netProfit` — without involving this game at all.
- Persistent daily log file with every battle played, filterable in the built-in log viewer.
- Built-in WPF settings window with a light/dark switch, searchable page navigation, live rule summaries, a colour wheel picker for the overlay, one-click OBS setup, and a filterable action log.

---

## 🎮 How It Works

A viewer runs `!battleship`. Two fleets are placed at random, the overlay lights up, and every `!fire` trades a salvo with the Admiral.

| Event | Trigger | Result |
|---------|---------|---------|
| 🚢 **Deploy** | A viewer runs the battleship command | Both fleets are placed at random. The player's own layout is whispered to them and drawn on the overlay. |
| 🌊 **Miss** | A shot lands in empty water | A peg on the board, and the Admiral returns fire. |
| 💥 **Hit** | A shot lands on a hull | The ship takes damage. Nothing is paid until it actually sinks. |
| 🔥 **Enemy ship sunk** | The last square of an enemy ship is hit | Its value is credited immediately. |
| ☠️ **Your ship sunk** | The Admiral finishes off one of yours | Its value is debited immediately. |
| 🏆 **Victory** | The Admiral's whole fleet is down | An optional win bonus on top of everything already earned. |
| 💀 **Defeat** | Your whole fleet is down | The battle ends; everything lost was already charged as it happened. |
| 🏳️ **Walked away** | The turn clock runs out | Settles whatever has already sunk. Damaged-but-floating ships are charged pro-rata. |
| ⚓ **Never opened fire** | A player starts a battle and walks away | The fleet returns to port. Nothing charged, nothing recorded. |
| 🚫 **Blocked / Addicted** | Player is in a blocked group or on the Addicted list | Blocked players can't play; Addicted players play normally but earn less. |
| ⏱️ **Cooldown** | Player starts again too soon, or hits Max Battles/Hour | A cooldown message instead of a battle. |

---

## ⚓ Commands

| Command | Info |
|---------|---------|
| `!battleship` | Put a fleet to sea and start a battle |
| `!fire B7` | Fire at a square. Accepts `b7`, `B 7`, `b-7` — and firing at the same square twice is rejected without costing your turn |
| `!fleet` | Redeploy your ships at random. Only before your first shot |
| `!board` | Repost shots, accuracy, ships remaining, and your running total |
| `!bstats` | Your record. Add a name to look someone else up |
| `!ballstats` | The leaderboard |

---

## 🎯 The Admiral

Three skill levels, measured over 10,000 games per difficulty on the Quick preset (8×8, 3 ships, 10 hull squares):

| Level | Behaviour | Avg. shots to clear the board |
|---|---|---|
| **Ensign** | Fires uniformly at random, never repeating a square | ~59 |
| **Captain** *(default)* | Hunts: once it lands a hit it works the neighbours, and once two hits line up it follows that axis to both ends | ~36 |
| **Admiral** | Adds lattice hunting — it only checks squares spaced to the shortest ship still afloat, so nothing can hide between the checks — and skips gaps too small for anything left | ~30 |

On Classic (10×10, 5 ships) the same three come out at roughly 96 / 62 / 52 shots.

A **blunder chance** slider makes any level throw away its plan and fire blind some percentage of the time. At 100% even the Admiral plays like an Ensign. It handicaps without ever letting the AI peek at your board.

---

## 💰 Points

Nothing is staked up front. Each ship class has a value you set:

| | |
|---|---|
| Sink an enemy ship | **+** its value, immediately |
| Lose one of your own | **−** its value, immediately |
| Win the battle | optional flat bonus on top |

Settling live is what closes the rage-quit hole. There is no moment where a player can see the battle turning and bail out cheaper than staying.

Ship-granularity leaves one small gap — walking away one hit before a Carrier sinks — and **Charge for damaged ships on abandon** (on by default) closes it by charging pro-rata for hit-but-floating ships when a player times out. It never applies during normal play.

Guard rails: a minimum balance to play (defaulting to the value of the player's own fleet, so nobody plays a game they cannot lose), balances that clamp at zero, an optional daily winnings cap, per-user and global cooldowns, and Max Battles/Hour.

---

## ⏱️ Timers

Both run on their own, so a player who walks away closes their own battle instead of leaving it open forever.

| Timer | Default | What happens |
|---|---|---|
| **Deploy** | 60s | Started a battle but never fired. Nothing has sunk, so nothing is charged and it is not recorded as a battle. |
| **Turn** | 90s | Chat warnings at **30s** and **15s** remaining, with the overlay ring turning amber then pulsing red. Running out settles whatever has already sunk. |

Both durations and both warning thresholds are configurable, and a threshold that would not fit inside the turn clock is skipped rather than fired instantly. The settings page spells out the resulting sequence as a sentence.

---

## 📺 OBS Overlay

A self-contained browser source that renders the battle live, installed automatically into the overlay folder — no manual file copying. Add a Browser Source pointing at `battleship_overlay.html`; the background is transparent so it layers over any scene.

- Two grids side by side: **your fleet** drawn in full, and **enemy waters** fogged until something is hit.
- Showing your own ships gives nothing away — the Admiral reads neither chat nor the overlay.
- Miss pegs, impact bursts, revealed silhouettes for sunk ships, and a marker on the square that was just hit.
- Fleet roster under each board with per-ship damage pips and struck-through sinkings, each side toggled on its own. Hiding one keeps the two boards level rather than shunting one up.
- Countdown ring that turns amber and then pulsing red at the same moments chat gets its warnings.
- Running points total and live accuracy, then a result badge that holds and fades.
- Scale, font scale, every colour, and per-element toggles are configurable from the OBS Overlay tab.
- The overlay folder can be a subfolder of the game directory or any full path on any drive.
- Version-stamped and self-upgrading, preserving your previous file as a `.bak`.

---

## 🏆 Leaderboard

`!ballstats` ranks players on **net** by default — everything won minus everything lost — which is the honest measure now that losing ships costs points. Also rankable by points won, ships sunk, or accuracy (which requires at least 25 shots to qualify, so one lucky shot is not a record).
🥇🥈🥉 medals for the top three, configurable to 1–10 places, with optional hiding of the broadcaster and bot account matched by user id so a name change can't put them back on the board.
`!bstats` reports a player's own record — battles, win rate, accuracy, ships sunk and lost, flawless victories, fewest-shots win, streaks and net. Pass a name to look up someone else.
Leaderboards can be reset on demand from the settings window, or automatically **weekly or monthly** for running seasons — either the running totals only, or every statistic.


---

## Get Started

[Setup Guide](https://github.com/aaskjer/Battleship-Game/blob/main/Setup-Guide.md)

[Frequently Asked Questions](https://github.com/aaskjer/Battleship-Game/blob/main/FAQ.md)

[Import String for streamer.bot](https://github.com/aaskjer/Battleship-Game/blob/main/Utilities/Import-String.md)

[Download](https://github.com/aaskjer/Battleship-Game/releases)

---

## Credits & Links

[aaskjer on Twitch](https://twitch.tv/aaskjer)

[aaskjer.github.io](https://aaskjer.github.io/) 

[Battleship-Game on SB Discord]()

[Streamer.bot](https://streamer.bot) / [nate1280](https://www.patreon.com/c/nate1280/home)

YOU ♡
