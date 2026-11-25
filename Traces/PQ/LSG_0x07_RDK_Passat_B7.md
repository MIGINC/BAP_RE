# LSG_0x07_RDK_Passat_B7

**Trace file**: `LSG_0x07_RDK_Passat_B7.crtd` (GVRET / SavvyCAN format)
**LSG**: 0x07 (tyre pressure), DF 4.0
**Vehicle**: Passat B7
**Platform**: PQ, ECAN, 500 kbit/s

**Filtered**: only LSG 0x07 frames on the two IDs below. Time starts at the module's BAP_Config.

## CAN IDs

| ID | Name | Direction |
|---|---|---|
| 0x6DD | BAP_RDK | RDK (FSG) → cluster |
| 0x6C8 | BAP_ASG_01 | cluster (ASG) → RDK |

## What it shows

Ignition on: BAP_Config, GetAll and StatusAll, the tyre size list, then the heartbeat rotation. The car is set to psi. The RDK starts in bar and switches at 0.7 s, see [0x07 RDK](../../docs/lsg/0x07-rdk.md#pressures-and-units).

## Expected values

The first of each:

| Time | Message | Pressures | Targets | Status |
|---|---|---|---|---|
| 0.350 | StatusAll, 39 bytes | 4 × 2.70 bar, spare none (unit 00) | 5 × 2.80 bar (unit 00) | `00 00 00 00 50 00` |
| 0.700 | 0x11 | 4 × 2.69 bar, spare none (unit 01) | | |
| 0.751 | 0x13 | | 5 × 2.83 bar (unit 01) | |
| 7.819 | 0x10 | | | `00 00 00 00 50 00` |

The 0x14 answer (35 bytes) starts at 0.400 and finishes at 0.800, with three single frames in the middle.
