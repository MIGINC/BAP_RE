# LSG 0x0B RVC, DF 5.1

Sizes are bytes; `var` = up to the size. **Car** = logged in car, sent by the module.

An emulated MIB2 decoded 0x0D, 0x10–0x19 and 0x1B–0x1D at this DF (tested with cluster). The camera in my car doesn't list 0x12–0x15 and 0x1B–0x1D, so there my own FSG announced them.

| FCT | Name | Class | Type | TX | RX | Car |
|---|---|---|---|---|---|---|
| 0x01 | GetAll | Cache | byteSequence | 54 var | 0 | yes |
| 0x02 | BAP-Config | Property | fixedByteSequence | 6 | 0 | yes |
| 0x03 | FunctionList | Property | byteSequence | 8 var | 0 | yes |
| 0x04 | HeartBeat | Property | int8 | 1 | 0 | yes |
| 0x0D | FactorySettings | Property | int8 | 1 | 1 | yes |
| 0x0E | FSG-Setup | Property | fixedByteSequence | 6 | 0 | yes |
| 0x0F | FSG-OperationState | Property | int8 | 1 | 0 | yes |
| 0x10 | FollowUp_Time | Property | int8 | 1 | 1 | yes |
| 0x11 | ScreenRV_ModeRV | Property | fixedByteSequence | 2 | 2 | yes |
| 0x12 | ScreenFV_ModeFV | Property | fixedByteSequence | 2 | 2 |  |
| 0x13 | ScreenSV_ModeSV | Property | fixedByteSequence | 2 | 2 |  |
| 0x14 | ScreenBV_ModeBV | Property | fixedByteSequence | 2 | 2 |  |
| 0x15 | View | Property | int8 | 1 | 1 |  |
| 0x16 | Car_State | Property | int8 | 1 | 0 | yes |
| 0x17 | Color | Property | fixedByteSequence | 2 | 2 | yes |
| 0x18 | Contrast | Property | fixedByteSequence | 2 | 2 | yes |
| 0x19 | Brightness | Property | fixedByteSequence | 2 | 2 | yes |
| 0x1A | VPS_Display | Property | fixedByteSequence | 4 | 4 | yes |
| 0x1B | DynamicMode | Property | fixedByteSequence | 2 | 2 |  |
| 0x1C | Overlay | Property | fixedByteSequence | 2 | 2 |  |
| 0x1D | CameraStates | Property | fixedByteSequence | 4 | 0 |  |
