# LSG 0x0A OPS, DF 3.3

Sizes are bytes; `var` = up to the size.

| FCT | Name | Class | Type | TX | RX |
|---|---|---|---|---|---|
| 0x01 | GetAll | Cache | byteSequence | 50 var | 0 |
| 0x02 | BAP-Config | Property | fixedByteSequence | 6 | 0 |
| 0x03 | FunctionList | Property | byteSequence | 8 var | 0 |
| 0x04 | HeartBeat | Property | int8 | 1 | 0 |
| 0x0E | FSG-Setup | Property | fixedByteSequence | 3 | 0 |
| 0x0F | FSG-OperationState | Property | int8 | 1 | 0 |
| 0x10 | PDCSoundFront | Property | fixedByteSequence | 2 | 2 |
| 0x11 | PDCSoundRear | Property | fixedByteSequence | 2 | 2 |
| 0x12 | PDCDistanceFront | Property | fixedByteSequence | 4 | 0 |
| 0x13 | PDCDistanceRear | Property | fixedByteSequence | 4 | 0 |
| 0x14 | LifeMonitoring | Property | fixedByteSequence | 2 | 2 |
| 0x15 | PDCMute | Property | int8 | 1 | 1 |
| 0x16 | Fct_16 | Property | int8 | 1 | 1 |
| 0x17 | PDCDefaultParkingMode | Property | int8 | 1 | 1 |
| 0x18 | PDCTrailerHitched | Property | int8 | 1 | 0 |
| 0x19 | PDCSystemOnOff | Property | int8 | 1 | 1 |
| 0x1A | PDCAutoActivation | Property | fixedByteSequence | 2 | 2 |
| 0x1B | Fct_1B | Property | fixedByteSequence | 4 | 0 |
| 0x1C | Fct_1C | Property | fixedByteSequence | 4 | 0 |
