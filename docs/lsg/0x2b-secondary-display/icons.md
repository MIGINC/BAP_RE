# Icon Mappings

Icons in Secondary Display text are ordinary UTF-8 characters from the Unicode Private Use Area. The cluster swaps them for its own glyphs, and you just drop them into the cell text next to normal characters.

This is the table my emulator's screen demo uses. It doesn't agree with the table I had in older notes: that one had signal at U+F003, and battery running the other way round. Treat any icon you haven't seen drawn on your own cluster as untested.

### Signal strength

Signal is **five characters side by side**, not one icon.

| Position | Filled | Empty |
|---|---|---|
| 1 to 5 | U+F803 to U+F807 (`EF A0 83` to `87`) | U+F808 to U+F80C (`EF A0 88` to `8C`) |

| Logged in car | Characters |
|---|---|
| "No network" | F808 F809 F80A F80B F80C |
| with an operator | F803 F804 F80A F80B F80C |

Drawn on an emulated cluster: F803 to F807 are filled bars of rising height, F808 to F80C the same ones empty (tested with cluster). FC00 to FC04 and F7FE draw as listed here too.

### Battery

| Unicode | UTF-8 | Meaning |
|---|---|---|
| U+FC00 | `EF B0 80` | full |
| U+FC01 | `EF B0 81` | 3 bars |
| U+FC02 | `EF B0 82` | 2 bars |
| U+FC03 | `EF B0 83` | 1 bar |
| U+FC04 | `EF B0 84` | empty |

### Phone number category

| Unicode | UTF-8 | Meaning |
|---|---|---|
| U+F7F4 | `EF 9F B4` | office |
| U+F7F5 | `EF 9F B5` | home |
| U+F7F6 | `EF 9F B6` | mobile |
| U+F7F9 | `EF 9F B9` | other |

### Messages

| Unicode | UTF-8 | Meaning |
|---|---|---|
| U+F7F7 | `EF 9F B7` | SMS read |
| U+F7F8 | `EF 9F B8` | SMS unread |
| U+F7FB | `EF 9F BB` | new SMS |

### Status

| Unicode | UTF-8 | Meaning |
|---|---|---|
| U+F7FD | `EF 9F BD` | missed calls |
| U+F7FE | `EF 9F BE` | Bluetooth |
| U+F7FF | `EF 9F BF` | Bluetooth visible |
| U+F800 | `EF A0 80` | headset |
| U+FB0F | `EF AC 8F` | player |
| U+FC0B | `EF B0 8B` | roaming |

### Logged in car

These code points are in a car log: U+F7FE, U+F803, U+F804, U+F808–U+F80C, U+FC00, U+FC04, and U+FC16–U+FC18, which aren't in the tables above and whose meaning I don't know.

### Status lines

In the car log, cell 2 is battery, space, Bluetooth (`EF B0 80 20 EF 9F BE`) and cell 3 is the five signal characters.

Related: [Secondary Display Overview](README.md) · [Constants and Definitions](constants.md)
