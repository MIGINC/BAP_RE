# LSG 0x19 LDW, DF 4.1

Sizes are bytes; `var` = up to the size.

| FCT | Name | Class | Type | TX | RX |
|---|---|---|---|---|---|
| 0x01 | GetAll | Cache | byteSequence | 26 var | 0 |
| 0x02 | BAP-Config | Property | fixedByteSequence | 6 | 0 |
| 0x03 | FunctionList | Property | byteSequence | 8 var | 0 |
| 0x04 | HeartBeat | Property | int8 | 1 | 0 |
| 0x0D | LDWHCASystemOnOff | Property | int8 | 1 | 1 |
| 0x0E | FSG-Setup | Property | int8 | 1 | 0 |
| 0x0F | FSG-OperationState | Property | int8 | 1 | 0 |
| 0x10 | WarningIntensity | Property | fixedByteSequence | 2 | 2 |
| 0x11 | WarningTime | Property | fixedByteSequence | 2 | 2 |
| 0x12 | Systemmode | Property | fixedByteSequence | 2 | 2 |
