# LSG 0x02 Klima2, DF 3.3

Sizes are bytes; `var` = up to the size. **Sim** = simulated at the application layer: a MIB2 decoded the value, but it skipped the BAP protocol.

| FCT | Name | Class | Type | TX | RX | Sim |
|---|---|---|---|---|---|---|
| 0x01 | GetAll | Cache | byteSequence | 42 var | 0 |  |
| 0x02 | BAP-Config | Property | fixedByteSequence | 6 | 0 |  |
| 0x03 | FunctionList | Property | byteSequence | 8 var | 0 |  |
| 0x04 | HeartBeat | Property | int8 | 1 | 0 |  |
| 0x0D | AirconSystemOnOffRow2 | Property | int8 | 1 | 1 | yes |
| 0x0F | FSG-OperationState | Property | int8 | 1 | 0 | yes |
| 0x1B | AirconTempZone3 | Property | fixedByteSequence | 2 | 2 | yes |
| 0x1C | AirconAirVolumeZone3 | Property | int8 | 1 | 1 | yes |
| 0x1D | AirconAirVolumeRegZone3 | Property | int8 | 1 | 1 | yes |
| 0x1E | AirconAirDistributionZone3 | Property | fixedByteSequence | 4 | 4 | yes |
| 0x1F | AirconSeatHeaterZone3 | Property | int8 | 1 | 1 | yes |
| 0x20 | AirconSeatVentilationZone3 | Property | int8 | 1 | 1 | yes |
| 0x21 | AirconTempZone4 | Property | fixedByteSequence | 2 | 2 |  |
| 0x22 | AirconAirVolumeZone4 | Property | int8 | 1 | 1 |  |
| 0x23 | AirconAirVolumeRegZone4 | Property | int8 | 1 | 1 |  |
| 0x24 | AirconAirDistributionZone4 | Property | fixedByteSequence | 4 | 4 |  |
| 0x25 | AirconSeatHeaterZone4 | Property | int8 | 1 | 1 | yes |
| 0x26 | AirconSeatVentilationZone4 | Property | int8 | 1 | 1 | yes |
| 0x2C | Fct_2C | Property | int8 | 1 | 1 |  |
| 0x32 | Fct_32 | Property | int8 | 1 | 1 |  |
| 0x33 | Fct_33 | Property | int8 | 1 | 1 |  |
