# LSG 0x03 ParkHeater, DF 3.0

Sizes are bytes; `var` = up to the size. **Sim** = simulated at the application layer: a MIB2 decoded the value, but it skipped the BAP protocol.

| FCT | Name | Class | Type | TX | RX | Sim |
|---|---|---|---|---|---|---|
| 0x01 | GetAll | Cache | byteSequence | 44 var | 0 |  |
| 0x02 | BAP-Config | Property | fixedByteSequence | 6 | 0 |  |
| 0x03 | FunctionList | Property | fixedByteSequence | 8 | 0 |  |
| 0x04 | HeartBeat | Property | int8 | 1 | 0 |  |
| 0x0D | AuxHeaterSetFactoryDefault | Property | int8 | 1 | 1 |  |
| 0x0F | FSG-OperationState | Property | int8 | 1 | 0 | yes |
| 0x10 | Fct_10 | Property | int8 | 1 | 1 |  |
| 0x11 | Duration | Property | fixedByteSequence | 2 | 2 |  |
| 0x12 | Ventilation | Property | int8 | 1 | 1 |  |
| 0x13 | EngineHeater | Property | int8 | 1 | 1 | yes |
| 0x14 | SystemMessages | Property | int8 | 1 | 0 | yes |
| 0x15 | Activation | Property | int8 | 1 | 1 |  |
| 0x16 | DepartureTime1 | Property | fixedByteSequence | 6 | 6 |  |
| 0x17 | DepartureTime2 | Property | fixedByteSequence | 6 | 6 |  |
| 0x18 | DepartureTime3 | Property | fixedByteSequence | 6 | 6 |  |
| 0x19 | Weekday | Property | int8 | 1 | 1 |  |
| 0x1A | HeatVentMode | Property | int8 | 1 | 1 | yes |
