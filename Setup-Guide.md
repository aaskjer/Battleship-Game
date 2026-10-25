# Setup Guide

## 1. Import the game

<p align="center"><img alt="The Import button in Streamer.bot" src="https://github.com/user-attachments/assets/2c17f5a0-15f3-4396-a277-a66638017fb3"></p>

Click **Import** in [Streamer.bot](https://streamer.bot/) and drag [`BattleshipGame.sb`](https://github.com/aaskjer/Battleship-Game/releases) into the window, or paste the [import string](https://github.com/aaskjer/Battleship-Game/blob/main/Import-String.md).

<p align="center"><img alt="The Import window" src="https://github.com/user-attachments/assets/1c43fd2b-c607-4df4-8023-ce2a246680a9"></p>

Click **Import**, and **Yes** on both messages that follow. You get three actions in the group **[BG] Battleship Game**:

| Action | What it does |
| --- | --- |
| **Battleship Game** | The game itself: every battle, the chat's battle and the duel |
| **[BG] - .Settings** | The settings window |
| **Battleship Scoreboard** | `!bstats` and `!ballstats` |

The .NET references are written so they work wherever Streamer.bot is installed — no **Find Refs** needed.

**Updating from 1.0.0?** Import over it. Your settings live in `BSG-Settings.json` and are kept; new settings start at their defaults. The import brings the commands 1.0.0 lacked, so enable the group again afterwards (step 2).

## 2. Enable the commands

The commands arrive switched off. In the **Commands** tab, right-click the group **[BG] - Battelship Game** and choose **Enable All**.

<p align="center"><img alt="Enable All in the command group" src="https://github.com/user-attachments/assets/0374b2fb-1cc5-4166-9a63-8129e8a51db8"></p>

| Command | Action | Command ID |
| --- | --- | --- |
| `!battleship` | Battleship Game | `33042222-a6c0-439d-8a50-e0033bfe692e` |
| `!fire` | Battleship Game | `76c89914-ac0f-4c8a-9e95-e60584472371` |
| `!fleet` | Battleship Game | `e3518842-d1cc-40e7-8991-c31beaed019a` |
| `!board` | Battleship Game | `77fdf075-4457-469b-b5ca-644421fc75bc` |
| `!seeschlacht` | Battleship Game | `6047090e-b187-44b9-a881-524f7d0481d5` |
| `!duel` | Battleship Game | `b5acc015-c1ee-458f-b94a-e97872dbded4` |
| `!accept` | Battleship Game | `0797e4bc-ec92-4940-af31-84f0b7ba5513` |
| `!team` | Battleship Game | `a6aab1e0-0e47-4d4e-9238-87067d369748` |
| `!stopbattle` | Battleship Game | `7e59baf2-0cf5-4442-963e-2df9776951be` |
| `!bstats` | Battleship Scoreboard | `d25e44f1-2da3-435c-b1c4-fc07f02c74a4` |
| `!ballstats` | Battleship Scoreboard | `a562b900-a7d7-42ca-a818-a2acb5b85ba7` |

The game knows its commands by their IDs, not by their text, so you can rename any of them. If you rename the duel commands, change them in the duel messages too (settings window, Messages page, under Team duel and Duel errors), which tell viewers what to type.

- `!seeschlacht` starts **Chat vs the Admiral**, where the whole chat votes.
- `!duel @name` challenges a viewer to a **team duel** (no name: anybody may accept), `!accept` takes it up, and `!team` joins a side during sign-up — `!team A`, `!team B`, `!team kim` to join kim's side, or plain `!team` for the smaller one.
- `!stopbattle` is for moderators and comes set to the **Moderators** group. It ends whatever is running and pays nothing more out; a duel's entries are refunded.

Twitch, YouTube and Kick all play, and every reply goes to the chat the command came from. To leave a platform out, untick it in the commands' sources (for example *Kick Message*).

## 3. Open the settings window

Open the **[BG] - .Settings** action, right-click **Test** under **Triggers** and choose **Test Trigger**. Everything is configured there; the pages sit in the rail on the left — Game, Fleet, Modes, Overlay, Clicks, Access, Messages, Stats, Log and About (Ctrl+1 to Ctrl+0).

At minimum, check:

- **Game** → your points variable, if you use neither Currency Core nor LOYALTY POINTS (both are found on their own).
- **Fleet** → pick a preset and set what each ship is worth. Quick (8×8) is the default and lands a battle in roughly 36 shots. The ship values are the whole economy; there is no bet.

## 4. Add the overlay to OBS

The **Overlay** page has a **Set the overlay up in OBS** button. It creates the scene and a browser source the size of your canvas, over the OBS connection Streamer.bot already has (**Stream Apps ▸ OBS Studio**), and writes the overlay file first if it is not there yet.

<p align="center"><img alt="The overlay in OBS" src="https://github.com/user-attachments/assets/78df8db9-e9aa-40c1-93f7-f7da84c0e507"></p>

To do it by hand: the overlay installs itself the first time the game action runs. Add a **Browser Source** in OBS for the local file

```
<Streamer.bot folder>\Chat Games\Battleship Game\Overlay\battleship_overlay.html
```

<p align="center"><img alt="The Battleship Game folder" src="https://github.com/user-attachments/assets/6d02e59c-ebbd-4b39-816a-c2be6ad5fd5d"></p>

The Overlay page has a button that opens this folder. The background is transparent, so it layers over any scene. Make the source the size of your canvas — 1920×1080 for most — and let it cover the whole canvas; the button does exactly that, and Click to Fire relies on it.

## 5. Play

```
!battleship
!fire B7
```

## 6. Optional: Click to Fire (Twitch)

Lets the player whose battle is on screen fire by clicking a square in enemy waters on the stream, through the free [Heat](https://dashboard.twitch.tv/extensions/cr20njfkgll4okyrhag7xxph270sqk) extension.

1. Install Heat on your channel and activate it as an **Overlay** extension.
2. In Streamer.bot open **Servers/Clients ▸ WebSocket Server**, tick **Auto Start** and press **Start Server**. The overlay hands each click to the **Battleship Game** action through it. The default address, `127.0.0.1:8080`, needs no change.
3. In the settings window open **Clicks**, switch on **Fire by clicking the stream** and save. Your channel ID is taken from the Twitch broadcaster account connected to Streamer.bot.
4. If you added the browser source by hand or resized it, make it cover the whole canvas: set its width and height to your canvas size, then right-click it ▸ **Transform ▸ Fit to screen**. Or enter where it sits on the Clicks page.
5. Check it: switch on **Show every click on the overlay**, save, then while live open your channel in a browser and click around the video. Each marker should land under the pointer, and the box in the corner says whether Heat and Streamer.bot are connected. Switch it off afterwards — viewers see it too.

Players just click: the first click on enemy waters in a player's battle is taken as theirs for that battle. Heat sends nearly every click anonymously, so this is how the game tells whose clicks are whose. If someone else's click got in first, the player clicks once and types `!fire link` in chat — that link is remembered for later battles. *The first click in a player's battle takes control* on the Clicks page switches the automatic part off.

Optional: **Add to OBS** under *Heat's click markers* on the same page puts Heat's own Markers overlay — a marker where each viewer last clicked — into your Battleship scene, so everyone watching sees the clicks land. Click to Fire works without it.

---

## Where things live

```
<Streamer.bot folder>\Chat Games\Battleship Game\
    BSG-Settings.json              all settings
    Overlay\
        battleship_overlay.html    the browser source
        battleship_state.json      written as the battle plays
        battleship_heat.json       Click to Fire settings for the overlay, once it is switched on
<Streamer.bot folder>\Chat Games\Logs\
    BSG-log-YYYY-MM-DD.txt         one file per day
```


---

[Frequently Asked Questions](https://github.com/aaskjer/Battleship-Game/blob/main/FAQ.md)
