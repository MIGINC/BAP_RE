# LSG 0x21 VZA, DF 3.3

Sizes are bytes; `var` = up to the size.

| FCT | Name | Class | Type | TX | RX |
|---|---|---|---|---|---|
| 0x01 | GetAll | Cache | byteSequence | 149 var | 0 |
| 0x02 | BAP-Config | Property | fixedByteSequence | 6 | 0 |
| 0x03 | FunctionList | Property | byteSequence | 8 var | 0 |
| 0x04 | HeartBeat | Property | int8 | 1 | 0 |
| 0x0D | TSDSystemOnOff | Property | int8 | 1 | 1 |
| 0x0E | FSG-Setup | Property | int8 | 1 | 0 |
| 0x0F | FSG-OperationState | Property | int8 | 1 | 0 |
| 0x10 | RoadSigns | Property | byteSequence | 123 var | 0 |
| 0x11 | RoadSignFilter | Property | fixedByteSequence | 2 | 2 |
| 0x12 | TrailerMode | Property | int8 | 1 | 0 |
| 0x13 | SystemMessages | Property | int8 | 1 | 0 |
