# LSG 0x1B AWV, DF 3.2

Sizes are bytes; `var` = up to the size. **Car** = logged in car, sent by the module.

| FCT | Name | Class | Type | TX | RX | Car |
|---|---|---|---|---|---|---|
| 0x01 | GetAll | Cache | byteSequence | 28 var | 0 | yes |
| 0x02 | BAP-Config | Property | fixedByteSequence | 6 | 0 | yes |
| 0x03 | FunctionList | Property | fixedByteSequence | 8 | 0 | yes |
| 0x04 | HeartBeat | Property | int8 | 1 | 0 | yes |
| 0x0F | FSG-OperationState | Property | int8 | 1 | 0 | yes |
| 0x10 | AWVSystemOnOff | Property | int8 | 1 | 1 | yes |
| 0x11 | AWVGong | Property | int8 | 1 | 1 |  |
| 0x12 | Fct_12 | Property | int8 | 1 | 1 |  |
| 0x13 | AWVWarning | Property | fixedByteSequence | 2 | 2 | yes |
| 0x14 | Fct_14 | Property | int8 | 1 | 1 | yes |
| 0x15 | Fct_15 | Property | fixedByteSequence | 6 | 0 | yes |
