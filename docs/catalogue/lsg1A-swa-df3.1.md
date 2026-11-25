# LSG 0x1A SWA, DF 3.1

Sizes are bytes; `var` = up to the size. **Sim** = simulated at the application layer: a MIB2 decoded the value, but it skipped the BAP protocol.

| FCT | Name | Class | Type | TX | RX | Sim |
|---|---|---|---|---|---|---|
| 0x01 | GetAll | Cache | byteSequence | 33 var | 0 |  |
| 0x02 | BAP-Config | Property | fixedByteSequence | 6 | 0 |  |
| 0x03 | FunctionList | Property | fixedByteSequence | 8 | 0 |  |
| 0x04 | HeartBeat | Property | int8 | 1 | 0 |  |
| 0x0F | FSG-OperationState | Property | int8 | 1 | 0 |  |
| 0x10 | Country | Property | int8 | 1 | 1 | yes |
| 0x11 | Brightness | Property | fixedByteSequence | 2 | 2 | yes |
| 0x12 | WarningTime | Property | fixedByteSequence | 2 | 2 | yes |
| 0x13 | FSG_Control | Property | int8 | 1 | 1 | yes |
| 0x14 | Gong | Property | int8 | 1 | 1 | yes |
| 0x15 | Gong_Volume | Property | int8 | 1 | 1 | yes |
| 0x16 | Configuration | Property | fixedByteSequence | 3 | 0 |  |
| 0x17 | RCTA | Property | fixedByteSequence | 2 | 2 | yes |
| 0x18 | RCTA_SensorData | Property | fixedByteSequence | 2 | 0 | yes |
| 0x19 | ExitAssist | Property | fixedByteSequence | 2 | 2 | yes |
