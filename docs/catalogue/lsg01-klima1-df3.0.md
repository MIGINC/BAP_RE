# LSG 0x01 Klima1, DF 3.0

Sizes are bytes; `var` = up to the size.

| FCT | Name | Class | Type | TX | RX |
|---|---|---|---|---|---|
| 0x01 | GetAll | Cache | byteSequence | 75 var | 0 |
| 0x02 | BAP-Config | Property | fixedByteSequence | 6 | 0 |
| 0x03 | FunctionList | Property | byteSequence | 8 var | 0 |
| 0x04 | HeartBeat | Property | int8 | 1 | 0 |
| 0x0D | AirconSystemOnOffRow1 | Property | int8 | 1 | 1 |
| 0x0E | FSG-Setup | Property | fixedByteSequence | 6 | 0 |
| 0x0F | FSG-OperationState | Property | int8 | 1 | 0 |
| 0x10 | AirconAirCirculationMan | Property | int8 | 1 | 1 |
| 0x11 | AirconAirCirculationAuto | Property | int8 | 1 | 1 |
| 0x12 | Fct_12 | Property | int8 | 1 | 1 |
| 0x13 | AirconSolar | Property | int8 | 1 | 1 |
| 0x14 | AirconAirCirculationMiddleExhaustion | Property | int8 | 1 | 1 |
| 0x15 | AirconSynchronisation | Property | fixedByteSequence | 2 | 2 |
| 0x16 | AirconPopupRequest | Property | fixedByteSequence | 4 | 0 |
| 0x17 | AirconPopupState | Property | fixedByteSequence | 2 | 2 |
| 0x18 | AirconRearWindowHeater | Property | int8 | 1 | 1 |
| 0x19 | AirconIndirectVentilation | Property | int8 | 1 | 1 |
| 0x1A | AirconPopupTime | Property | int8 | 1 | 1 |
| 0x1B | AirconTempZone1 | Property | fixedByteSequence | 2 | 2 |
| 0x1C | AirconAirVolumeZone1 | Property | int8 | 1 | 1 |
| 0x1D | AirconAirVolumeZone1 | Property | int8 | 1 | 1 |
| 0x1E | AirconAirDistributionZone1_AirconAirVolumeZone1 | Property | fixedByteSequence | 4 | 4 |
| 0x1F | AirconSeatHeaterZone1 | Property | int8 | 1 | 1 |
| 0x20 | AirconSeatVentilationZone1 | Property | int8 | 1 | 1 |
| 0x21 | AirconTempZone2 | Property | fixedByteSequence | 2 | 2 |
| 0x22 | AirconAirVolumeZone2 | Property | int8 | 1 | 1 |
| 0x23 | AirconAirVolumeZone2 | Property | int8 | 1 | 1 |
| 0x24 | AirconAirDistributionZone2_AirconAirVolumeZone2 | Property | fixedByteSequence | 4 | 4 |
| 0x25 | AirconSeatHeaterZone2 | Property | int8 | 1 | 1 |
| 0x26 | AirconSeatVentilationZone2 | Property | int8 | 1 | 1 |
| 0x27 | AirconRearControl | Property | int8 | 1 | 1 |
| 0x28 | AirconAirCirculationSensitivity | Property | int8 | 1 | 1 |
| 0x29 | AirconResidualHeat | Property | int8 | 1 | 1 |
| 0x2A | AirconHeater | Property | fixedByteSequence | 2 | 2 |
| 0x2B | Fct_2B | Property | fixedByteSequence | 2 | 2 |
| 0x2C | AirconRearAuxHeater | Property | int8 | 1 | 1 |
| 0x2D | AirconFrontWindowHeater | Property | fixedByteSequence | 2 | 2 |
| 0x2E | AirconDefrost | Property | int8 | 1 | 1 |
| 0x2F | AirconSuppressVisualisation | Property | int8 | 1 | 1 |
| 0x30 | AirconBlowerCompensation | Property | int8 | 1 | 1 |
| 0x31 | AirconAC | Property | int8 | 1 | 1 |
