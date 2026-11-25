# LSG 0x1A SWA, DF 3.4

Sizes are bytes; `var` = up to the size. **Car** = logged in car, sent by the module. **Sim** = simulated at the application layer: a MIB2 decoded the value, but it skipped the BAP protocol.

| FCT | Name | Class | Type | TX | RX | Car | Sim |
|---|---|---|---|---|---|---|---|
| 0x01 | GetAll | Cache | byteSequence | 41 var | 0 | yes |  |
| 0x02 | BAP-Config | Property | fixedByteSequence | 6 | 0 | yes |  |
| 0x03 | FunctionList | Property | fixedByteSequence | 8 | 0 | yes |  |
| 0x04 | HeartBeat | Property | int8 | 1 | 0 | yes |  |
| 0x0F | FSG-OperationState | Property | int8 | 1 | 0 | yes |  |
| 0x10 | Country | Property | int8 | 1 | 1 |  | yes |
| 0x11 | Brightness | Property | fixedByteSequence | 2 | 2 | yes | yes |
| 0x12 | WarningTime | Property | fixedByteSequence | 2 | 2 |  | yes |
| 0x13 | FSG_Control | Property | int8 | 1 | 1 | yes | yes |
| 0x14 | Gong | Property | int8 | 1 | 1 |  | yes |
| 0x15 | SWAGongVolume | Property | int8 | 1 | 1 |  | yes |
| 0x16 | Configuration | Property | fixedByteSequence | 3 | 0 | yes |  |
| 0x17 | RCTA | Property | fixedByteSequence | 2 | 2 | yes | yes |
| 0x18 | RCTA_SensorData | Property | fixedByteSequence | 2 | 0 | yes | yes |
| 0x19 | ExitAssist | Property | fixedByteSequence | 2 | 2 |  | yes |
| 0x1A | Fct_1A | Property | fixedByteSequence | 2 | 0 |  |  |
| 0x1B | RCTA_BrakeIntervention | Property | fixedByteSequence | 2 | 2 |  |  |
| 0x1C | Personalisation | Array | byteSequence | 34 var | 31 |  |  |
| 0x1D | Fct_1D | Property | fixedByteSequence | 2 | 2 |  |  |
