# LSG 0x0B RVC, DF 3.1

Sizes are bytes; `var` = up to the size.

| FCT | Name | Class | Type | TX | RX |
|---|---|---|---|---|---|
| 0x01 | GetAll | Cache | byteSequence | 27 var | 0 |
| 0x02 | BAP-Config | Property | fixedByteSequence | 6 | 0 |
| 0x03 | FunctionList | Property | byteSequence | 8 var | 0 |
| 0x04 | HeartBeat | Property | int8 | 1 | 0 |
| 0x0F | FSG-OperationState | Property | int8 | 1 | 0 |
| 0x10 | FollowUp_Time | Property | int8 | 1 | 1 |
| 0x11 | Fct_11 | Property | int8 | 1 | 1 |
| 0x12 | Parking_Mode | Property | int8 | 1 | 1 |
| 0x13 | Car_State | Property | int8 | 1 | 0 |
| 0x14 | Color | Property | int8 | 1 | 1 |
| 0x15 | Contrast | Property | int8 | 1 | 1 |
| 0x16 | Brightness | Property | int8 | 1 | 1 |
| 0x17 | VPS_Display | Property | fixedByteSequence | 2 | 2 |
