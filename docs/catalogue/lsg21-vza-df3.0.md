# LSG 0x21 VZA, DF 3.0

Sizes are bytes; `var` = up to the size. **Car** = logged in car, sent by the module. **Sim** = simulated at the application layer: a MIB2 decoded the value, but it skipped the BAP protocol.

| FCT | Name | Class | Type | TX | RX | Car | Sim |
|---|---|---|---|---|---|---|---|
| 0x01 | GetAll | Cache | byteSequence | 146 var | 0 | yes |  |
| 0x02 | BAP-Config | Property | fixedByteSequence | 6 | 0 | yes |  |
| 0x03 | FunctionList | Property | byteSequence | 8 var | 0 | yes |  |
| 0x04 | HeartBeat | Property | int8 | 1 | 0 | yes |  |
| 0x0D | TSDSystemOnOff | Property | int8 | 1 | 1 | yes | yes |
| 0x0E | FSG-Setup | Property | int8 | 1 | 0 | yes | yes |
| 0x0F | FSG-OperationState | Property | int8 | 1 | 0 | yes | yes |
| 0x10 | RoadSigns | Property | byteSequence | 120 var | 0 | yes | yes |
| 0x11 | RoadSignFilter | Property | fixedByteSequence | 2 | 2 | yes | yes |
| 0x12 | TrailerMode | Property | int8 | 1 | 0 | yes | yes |
| 0x13 | SystemMessages | Property | int8 | 1 | 0 | yes | yes |
