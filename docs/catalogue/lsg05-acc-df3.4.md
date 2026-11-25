# LSG 0x05 ACC, DF 3.4

Sizes are bytes; `var` = up to the size. **Car** = logged in car, sent by the module. **Sim** = simulated at the application layer: a MIB2 decoded the value, but it skipped the BAP protocol.

| FCT | Name | Class | Type | TX | RX | Car | Sim |
|---|---|---|---|---|---|---|---|
| 0x01 | GetAll | Cache | byteSequence | 39 var | 0 | yes |  |
| 0x02 | BAP-Config | Property | fixedByteSequence | 6 | 0 | yes |  |
| 0x03 | FunctionList | Property | fixedByteSequence | 8 | 0 | yes |  |
| 0x04 | HeartBeat | Property | int8 | 1 | 0 | yes |  |
| 0x0F | FSG-OperationState | Property | int8 | 1 | 0 | yes |  |
| 0x10 | GongState | Property | int8 | 1 | 1 |  | yes |
| 0x11 | GongVolume | Property | int8 | 1 | 1 |  | yes |
| 0x12 | DefaultMode | Property | int8 | 1 | 1 |  | yes |
| 0x13 | TimeGap | Property | fixedByteSequence | 2 | 2 | yes | yes |
| 0x14 | DrivingProgram | Property | fixedByteSequence | 2 | 2 | yes | yes |
| 0x15 | CurveAssist | Property | fixedByteSequence | 2 | 2 |  | yes |
| 0x16 | SpeedLimitAdoption | Property | fixedByteSequence | 2 | 2 |  | yes |
| 0x17 | SpeedLimitOffset | Property | fixedByteSequence | 3 | 3 |  | yes |
| 0x18 | TrafficJamAssist | Property | fixedByteSequence | 2 | 2 |  | yes |
| 0x19 | DistanceWarning | Property | fixedByteSequence | 3 | 3 |  | yes |
| 0x1A | FSG_Setup | Property | fixedByteSequence | 3 | 0 | yes | yes |
| 0x1B | FSG_Control | Property | int8 | 1 | 1 |  |  |
