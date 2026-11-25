# LSG 0x07 RDK, DF 4.0

Sizes are bytes; `var` = up to the size. **Car** = logged in car, sent by the module. **Sim** = simulated at the application layer: a MIB2 decoded the value, but it skipped the BAP protocol.

| FCT | Name | Class | Type | TX | RX | Car | Sim |
|---|---|---|---|---|---|---|---|
| 0x01 | GetAll | Cache | byteSequence | 48 var | 0 | yes |  |
| 0x02 | BAP-Config | Property | fixedByteSequence | 6 | 0 | yes |  |
| 0x03 | FunctionList | Property | byteSequence | 8 var | 0 | yes |  |
| 0x04 | HeartBeat | Property | int8 | 1 | 0 | yes |  |
| 0x0D | SystemOnOff | Property | int8 | 1 | 1 | yes | yes |
| 0x0E | FSG-Setup | Property | fixedByteSequence | 2 | 0 | yes | yes |
| 0x0F | FSG-OperationState | Property | int8 | 1 | 0 | yes | yes |
| 0x10 | TPMS_Status | Property | fixedByteSequence | 6 | 0 | yes | yes |
| 0x11 | TirePressure | Property | fixedByteSequence | 6 | 0 | yes | yes |
| 0x12 | TireTemperature | Property | fixedByteSequence | 6 | 0 |  |  |
| 0x13 | TargetPressure | Property | fixedByteSequence | 6 | 0 | yes | yes |
| 0x14 | TireSizeList | Array | byteSequence | 683 var | 3 | yes |  |
| 0x15 | Fct_15 | Property | int8 | 1 | 1 |  |  |
| 0x16 | TireChanged | Method | int8 | 1 | 0 |  | yes |
| 0x17 | WheelSet | Method | fixedByteSequence | 2 | 2 | yes | yes |
| 0x18 | Fct_18 | Property | fixedByteSequence | 2 | 2 |  | yes |

## Layouts

### 0x14 TireSizeList

array, header `arrayheader`, up to 11 elements:
- element:
  - `Text`: string

## Records

- `arrayheader`:
  - `Mode`: nibble_hi (bits 0 Shift, 1 Backward, 2 PosTransmitted, 3 IndexSize16)
  - `RecordAddress`: nibble_lo
  - `Start`: index
  - `Elements`: index
