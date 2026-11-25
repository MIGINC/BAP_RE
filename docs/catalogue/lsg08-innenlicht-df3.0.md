# LSG 0x08 Innenlicht, DF 3.0

Sizes are bytes; `var` = up to the size. **Car** = logged in car, sent by the module. **Sim** = simulated at the application layer: a MIB2 decoded the value, but it skipped the BAP protocol.

| FCT | Name | Class | Type | TX | RX | Car | Sim |
|---|---|---|---|---|---|---|---|
| 0x01 | GetAll | Cache | byteSequence | 21 var | 0 | yes |  |
| 0x02 | BAP-Config | Property | fixedByteSequence | 6 | 0 | yes |  |
| 0x03 | FunctionList | Property | fixedByteSequence | 8 | 0 | yes |  |
| 0x04 | HeartBeat | Property | int8 | 1 | 0 | yes |  |
| 0x0D | SetFactoryDefault | Property | int8 | 1 | 1 | yes |  |
| 0x0F | FSG-OperationState | Property | int8 | 1 | 0 | yes | yes |
| 0x10 | Colour | Property | fixedByteSequence | 2 | 2 | yes | yes |
| 0x11 | AmbianceLighting | Property | fixedByteSequence | 2 | 2 |  | yes |
