# LSG 0x09 Aussenlicht, DF 4.3

Sizes are bytes; `var` = up to the size. **Car** = logged in car, sent by the module. **Sim** = simulated at the application layer: a MIB2 decoded the value, but it skipped the BAP protocol.

| FCT | Name | Class | Type | TX | RX | Car | Sim |
|---|---|---|---|---|---|---|---|
| 0x01 | GetAll | Cache | byteSequence | 36 var | 0 | yes |  |
| 0x02 | BAP-Config | Property | fixedByteSequence | 6 | 0 | yes |  |
| 0x03 | FunctionList | Property | fixedByteSequence | 8 | 0 | yes |  |
| 0x04 | HeartBeat | Property | int8 | 1 | 0 | yes |  |
| 0x0D | ExtLightSetFactoryDefault | Property | int8 | 1 | 1 | yes |  |
| 0x0F | FSG-OperationState | Property | int8 | 1 | 0 | yes |  |
| 0x10 | ExtLightDayLight | Property | int8 | 1 | 1 | yes |  |
| 0x12 | ExtLightComingHome | Property | fixedByteSequence | 2 | 2 | yes | yes |
| 0x13 | ExtLightLeavingHome | Property | fixedByteSequence | 2 | 2 | yes | yes |
| 0x15 | ExtLightLampErrorDetection | Property | fixedByteSequence | 6 | 0 | yes | yes |
| 0x16 | ExtLightMotorwayBlinking | Property | fixedByteSequence | 2 | 2 | yes |  |
| 0x17 | ExtLightLampErrorDetectionTrailer | Property | int8 | 1 | 0 | yes | yes |
| 0x19 | ExtLightHeadLightSystem | Property | fixedByteSequence | 2 | 2 | yes | yes |
| 0x1A | ExtLightGlidingLightSystem | Property | int8 | 1 | 0 |  |  |
| 0x1B | ExtLightMaskedHighBeam | Property | fixedByteSequence | 2 | 0 | yes | yes |
