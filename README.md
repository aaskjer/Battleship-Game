<p align="center">
  <img src="https://github.com/aaskjer/Battleship-Game/blob/main/Assets/logo_full.png?raw=true" alt="Battleship Game" width="260" height="260">
</p>

<h1 align="center">Battleship Game</h1>

<p align="center">
  <b>Your chat plays Battleship against a CPU admiral — live on stream.</b><br>
  A chat and overlay game for <a href="https://streamer.bot">Streamer.bot</a> and OBS, on Twitch, YouTube and Kick.
</p>

<p align="center">
  <a href="https://github.com/aaskjer/Battleship-Game/releases"><img src="https://img.shields.io/github/v/release/aaskjer/Battleship-Game?label=release&color=2563eb" alt="Release"></a>
  <a href="https://github.com/aaskjer/Battleship-Game/releases"><img src="https://img.shields.io/github/downloads/aaskjer/Battleship-Game/total?color=16a34a" alt="Downloads"></a>
  <img src="https://img.shields.io/badge/platforms-Twitch%20%7C%20YouTube%20%7C%20Kick-9146FF" alt="Twitch, YouTube, Kick">
  <img src="https://img.shields.io/badge/Streamer.bot-1.0%2B-0B72FF" alt="Streamer.bot 1.0+">
  <a href="https://github.com/aaskjer/Battleship-Game/blob/main/LICENSE"><img src="https://img.shields.io/github/license/aaskjer/Battleship-Game?color=64748b" alt="License"></a>
</p>

<p align="center">
  <a href="https://github.com/aaskjer/Battleship-Game/releases"><b>⬇️ Download</b></a> ·
  <a href="https://github.com/aaskjer/Battleship-Game/blob/main/Import-String.md"><b>📋 Import string</b></a> ·
  <a href="https://github.com/aaskjer/Battleship-Game/blob/main/Setup-Guide.md"><b>🛠️ Setup Guide</b></a> ·
  <a href="https://github.com/aaskjer/Battleship-Game/blob/main/FAQ.md"><b>❓ FAQ</b></a>
</p>

<p align="center">
  <img src="https://github.com/user-attachments/assets/78df8db9-e9aa-40c1-93f7-f7da84c0e507" alt="The Battleship overlay in OBS" width="720">
</p>

---

## ✨ What's new in 2.0

| | |
| --- | --- |
| 👥 **Chat vs the Admiral** | The whole chat is one fleet. Every round each viewer gets one vote, and the most-voted square is fired at. Win, and the pot is shared. |
| ⚔️ **Team duel** | One chat team against another: challenge, accept, pick a side, then take turns voting on each shot. The winners share every entry. |
| 🖱️ **Click to Fire** | With the free Heat extension, players fire by clicking enemy waters right on the stream (Twitch). |
| 🛑 **Moderator stop** | `!stopbattle` ends whatever is running, pays nothing more out and refunds duel entries. |
| 🎨 **New settings window** | Rebuilt in the look of the other Chat Games, with a chat preview for every message. |
| 🌐 **Every platform plays** | No more platform switches: Twitch, YouTube and Kick all play, and replies go to the chat the command came from. |

---

## 🎮 Three ways to play

| Mode | Started with | How it plays |
| --- | --- | --- |
| 🚢 **Solo** | `!battleship` | One viewer against the Admiral. Each `!fire` is a full exchange: the player shoots, then the Admiral shoots back. |
| 👥 **Chat vs the Admiral** | `!seeschlacht` | Everybody votes, by clicking or with `!fire B7`. Sinkings fill a pot, losses drain it; win, and it is shared between everyone who voted often enough. |
| ⚔️ **Team duel** | `!duel @name` | Team A against Team B. Every member pays the entry fee, the sides take turns, and the side that sinks the other's fleet shares the pot. |

Every mode can be switched off on its own.

---

## ⚓ Commands

| Command | What it does |
| --- | --- |
| `!battleship` | Put a fleet to sea and start a solo battle |
| `!fire B7` | Fire at a square — also `b7`, `B 7`, `b-7`. A square fired at before is refused and costs nothing |
| `!fleet` | Redeploy your ships at random, before your first shot |
| `!board` | Shots, accuracy, ships left and the running total |
| `!seeschlacht` | Start Chat vs the Admiral |
| `!duel @name` | Challenge a viewer to a team duel — or anybody, with no name |
| `!accept` | Take up a challenge and captain Team B |
| `!team A` | Join a side during sign-up: `A`, `B`, a player's name, or nothing for the smaller side |
| `!fire link` | Link your clicks on stream to you, once (Click to Fire) |
| `!stopbattle` | Moderators: end whatever is running, nothing more is paid out |
| `!bstats` | Your record — add a name to look someone else up |
| `!ballstats` | The leaderboard |

The game knows its commands by their IDs, so rename any of them to whatever your chat likes.

---

## 🌊 How a battle plays

Both fleets are placed at random and never shown to the other side. Every sinking settles on the spot.

