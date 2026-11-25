# LSG 0x05 ACC, DF 3.0

Sizes are bytes; `var` = up to the size.

| FCT | Name | Class | Type | TX | RX |
|---|---|---|---|---|---|
| 0x01 | GetAll | Cache | byteSequence | 23 var | 0 |
| 0x02 | BAP-Config | Property | fixedByteSequence | 6 | 0 |
| 0x03 | FunctionList | Property | fixedByteSequence | 8 | 0 |
| 0x04 | HeartBeat | Property | int8 | 1 | 0 |
| 0x0F | FSG-OperationState | Property | int8 | 1 | 0 |
| 0x10 | GongState | Property | int8 | 1 | 1 |
| 0x11 | GongVolume | Property | int8 | 1 | 1 |
| 0x12 | DefaultMode | Property | int8 | 1 | 1 |
| 0x13 | TimeGap | Property | fixedByteSequence | 2 | 2 |
| 0x14 | DrivingProgram | Property | fixedByteSequence | 2 | 2 |
