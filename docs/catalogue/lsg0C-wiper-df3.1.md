# LSG 0x0C Wiper, DF 3.1

Sizes are bytes; `var` = up to the size. **Sim** = simulated at the application layer: a MIB2 decoded the value, but it skipped the BAP protocol.

| FCT | Name | Class | Type | TX | RX | Sim |
|---|---|---|---|---|---|---|
| 0x01 | GetAll | Cache | byteSequence | 26 var | 0 |  |
| 0x02 | BAP-Config | Property | fixedByteSequence | 6 | 0 |  |
| 0x03 | FunctionList | Property | byteSequence | 8 var | 0 |  |
| 0x04 | HeartBeat | Property | int8 | 1 | 0 |  |
| 0x0D | WiperSetFactoryDefault | Property | int8 | 1 | 1 |  |
| 0x0F | FSG-OperationState | Property | int8 | 1 | 0 | yes |
| 0x10 | WiperServicePosition | Property | fixedByteSequence | 1 | 1 |  |
| 0x12 | WiperRearWiping | Property | int8 | 1 | 1 | yes |
| 0x13 | WiperTearsWiping | Property | int8 | 1 | 1 | yes |
| 0x14 | WiperRainSensorOnOff | Property | int8 | 1 | 1 | yes |
| 0x15 | WiperRainSensorConfig | Property | int8 | 1 | 1 | yes |
| 0x16 | EasyEntrySteeringColumn | Property | fixedByteSequence | 2 | 2 | yes |
