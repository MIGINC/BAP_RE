# LSG 0x08 Innenlicht, DF 5.1

Sizes are bytes; `var` = up to the size. **Sim** = simulated at the application layer: a MIB2 decoded the value, but it skipped the BAP protocol.

| FCT | Name | Class | Type | TX | RX | Sim |
|---|---|---|---|---|---|---|
| 0x01 | GetAll | Cache | byteSequence | 135 var | 0 |  |
| 0x02 | BAP-Config | Property | fixedByteSequence | 6 | 0 |  |
| 0x03 | FunctionList | Property | byteSequence | 8 var | 0 |  |
| 0x04 | HeartBeat | Property | int8 | 1 | 0 |  |
| 0x0D | SetFactoryDefault | Property | int8 | 1 | 1 |  |
| 0x0E | FSG-Setup | Property | byteSequence | 41 var | 0 |  |
| 0x0F | FSG-OperationState | Property | int8 | 1 | 0 |  |
| 0x10 | Colour | Property | fixedByteSequence | 2 | 2 | yes |
| 0x11 | AmbianceLighting | Property | fixedByteSequence | 2 | 2 | yes |
| 0x12 | EnvironmentDependence | Property | fixedByteSequence | 2 | 2 | yes |
| 0x13 | TemperatureDependence | Property | fixedByteSequence | 2 | 2 | yes |
| 0x14 | SpeedDependence | Property | fixedByteSequence | 2 | 2 | yes |
| 0x15 | Brightness | Property | fixedByteSequence | 3 | 3 | yes |
| 0x16 | IlluminationSet1 | Property | fixedByteSequence | 2 | 2 | yes |
| 0x17 | IlluminationSet2 | Property | fixedByteSequence | 2 | 2 | yes |
| 0x18 | IlluminationSet3 | Property | fixedByteSequence | 2 | 2 | yes |
| 0x19 | IlluminationSet4 | Property | fixedByteSequence | 2 | 2 | yes |
| 0x1A | IlluminationSet5 | Property | fixedByteSequence | 2 | 2 | yes |
| 0x1B | IlluminationSet6 | Property | fixedByteSequence | 2 | 2 | yes |
| 0x1C | IlluminationSet7 | Property | fixedByteSequence | 2 | 2 | yes |
| 0x1D | IlluminationSet8 | Property | fixedByteSequence | 2 | 2 | yes |
| 0x1E | IntLightConfig | Property | byteSequence | 14 var | 0 |  |
| 0x1F | ActiveProfile | Property | fixedByteSequence | 2 | 2 | yes |
| 0x20 | AmbientLightColor | Property | fixedByteSequence | 4 | 4 | yes |
| 0x21 | ContourLightColor | Property | fixedByteSequence | 4 | 4 | yes |
| 0x22 | IlluminationProfile1 | Property | fixedByteSequence | 2 | 2 | yes |
| 0x23 | IlluminationProfile2 | Property | fixedByteSequence | 2 | 2 | yes |
| 0x24 | IlluminationProfile3 | Property | fixedByteSequence | 2 | 2 | yes |
| 0x25 | IlluminationProfile4 | Property | fixedByteSequence | 2 | 2 | yes |
| 0x26 | IlluminationProfile5 | Property | fixedByteSequence | 2 | 2 | yes |
| 0x27 | IlluminationProfile6 | Property | fixedByteSequence | 2 | 2 | yes |
| 0x28 | IlluminationProfile7 | Property | fixedByteSequence | 2 | 2 | yes |
| 0x29 | IlluminationProfile8 | Property | fixedByteSequence | 2 | 2 | yes |
| 0x2A | RGBColorList | Array | byteSequence | 45 var | 44 | yes |
| 0x2B | FollowUpTime | Property | fixedByteSequence | 2 | 2 | yes |
