# LSG 0x0A OPS, DF 4.1

Sizes are bytes; `var` = up to the size. **Car** = logged in car, sent by the module.

| FCT | Name | Class | Type | TX | RX | Car |
|---|---|---|---|---|---|---|
| 0x01 | GetAll | Cache | byteSequence | 113 var | 0 | yes |
| 0x02 | BAP-Config | Property | fixedByteSequence | 6 | 0 | yes |
| 0x03 | FunctionList | Property | byteSequence | 8 var | 0 | yes |
| 0x04 | HeartBeat | Property | int8 | 1 | 0 | yes |
| 0x0D | PDCSystemOnOff | Property | int8 | 1 | 1 | yes |
| 0x0E | FSG-Setup | Property | fixedByteSequence | 6 | 0 | yes |
| 0x0F | FSG-OperationState | Property | int8 | 1 | 0 | yes |
| 0x10 | PDCSoundFront | Property | fixedByteSequence | 2 | 2 | yes |
| 0x11 | PDCSoundRear | Property | fixedByteSequence | 2 | 2 | yes |
| 0x12 | PDCDistanceFront | Property | fixedByteSequence | 6 | 0 | yes |
| 0x13 | PDCDistanceRear | Property | fixedByteSequence | 6 | 0 | yes |
| 0x14 | Fct_14 | Property | fixedByteSequence | 2 | 2 | yes |
| 0x15 | OPS_Control | Property | fixedByteSequence | 2 | 2 | yes |
| 0x16 | Fct_16 | Property | int8 | 1 | 1 | yes |
| 0x17 | PDCTrailerHitched | Property | int8 | 1 | 0 | yes |
| 0x18 | AutoActivation | Property | fixedByteSequence | 2 | 2 | yes |
| 0x19 | PDCDistanceLeft | Property | fixedByteSequence | 6 | 0 | yes |
| 0x1A | PDCDistanceRight | Property | fixedByteSequence | 6 | 0 | yes |
| 0x1B | Fct_1B | Property | fixedByteSequence | 2 | 2 |  |
| 0x1C | Fct_1C | Property | fixedByteSequence | 2 | 2 |  |
| 0x1D | Fct_1D | Property | fixedByteSequence | 2 | 2 |  |
| 0x1E | Fct_1E | Property | fixedByteSequence | 2 | 0 |  |
| 0x1F | PathDisplay | Property | fixedByteSequence | 6 | 0 | yes |
| 0x20 | Fct_20 | Property | fixedByteSequence | 2 | 2 | yes |
| 0x21 | Fct_21 | Property | int16 | 2 | 0 | yes |
| 0x22 | Fct_22 | Property | fixedByteSequence | 6 | 0 |  |
| 0x23 | Fct_23 | Property | fixedByteSequence | 6 | 0 |  |
| 0x24 | Fct_24 | Property | fixedByteSequence | 4 | 0 |  |
| 0x25 | Fct_25 | Property | fixedByteSequence | 5 | 5 |  |
| 0x26 | Fct_26 | Property | fixedByteSequence | 1 | 0 |  |
| 0x27 | Fct_27 | Property | int8 | 1 | 0 |  |
| 0x28 | Fct_28 | Property | fixedByteSequence | 2 | 2 |  |
| 0x29 | Fct_29 | Property | fixedByteSequence | 2 | 2 |  |
| 0x2A | Fct_2A | Property | fixedByteSequence | 2 | 2 |  |
| 0x2B | Fct_2B | Property | fixedByteSequence | 3 | 3 |  |
| 0x2C | Fct_2C | Property | fixedByteSequence | 2 | 2 |  |
