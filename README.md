<p align="center">
  <img src="https://github.com/aaskjer/Battleship-Game/blob/main/Assets/logo_full.png?raw=true" alt="Battleship Game" width="400" height="400">
</p>

<h1 align="center">Battleship Game</h1>

<p align="center">
  <b>Chat commands a fleet against a CPU admiral — on Twitch, YouTube and Kick.</b><br>
  Every salvo lands live on your OBS overlay.
</p>

<p align="center">
  <img src="https://img.shields.io/badge/platforms-Twitch%20%7C%20YouTube%20%7C%20Kick-9146FF" alt="Platforms">
  <img src="https://img.shields.io/badge/tool-streamer.bot-0b73ff" alt="Streamer.bot">
  <a href="https://opensource.org/licenses/MIT"><img src="https://img.shields.io/badge/License-MIT-yellow" alt="License: MIT"></a>
  <img src="https://img.shields.io/github/v/release/aaskjer/Battleship-Game?label=stable" alt="Release">
  <img src="https://img.shields.io/github/downloads/aaskjer/Battleship-Game/total" alt="Downloads">
  <img src="https://img.shields.io/github/stars/aaskjer/Battleship-Game" alt="Stars">
</p>

<p align="center">
  <a href="https://github.com/aaskjer/Battleship-Game/releases"><b>Download</b></a> &nbsp;·&nbsp;
  <a href="Setup-Guide.md"><b>Setup Guide</b></a> &nbsp;·&nbsp;
  <a href="FAQ.md"><b>FAQ</b></a> &nbsp;·&nbsp;
  <a href="Import-String.md"><b>Import String</b></a>
</p>

---

## 📖 Overview

Battleship Game is a chat game for Streamer.bot. A viewer puts a fleet to sea with `!battleship`, and both
fleets are placed at random, never revealed. Every `!fire` trades a salvo: the player shoots, then the
Admiral shoots back. Twitch, YouTube and Kick can each be switched on or off.

## ✨ Highlights

- 💰 **No entry fee, no rage-quit discount.** Every ship class has a point value. Sink one of the Admiral's
  ships and you are paid on the spot; lose one of yours and you are charged on the spot. Leaving a lost
  battle early saves nothing.
- 🎯 **An opponent that plays fair.** The Admiral never sees your board. It knows where it has fired, what it
  found and which ship lengths are still afloat. Its three levels need about 59, 36 and 30 shots to clear
  the Quick board, measured over 10,000 games each, and a blunder slider weakens any of them.
- 🗺️ **Boards of any size.** Blitz (6×6), Quick (8×8) and Classic (10×10) presets, or a custom fleet on
  anything from 5×5 to 12×12.
- ⏱️ **Battles that close themselves.** A deploy timer and a turn clock end abandoned games. Chat gets
  warnings at 30 and 15 seconds while the overlay ring turns amber, then red. Ships that are hit but still
  afloat are charged pro rata when a player times out.
- 📺 **A live OBS overlay.** Your fleet in full, enemy waters fogged until something is hit, damage pips per
  ship, running points and accuracy. It installs itself and keeps the previous version as a `.bak` when it
  updates.
- 🏆 **Records other actions can read.** Stats live in persisted `bsg_*` user variables. The leaderboard ranks
  net result, points won, ships sunk or accuracy, and resets weekly or monthly if you want seasons.
- ⚙️ **One settings window.** Light and dark themes, live rule summaries, one-click OBS setup and a filterable
  log. Every chat message is editable, and Free Game mode has its own set, so no line mentions points that
  were never at stake.

---

## Credits

- **Author:** [aaskjer on Twitch](https://twitch.tv/aaskjer) · [aaskjer.github.io](https://aaskjer.github.io/)
- **Built for:** [Streamer.bot](https://streamer.bot) by [nate1280](https://www.patreon.com/c/nate1280/home)
- **Works with:** [Currency Core by StreamUP](https://streamup.tips/product/currency-core) · [LOYALTY POINTS by tawmae](https://tawmae.xyz/loyalty-points)
- **Thanks:** YOU ♡
