# LSG 0x22 MKE, DF 3.0

Sizes are bytes; `var` = up to the size. **Sim** = simulated at the application layer: a MIB2 decoded the value, but it skipped the BAP protocol.

| FCT | Name | Class | Type | TX | RX | Sim |
|---|---|---|---|---|---|---|
| 0x01 | GetAll | Cache | byteSequence | 20 var | 0 |  |
| 0x02 | BAP-Config | Property | fixedByteSequence | 6 | 0 |  |
| 0x03 | FunctionList | Property | byteSequence | 8 var | 0 |  |
| 0x04 | HeartBeat | Property | int8 | 1 | 0 |  |
| 0x0D | MKESystemOnOff | Property | int8 | 1 | 1 | yes |
| 0x0E | FSG-Setup | Property | void | 0 | 0 |  |
| 0x0F | FSG-OperationState | Property | int8 | 1 | 0 | yes |
| 0x10 | Fct_10 | Property | int8 | 1 | 0 |  |
