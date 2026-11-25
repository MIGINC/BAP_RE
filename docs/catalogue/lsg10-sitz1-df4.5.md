# LSG 0x10 Sitz1, DF 4.5

Sizes are bytes; `var` = up to the size. **Sim** = simulated at the application layer: a MIB2 decoded the value, but it skipped the BAP protocol.

| FCT | Name | Class | Type | TX | RX | Sim |
|---|---|---|---|---|---|---|
| 0x01 | GetAll | Property | fixedByteSequence | 101 | 0 |  |
| 0x02 | BAP-Config | Property | fixedByteSequence | 6 | 0 |  |
| 0x03 | FunctionList | Property | fixedByteSequence | 8 | 0 |  |
| 0x04 | HeartBeat | Property | int8 | 1 | 0 |  |
| 0x0E | FSG-Setup | Property | fixedByteSequence | 3 | 0 | yes |
| 0x0F | FSG-OperationState | Property | int8 | 1 | 0 | yes |
| 0x10 | SeatRadioKeyAutomatic | Property | fixedByteSequence | 2 | 2 | yes |
| 0x17 | SeatEasyEntry | Property | fixedByteSequence | 1 | 1 | yes |
| 0x28 | Fct_28 | Array | byteSequence | 48 var | 45 |  |
