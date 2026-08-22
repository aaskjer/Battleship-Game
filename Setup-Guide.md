# Setup Guide

## 1. Import the game

<p align="center"><img alt="grafik" src="https://github.com/user-attachments/assets/2c17f5a0-15f3-4396-a277-a66638017fb3"> </p> 

Click on `Import` in [streamer.bot](https://streamer.bot/) and drag the [BattleshipGame.sb](https://github.com/aaskjer/Battleship-Game/releases) file into the window or copy paste the [import string](https://github.com/aaskjer/Battleship-Game/blob/main/Import-String.md).

<p align="center"><img alt="grafik" src="https://github.com/user-attachments/assets/1c43fd2b-c607-4df4-8023-ce2a246680a9"> </p> 

Click on `Import` and `Yes` on both following popup messages.


<p align="center"><img alt="grafik" src="https://github.com/user-attachments/assets/0374b2fb-1cc5-4166-9a63-8129e8a51db8"> </p> 


Enable all commands in the `Commands` tab of streamer.bot.

| Action | File | What it does |
| --- | --- | --- |
| **Battleship Game** | `BattleshipGame.cs` | The game itself |
| **Battleship Game - Settings** | `BattleshipGame-GUI.cs` | The settings window |
| **Battleship Game - Stats** | `BattleshipStats.cs` | `!bstats` and `!ballstats` |

## 2. Create the commands

Point the first four at **Battleship Game** and the last two at **Battleship Game - Stats**. The command IDs matter — the action routes on them, not on the command text, so you can rename any of these to whatever your chat prefers.

## 3. Open the settings window

Run the **Battleship Game - Settings** action. Everything is configured there — platforms, currency, the fleet, the Admiral's skill, ship values, timers, the overlay and the leaderboard.

<p align="center"><img alt="grafik" src="https://github.com/user-attachments/assets/ba957c57-f1bd-408f-8bc4-b5655cf39700"> </p> 

At minimum, check:

- **General Settings** → which platforms are enabled, and your points variable if you do not use Currency Core or LOYALTY POINTS.
- **Ship Values & Economy** → what each ship is worth. This is the whole economy; there is no bet.
- **Board & Fleet** → pick a preset. Quick (8×8) is the default and lands a battle in roughly 36 shots.

## 4. Add the overlay to OBS

The **OBS Overlay** settings page has a **Set the overlay up in OBS** button that creates the scene and the browser source for you, over the OBS connection Streamer.bot already has. It writes the overlay file first if it is not there yet, so it works before anyone has played a round.

<p align="center"><img alt="grafik" src="https://github.com/user-attachments/assets/78df8db9-e9aa-40c1-93f7-f7da84c0e507"> </p> 

To do it by hand instead: the overlay installs itself the first time the game action runs, then add a **Browser Source** in OBS pointing at:

<p align="center"><img alt="grafik" src="https://github.com/user-attachments/assets/6d02e59c-ebbd-4b39-816a-c2be6ad5fd5d"> </p>

The exact path is shown on the **OBS Overlay** settings page, which also has a button to open the folder. The background is transparent, so it layers over any scene. 1920×1080 is a good source size.

## 5. Play

```
!battleship
!fire B7
```

---

[Frequently Asked Questions](https://github.com/aaskjer/Battleship-Game/blob/main/FAQ.md)
