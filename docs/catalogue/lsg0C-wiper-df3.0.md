# LSG 0x0C Wiper, DF 3.0

Sizes are bytes; `var` = up to the size.

| FCT | Name | Class | Type | TX | RX |
|---|---|---|---|---|---|
| 0x01 | GetAll | Cache | byteSequence | 19 var | 0 |
| 0x02 | BAP-Config | Property | fixedByteSequence | 6 | 0 |
| 0x03 | FunctionList | Property | fixedByteSequence | 8 | 0 |
| 0x04 | HeartBeat | Property | int8 | 1 | 0 |
| 0x0D | WiperSetFactoryDefault | Property | int8 | 1 | 1 |
| 0x0F | FSG-OperationState | Property | int8 | 1 | 0 |
| 0x10 | WiperServicePosition | Property | fixedByteSequence | 1 | 1 |
| 0x12 | WiperRearWiping | Property | int8 | 1 | 1 |
