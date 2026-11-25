# LSG 0x01 Klima1, DF 3.3

Sizes are bytes; `var` = up to the size. **Car** = logged in car, sent by the module.

| FCT | Name | Class | Type | TX | RX | Car |
|---|---|---|---|---|---|---|
| 0x01 | GetAll | Cache | byteSequence | 79 var | 0 | yes |
| 0x02 | BAP-Config | Property | fixedByteSequence | 6 | 0 | yes |
| 0x03 | FunctionList | Property | byteSequence | 8 var | 0 | yes |
| 0x04 | HeartBeat | Property | int8 | 1 | 0 | yes |
| 0x0D | AirconSystemOnOffRow1 | Property | int8 | 1 | 1 | yes |
| 0x0E | FSG-Setup | Property | fixedByteSequence | 6 | 0 | yes |
| 0x0F | FSG-OperationState | Property | int8 | 1 | 0 | yes |
| 0x10 | AirconAirCirculationMan | Property | int8 | 1 | 1 |  |
| 0x11 | AirconAirCirculationAuto | Property | int8 | 1 | 1 | yes |
| 0x12 | Fct_12 | Property | int8 | 1 | 1 |  |
| 0x13 | AirconSolar | Property | int8 | 1 | 1 |  |
| 0x14 | AirconAirCirculationMiddleExhaustion | Property | int8 | 1 | 1 |  |
| 0x15 | AirconSynchronisation | Property | fixedByteSequence | 2 | 2 |  |
| 0x16 | AirconPopupRequest | Property | fixedByteSequence | 4 | 0 | yes |
| 0x17 | AirconPopupState | Property | fixedByteSequence | 2 | 2 | yes |
| 0x18 | AirconRearWindowHeater | Property | int8 | 1 | 1 | yes |
| 0x19 | AirconIndirectVentilation | Property | int8 | 1 | 1 |  |
| 0x1A | AirconPopupTime | Property | int8 | 1 | 1 | yes |
| 0x1B | AirconTempZone1 | Property | fixedByteSequence | 2 | 2 | yes |
| 0x1C | AirconAirVolumeZone1 | Property | int8 | 1 | 1 | yes |
| 0x1D | AirconAirVolumeRegZone1 | Property | int8 | 1 | 1 | yes |
| 0x1E | AirconAirDistributionZone1 | Property | fixedByteSequence | 4 | 4 | yes |
| 0x1F | AirconSeatHeaterZone1 | Property | int8 | 1 | 1 | yes |
| 0x20 | AirconSeatVentilationZone1 | Property | int8 | 1 | 1 |  |
| 0x21 | AirconTempZone2 | Property | fixedByteSequence | 2 | 2 | yes |
| 0x22 | AirconAirVolumeZone2 | Property | int8 | 1 | 1 | yes |
| 0x23 | AirconAirVolumeRegZone2 | Property | int8 | 1 | 1 | yes |
| 0x24 | AirconAirDistributionZone2 | Property | fixedByteSequence | 4 | 4 | yes |
| 0x25 | AirconSeatHeaterZone2 | Property | int8 | 1 | 1 | yes |
| 0x26 | AirconSeatVentilationZone2 | Property | int8 | 1 | 1 |  |
| 0x27 | AirconRearControl | Property | int8 | 1 | 1 | yes |
| 0x28 | AirconAirCirculationSensitivity | Property | int8 | 1 | 1 |  |
| 0x29 | AirconResidualHeat | Property | int8 | 1 | 1 |  |
| 0x2A | AirconHeater | Property | fixedByteSequence | 2 | 2 |  |
| 0x2B | Fct_2B | Property | fixedByteSequence | 2 | 2 |  |
| 0x2C | AirconRearAuxHeater | Property | int8 | 1 | 1 |  |
| 0x2D | AirconFrontWindowHeater | Property | fixedByteSequence | 2 | 2 |  |
| 0x2E | AirconDefrost | Property | int8 | 1 | 1 | yes |
| 0x2F | AirconSuppressVisualisation | Property | int8 | 1 | 1 | yes |
| 0x30 | AirconBlowerCompensation | Property | int8 | 1 | 1 |  |
| 0x31 | AirconAC | Property | int8 | 1 | 1 | yes |
| 0x32 | Fct_32 | Property | int8 | 1 | 1 |  |
| 0x33 | Fct_33 | Property | int8 | 1 | 1 |  |
| 0x34 | Fct_34 | Property | int8 | 1 | 1 |  |
| 0x35 | Fct_35 | Property | int8 | 1 | 1 | yes |
