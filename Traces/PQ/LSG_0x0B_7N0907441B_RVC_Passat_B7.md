# LSG_0x0B_7N0907441B_RVC_Passat_B7

**Trace file**: `LSG_0x0B_7N0907441B_RVC_Passat_B7.crtd` (GVRET / SavvyCAN format)
**Part number**: 7N0 907 441 B (MQB-generation camera, factory-fitted to late PQ cars with MIB2 PQ)
**LSG**: 0x0B (rear view camera)
**Vehicle**: 2011 Passat B7
**Platform**: PQ, infotainment CAN
**Logger**: Teensy++ CAN logger. It drops frames: many continuation frames are missing, so most segmented messages don't reassemble.

**Filtered**: only LSG 0x0B frames on the two IDs below; everything else on the bus is removed.

## CAN IDs

| ID | Name | Direction |
|---|---|---|
| 0x6DC | BAP_RV | camera (FSG) → radio |
| 0x6CA | BAP_ASG_04 | radio (ASG) → camera |

## What I did

1. Activated the camera (PDC button)
2. Cycled camera modes: standard, trailer, parallel parking
3. Moved the brightness, contrast and colour sliders from minimum to maximum

(There are no timestamps for these, so matching actions to frames is still guesswork.)

## Decoded

**BAP_Config** `03 01 0B 00 05 01`: BAP 3.1, LSG class 0x0B, LSG version 5.1.

**FunctionList** `78 07 C3 E0 00 00 00 00`, MSB-first, bit N = FCT N:

```plaintext
78 -> 1,2,3,4          GetAll*, BAP_Config, FunctionList, HeartBeat
07 -> 13,14,15         0x0D, FSG-Setup, OperationState
C3 -> 16,17,22,23      0x10, 0x11, 0x16, 0x17
E0 -> 24,25,26         0x18, 0x19, 0x1A
```

\* Bit 1 (GetAll) is **set** here. Some PQ modules set it and most leave it clear (`0x38…`), see [Generic Functions](../../docs/protocol/generic-functions.md#0x03-functionlist). If you're emulating this one, copy the `0x78`.

**Heartbeat rotation.** One HeartbeatStatus per second, in FCT order, then back round. The run from 262.6 s to 287.6 s is complete:

```plaintext
262.61  32 C2 03 01 0B 00 05 01   0x02 BAP_Config
263.61  80 08 32 C3 78 07 C3 E0   0x03 FunctionList (start frame)
264.60  32 C4 0A                  0x04 HeartBeat = 1 s
265.61  32 CD 10                  0x0D
266.60  32 CE 00 01 0B 00 00 00   0x0E FSG-Setup
267.61  32 CF 02                  0x0F OperationState
268.61  32 D0 64                  0x10
269.61  32 D1 01 01               0x11
270.60  32 D6 38                  0x16
271.60  32 D7 32 01               0x17
272.59  32 D8 32 01               0x18
273.59  32 D9 1E 01               0x19
274.59  32 DA FF 00 00 00         0x1A
275.61  32 C2 03 01 0B 00 05 01   0x02 again
```

**Start-up** is at 8.48 s and again at 336.59 s. The BAP_Config Reset isn't in this trace either time. The same kind of camera sends one (`02 C2 03 01 0B 00 05 01`) in another car log, so the logger probably dropped it. The first start-up has `42 D6`, `42 CF`, `42 C2`. The second has `42 D6` and `42 CE`, then heartbeats.

**Radio requests** (0x6CA):

| Time | Frame | Meaning |
|---|---|---|
| 9.24 | `12 C1` | GetAll |
| 34.38 | `12 C2` | Get BAP_Config, followed by a StatusAll (`80 27 42 C1 …`, 39 bytes, partly lost) |
| 35.34 | `22 DA 00 00 00 00` | SetGet 0x1A |
| 127.9–138.5 | `22 DA 11 01 01 0x` | SetGet 0x1A, last byte 01 / 08 / 02 |

The camera reports 0x1A as Status, usually right after a write. Not every write has one in this log, and the `… 08` at 136.659 s comes just before the radio's own `… 08`. It also sends 0x1A values of its own (`F1 01 01 01`, `10 00 00 00`, `FF 00 00 00`).

## Observations

- **0x1A VPS_Display** is the only thing the radio writes. Its values change during the mode cycling, so it's the mode / view control. The last byte stepping 01 → 08 → 01 → 02 looks like the mode selector.
- **0x17 Color and 0x18 Contrast = `32 01`, 0x19 Brightness = `1E 01`**: `[0–100][01]`, so 50, 50 and 30. They don't change anywhere in this log, which is odd given the slider sweep, and may be down to dropped frames.
- **0x10 FollowUp_Time = `64`** (100) and **0x16 Car_State = `38`** never change.
- **0x0D FactorySettings = `10`**, the same value the tyre pressure module reports. See the [0x0D notes](../../docs/protocol/generic-functions.md#0x0d-the-system-switch).
- **OperationState** goes from 00 to 02 somewhere between 219 s (last 00) and 267 s (first 02). Maybe it's the camera turning off?

## Wanted

A cleaner capture, with a logger that doesn't drop frames and timestamps on each action, would settle 0x16–0x1A. Names are in the [DF 5.1 catalogue page](../../docs/catalogue/lsg0B-rvc-df5.1.md).
