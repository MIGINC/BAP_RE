# LSG 0x32 Navigation, DF 3.0

Sizes are bytes; `var` = up to the size. **Car** = logged in car, sent by the module.

| FCT | Name | Class | Type | TX | RX | Car |
|---|---|---|---|---|---|---|
| 0x01 | GetAll | Cache | byteSequence | 201 var | 0 | yes |
| 0x02 | BAP-Config | Property | fixedByteSequence | 6 | 0 | yes |
| 0x03 | FunctionList | Property | byteSequence | 8 var | 0 | yes |
| 0x04 | HeartBeat | Property | int8 | 1 | 0 | yes |
| 0x0F | FSG-OperationState | Property | int8 | 1 | 0 | yes |
| 0x10 | CompassInfo | Property | fixedByteSequence | 3 | 0 | yes |
| 0x11 | RG_Status | Property | int8 | 1 | 0 | yes |
| 0x12 | DistanceToNextManeuver | Property | byteSequence | 7 var | 0 | yes |
| 0x13 | CurrentPositionInfo | Property | byteSequence | 75 var | 0 | yes |
| 0x14 | TurnToInfo | Property | byteSequence | 75 var | 0 | yes |
| 0x15 | DistanceTimeToDestination | Property | byteSequence | 11 var | 0 | yes |
| 0x16 | ManeuverDescriptor | Array | byteSequence | 63 var | 3 | yes |
| 0x17 | LaneGuidance | Array | byteSequence | 20 var | 3 |  |
| 0x18 | Fct_18 | Property | void | 0 | 0 |  |
| 0x19 | MagnetFieldZone | Property | int8 | 1 | 1 |  |
| 0x1A | Calibration | Property | int8 | 1 | 1 |  |
| 0x1B | Fct_1B | Property | fixedByteSequence | 1 | 1 |  |

## Layouts

### 0x16 ManeuverDescriptor

array, header `arrayheader`, up to 3 elements:
- element:
  - `A`: u8 (ra_in [0, 1])
  - `B`: u8 (ra_in [0, 2])
  - `C`: u8 (ra_in [0, 3])
  - `Text`: string (max 16; ra_in [0, 4])

## Records

- `arrayheader`:
  - `Mode`: nibble_hi (bits 0 Shift, 1 Backward, 2 PosTransmitted, 3 IndexSize16)
  - `RecordAddress`: nibble_lo
  - `Start`: index
  - `Elements`: index
