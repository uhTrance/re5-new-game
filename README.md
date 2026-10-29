# RE5 New Game

Resets your Resident Evil 5 (Steam) save to an untouched new game, with **Professional difficulty already unlocked**. One small `.exe`, nothing to install.

## Use

1. Close Resident Evil 5.
2. Run `RE5 New Game.exe` (from [Releases](../../releases)).
3. Start the game. If Steam offers to download a cloud save, **refuse it**, or it puts your old save back.

It finds your save on its own (`Steam\userdata\<id>\21690\remote\savedata.bin`). If more than one Steam account on the PC has an RE5 save, it asks which one. You can also drag a `savedata.bin` onto the exe.

Your old save is copied to a `RE5 save backups` folder next to the exe first. To go back, close the game and copy a backup over `savedata.bin` (renaming it to `savedata.bin`).

## What it changes

Everything. All story progress, unlocks, money, points, inventory and stats go, and so do the options (controls, brightness, volume), which live in the same file. Only your Steam ID is kept, because the game rejects a save made under another ID. The save date is set to now.

It does not copy anyone else's save. A new-game save is all zeros apart from about 200 values, which are listed in [`newgame_table.h`](newgame_table.h), so the tool clears your own save, sets those values and recalculates the checksum.

## Save format

`savedata.bin` is 0x5D50 bytes. Each 8-byte block is XORed with the previous encoded block; the first block is XORed with the seed `00 21 11 08 C0 4B 00 00`. Once decoded:

| Offset | Contents |
| --- | --- |
| `0x00` | Steam ID (64-bit) |
| `0x08` | Checksum: 32-bit sum of the dwords at `0x10`–`0x3BAC` and `0x3BB0`–`0x52AC` |
| `0x10` | Save date: year, month, day, hour, minute, second (dwords) |

Credit to [shinneider's RE5 Save Editor](https://github.com/shinneider/RE5-Save-Editor) for working out the encoding and checksum.

## Build

Needs Visual Studio with the C++ tools. Run `build_newgame.bat`. It produces `RE5 New Game.exe`, a 32-bit exe with the C runtime linked in statically.

Steam version only. Not affiliated with Capcom.
