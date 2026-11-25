# LSG 0x10 Sitz1, DF 4.0

Sizes are bytes; `var` = up to the size.

| FCT | Name | Class | Type | TX | RX |
|---|---|---|---|---|---|
| 0x01 | GetAll | Cache | void | 0 | 0 |
| 0x02 | BAP-Config | Property | fixedByteSequence | 6 | 0 |
| 0x03 | FunctionList | Property | fixedByteSequence | 8 | 0 |
| 0x04 | HeartBeat | Property | int8 | 1 | 0 |
| 0x0E | FSG-Setup | Property | fixedByteSequence | 3 | 0 |
| 0x0F | FSG-OperationState | Property | int8 | 1 | 0 |
| 0x10 | SeatRadioKeyAutomatic | Property | fixedByteSequence | 2 | 2 |
| 0x17 | SeatEasyEntry | Property | fixedByteSequence | 1 | 1 |
