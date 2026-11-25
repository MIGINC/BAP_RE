# LSG 0x1A SWA, DF 3.0

Sizes are bytes; `var` = up to the size.

| FCT | Name | Class | Type | TX | RX |
|---|---|---|---|---|---|
| 0x01 | GetAll | Cache | byteSequence | 24 var | 0 |
| 0x02 | BAP-Config | Property | fixedByteSequence | 6 | 0 |
| 0x03 | FunctionList | Property | fixedByteSequence | 8 | 0 |
| 0x04 | HeartBeat | Property | int8 | 1 | 0 |
| 0x0F | FSG-OperationState | Property | int8 | 1 | 0 |
| 0x10 | Country | Property | int8 | 1 | 1 |
| 0x11 | Brightness | Property | fixedByteSequence | 2 | 2 |
| 0x12 | WarningTime | Property | fixedByteSequence | 2 | 2 |
| 0x13 | FSG_Control | Property | int8 | 1 | 1 |
| 0x14 | Gong | Property | int8 | 1 | 1 |
| 0x15 | Gong_Volume | Property | int8 | 1 | 1 |
