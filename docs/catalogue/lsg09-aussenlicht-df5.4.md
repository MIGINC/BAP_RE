# LSG 0x09 Aussenlicht, DF 5.4

Sizes are bytes; `var` = up to the size. **Sim** = simulated at the application layer: a MIB2 decoded the value, but it skipped the BAP protocol.

| FCT | Name | Class | Type | TX | RX | Sim |
|---|---|---|---|---|---|---|
| 0x01 | GetAll | Cache | byteSequence | 55 var | 0 |  |
| 0x02 | BAP-Config | Property | fixedByteSequence | 6 | 0 |  |
| 0x03 | FunctionList | Property | byteSequence | 8 var | 0 |  |
| 0x04 | HeartBeat | Property | int8 | 1 | 0 |  |
| 0x0D | ExtLightSetFactoryDefault | Property | int8 | 1 | 1 |  |
| 0x0E | FSG-Setup | Property | int8 | 1 | 0 | yes |
| 0x0F | FSG-OperationState | Property | int8 | 1 | 0 | yes |
| 0x10 | ExtLightDayLight | Property | fixedByteSequence | 2 | 2 | yes |
| 0x11 | ExtLightTourist | Property | fixedByteSequence | 2 | 2 | yes |
| 0x12 | ExtLightComingHome | Property | fixedByteSequence | 2 | 2 | yes |
| 0x13 | ExtLightLeavingHome | Property | fixedByteSequence | 2 | 2 | yes |
| 0x14 | ExtLightSwitchOnSensitivity | Property | fixedByteSequence | 2 | 2 | yes |
| 0x15 | ExtLightLampErrorDetection | Property | fixedByteSequence | 6 | 0 | yes |
| 0x16 | ExtLightMotorwayBlinking | Property | fixedByteSequence | 3 | 3 | yes |
| 0x17 | ExtLightLampErrorDetectionTrailer | Property | int8 | 1 | 0 | yes |
| 0x18 | ExtLightAdaptive | Property | fixedByteSequence | 2 | 2 | yes |
| 0x19 | ExtLightHeadLightSystem | Property | fixedByteSequence | 2 | 2 | yes |
| 0x1A | ExtLightGlidingLightSystem | Property | fixedByteSequence | 2 | 2 | yes |
| 0x1B | ExtLightMaskedHighBeam | Property | fixedByteSequence | 2 | 2 | yes |
| 0x1C | Fct_1C | Property | int8 | 1 | 0 |  |
| 0x1D | ExtLightAutomaticLight | Property | fixedByteSequence | 2 | 2 | yes |
| 0x1E | ExtLightLaserLight | Property | fixedByteSequence | 2 | 2 | yes |
| 0x1F | ExtLightHeadlightRange | Property | fixedByteSequence | 2 | 2 | yes |
