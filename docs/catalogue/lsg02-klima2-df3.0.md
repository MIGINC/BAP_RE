# LSG 0x02 Klima2, DF 3.0

Sizes are bytes; `var` = up to the size.

| FCT | Name | Class | Type | TX | RX |
|---|---|---|---|---|---|
| 0x01 | GetAll | Cache | byteSequence | 37 var | 0 |
| 0x02 | BAP-Config | Property | fixedByteSequence | 6 | 0 |
| 0x03 | FunctionList | Property | byteSequence | 8 var | 0 |
| 0x04 | HeartBeat | Property | int8 | 1 | 0 |
| 0x1B | AirconTempZone3 | Property | fixedByteSequence | 2 | 2 |
| 0x1C | AirconAirVolumeZone3 | Property | int8 | 1 | 1 |
| 0x1D | AirconAirVolumeZone3 | Property | int8 | 1 | 1 |
| 0x1E | AirconAirDistributionZone3_AirconAirVolumeZone3 | Property | fixedByteSequence | 4 | 4 |
| 0x1F | AirconSeatHeaterZone3 | Property | int8 | 1 | 1 |
| 0x20 | AirconSeatVentilationZone3 | Property | int8 | 1 | 1 |
| 0x21 | AirconTempZone4 | Property | fixedByteSequence | 2 | 2 |
| 0x22 | AirconAirVolumeZone4 | Property | int8 | 1 | 1 |
| 0x23 | AirconAirVolumeRegZone4 | Property | int8 | 1 | 1 |
| 0x24 | AirconAirDistributionZone4 | Property | fixedByteSequence | 4 | 4 |
| 0x25 | AirconSeatHeaterZone4 | Property | int8 | 1 | 1 |
| 0x26 | AirconSeatVentilationZone4 | Property | int8 | 1 | 1 |
