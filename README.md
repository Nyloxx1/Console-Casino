# Gambling Arena Console

![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?logo=python&logoColor=white)
![Platform](https://img.shields.io/badge/Platform-Windows-0078D6?logo=windows&logoColor=white)
![License](https://img.shields.io/badge/License-Not%20specified-lightgrey)

A play-money casino simulator for Windows Command Prompt with local and LAN multiplayer features.

This is an entertainment project only. It does not use real money, process payments, or provide gambling services.

## Setup And Run

### Install
- Install Casino.7z
- extract Casino.7z
- run launch_casino.cmd
- have fun
- If you get any errors refer to below

### Requirements

- Windows with Command Prompt or PowerShell
- Python 3 installed and available as `python`
- No third-party packages are required

Check Python from a terminal:

```text
python --version
```

### Start The Casino

From the project folder, run:

```text
python casino_console.py
```

You can also launch it with `launch_casino.cmd`.

On first launch, choose a profile and create its password and personal secret code. The secret code is used in the main menu to access chip and power-up changes. The casino creates the required save files automatically.

### Fresh Install

This public copy intentionally contains no player profiles, passwords, saves, logs, or personal data. Running it creates a new `saves/` directory locally.


### Set Up Local Friends

1. Open **Settings > View Profiles**.
2. Create a profile for each friend.
3. Return to the main menu and choose **Profile Match**.
4. Select a friend and choose Texas Hold'em or Five-card Poker.
5. The friend authenticates their profile, then both players pass the keyboard between turns.

The losing profile loses the stake and the winning profile gains it. Both saves are updated after the hand.

### Set Up The Network Lobby

The Network Lobby plays one remote Texas Hold'em hand over a local network.

1. Put both computers on the same local network.
2. On one computer, open **Network Lobby > Host Texas Hold'em** and choose a port, or accept the default `5050`.
3. Find the host computer's local IP address with `ipconfig`.
4. On the second computer, open **Network Lobby > Join Texas Hold'em**.
5. Enter the host's local IP address and the same port.
6. Allow Python through Windows Firewall on a private network if Windows asks.

The lobby now plays one remote Texas Hold'em hand. The host deals the cards, both players see their own private cards and the shared community cards, and the result updates each computer's local profile save. Start another hand by returning to **Network Lobby**.

## Code layout

The main menu and shared casino systems remain in `casino_console.py`. Each table is available through a separate module in `games/`, with common round bookkeeping in `games/common.py`:

- `games/roulette.py`
- `games/texas_holdem.py`
- `games/five_card_poker.py`
- `games/blackjack.py`
- `games/baccarat.py`
- `games/craps.py`
- `games/slots.py`
- `games/plinko.py`
- `games/keno.py`

## Games

- Roulette
- Texas Hold'em
- Five-card Poker
- Blackjack
- Baccarat
- Craps
- Slot Machine
- Plinko
- Keno

Texas Hold'em and Five-card Poker support local opponents. Use **Profile Match** from the main menu to choose a friend profile and a game, then pass the keyboard between players. Both profiles authenticate before play; the losing profile loses the stake and the winning profile gains it, with both saves updated. You can also choose AI or another profile directly when entering either supported game. Games without direct player-versus-player rules continue to use their table AI or dealer.

The **Network Lobby** hosts or joins a standard-library TCP Texas Hold'em game on a local network. It is not an Internet service; both computers must be reachable on the same network and the host may need a private-network Windows Firewall rule.

After a game, the table offers options to play again, return to the casino menu, or save and quit.

## Player systems

- Multiple player profiles
- Separate Normal and Cheat saves
- Casual, Standard, Hardcore, and Nightmare difficulties
- Five lives in Standard difficulty, with difficulty-specific starting lives
- Bankroll reset when a life is lost
- Full run reset when all lives are gone
- Match history, win streaks, reputation, and best-run tracking
- Credit rewards with an increasing win-streak multiplier
- Lounge Shop upgrades, VIP Passes, and Life Tokens
- Optional Always Win setting
- Blackjack split and double-down options
- Difficulty-sensitive Texas Hold'em AI
- Match summaries and rematches for profile games

## Tutorials and Settings

From the main menu:

- **Game Tutorials** explains how each table works without automatically interrupting a game.
- **Settings** contains difficulty, profile management, Normal/Cheat mode, Always Win, and save management.

The game also contains a hidden bonus code. It is intentionally not documented here.

## Tests

Run the test suite from the project folder:

```text
python -m unittest -v
```

The project uses only Python's standard library, so no package installation is required.

## Account security

Profiles and the Admin account use masked passwords stored as salted PBKDF2-SHA256 hashes; plaintext passwords are never written to save files. Login attempts are limited to three tries and failed/successful login events are recorded in `security.log` without recording passwords. Existing profiles without a credential record require a password to be created on their first login. Passwords cannot be recovered by the game, so profile exports and backups should be kept secure.

Local friend matches require the selected friend profile to authenticate before its bankroll is loaded.

## Save system

Saves are stored in the `saves` folder using separate files for each profile and mode:

```text
saves/<profile>_normal.json
saves/<profile>_cheat.json
saves/player_profiles.json
```

The game saves automatically after important actions. Save writes are atomic, and the previous version is retained as a `.bak` file. If a primary save becomes corrupted, the game attempts to recover it from the backup.

Use **Settings > Save Management** to save immediately or restore the automatic backup.

Profile changes made through Admin are snapshotted under `profile_backups` before the change.

## Disclaimer

This is a play-money casino simulation for entertainment only. It does not use real money or provide gambling services.