| | Event | What happens |
| --- | --- | --- |
| 🚢 | **Deploy** | Both fleets are placed. The player's own fleet is drawn on the overlay, and optionally whispered to them. |
| 🌊 | **Miss** | A peg on the board, and the Admiral fires back. |
| 💥 | **Hit** | The ship takes damage. Nothing is paid until it sinks. |
| 🔥 | **Enemy ship sunk** | Its value is paid at once. |
| ☠️ | **Your ship sunk** | Its value is charged at once. |
| 🏆 | **Victory** | The whole enemy fleet is down — plus an optional win bonus. |
| 💀 | **Defeat** | Your fleet is gone; everything lost was already charged as it happened. |
| ⏰ | **Walked away** | The turn clock runs out and settles whatever has sunk. Damaged ships can be charged pro-rata. |

Board presets: **Blitz** (6×6, 3 ships), **Quick** (8×8, 3 ships) and **Classic** (10×10, 5 ships) — or any fleet on a 5×5 to 12×12 grid, with an optional rule that ships may not touch.

---

## 🎯 The Admiral

| Level | Plays like | Shots to clear a Quick board |
| --- | --- | --- |
| **Ensign** | Fires at random, never twice at a square | ~59 |
| **Captain** *(default)* | Hunts down a ship once it lands a hit, and follows a line of hits to both ends | ~36 |
| **Admiral** | Also hunts on a lattice spaced to the shortest ship still afloat, so nothing hides between its shots | ~30 |

A **blunder chance** makes any level fire blind now and then. **The Admiral never sees your board**: it only knows where it fired, what it found, and which ship lengths are still afloat.

<details>
<summary>Measured how?</summary>

10,000 self-games per level and preset. On Classic (10×10, 5 ships) the three levels need roughly 96, 62 and 52 shots.

</details>

---

## 💰 Points

There is **no bet and no entry fee** in a solo battle. Every ship has a value you set:

| | |
| --- | --- |
| Sink an enemy ship | **+** its value, at once |
| Lose one of your own | **−** its value, at once |
| Win the battle | an optional bonus on top |

Because everything settles as it happens, bailing out of a battle that goes badly saves nothing. Guard rails: a minimum balance to play, balances that never go below zero, a daily winnings cap, cooldowns, battles per hour, and an *addicted* list whose players win less. **Free game** turns all of it off for bragging rights only.

Works with [Currency Core by StreamUP](https://streamup.tips/product/currency-core) and [LOYALTY POINTS by tawmae](https://tawmae.xyz/loyalty-points) out of the box, or any points variable of your own.

---

## 📺 The overlay

- Two boards side by side: **your fleet** drawn in full, **enemy waters** fogged until something is hit
- Impact bursts, sunk silhouettes, a marker on the last shot, and a fleet roster with damage pips
- A countdown ring that turns amber, then pulses red, as chat gets its warnings
- The chat's votes and the duel's sides drawn right on the board
- Transparent, scalable, every colour configurable — and set up in OBS with one click

---

## 🏆 Stats and leaderboard

`!bstats` shows a player's record: battles, win rate, accuracy, ships sunk and lost, streaks and net. `!ballstats` ranks by net, points won, ships sunk or accuracy, with 🥇🥈🥉 for the top three. Leaderboards reset on demand, or automatically every week or month for seasons. Every value lives in a `bsg_*` user variable, so your own actions can read it too.

---

## ⚙️ The settings window

Everything is set up in one window, in the look of the other Chat Games:

| Page | |
| --- | --- |
| **Game** | Points, free game, economy, the Admiral, the battle clock |
| **Fleet** | Presets, board size, ships with their lengths and values |
| **Modes** | Solo, Chat vs the Admiral, Team duel |
| **Overlay** | Size, colours, what is shown, one-click OBS setup |
| **Clicks** | Click to Fire with Heat |
| **Access** | Cooldowns, blocked and addicted viewers |
| **Messages** | Every chat message, with placeholder chips and a chat preview |
| **Stats** | Leaderboard and seasons |
| **Log** | Every battle, searchable and filterable |

---

## 🚀 Get started

1. **Import** — drag [`BattleshipGame.sb`](https://github.com/aaskjer/Battleship-Game/releases) into Streamer.bot's Import window, or paste the [import string](https://github.com/aaskjer/Battleship-Game/blob/main/Import-String.md).
2. **Enable the commands** in the `[BG] - Battelship Game` group.
3. **Open the settings** and check your points and fleet.
4. **Add the overlay** to OBS with one click on the Overlay page.

The [Setup Guide](https://github.com/aaskjer/Battleship-Game/blob/main/Setup-Guide.md) walks through every step, and the [FAQ](https://github.com/aaskjer/Battleship-Game/blob/main/FAQ.md) answers the usual questions.

---

## 💜 Credits

Made by **[aaskjer](https://twitch.tv/aaskjer)** · [aaskjer.github.io](https://aaskjer.github.io/)<br>
Built on [Streamer.bot](https://streamer.bot) by [nate1280](https://www.patreon.com/c/nate1280/home)<br>
Questions and ideas: [Battleship Game on the Streamer.bot Discord](https://discord.com/channels/834650675224248362/1447648588694818836/1447648588694818836)

And **YOU** ♡
