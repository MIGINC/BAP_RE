# LSG 0x0B RVC, DF 5.3

Sizes are bytes; `var` = up to the size.

An emulated MIB2 decoded 0x0D, 0x10–0x19, 0x1B–0x1D and 0x20–0x22 at this DF (tested with cluster). It didn't react to 0x1E and 0x1F.

| FCT | Name | Class | Type | TX | RX |
|---|---|---|---|---|---|
| 0x01 | GetAll | Cache | byteSequence | 67 var | 0 |
| 0x02 | BAP-Config | Property | fixedByteSequence | 6 | 0 |
| 0x03 | FunctionList | Property | byteSequence | 8 var | 0 |
| 0x04 | HeartBeat | Property | int8 | 1 | 0 |
| 0x0D | FactorySettings | Property | int8 | 1 | 1 |
| 0x0E | FSG-Setup | Property | fixedByteSequence | 6 | 0 |
| 0x0F | FSG-OperationState | Property | int8 | 1 | 0 |
| 0x10 | FollowUp_Time | Property | int8 | 1 | 1 |
| 0x11 | ScreenRV_ModeRV | Property | fixedByteSequence | 2 | 2 |
| 0x12 | ScreenFV_ModeFV | Property | fixedByteSequence | 2 | 2 |
| 0x13 | ScreenSV_ModeSV | Property | fixedByteSequence | 2 | 2 |
| 0x14 | ScreenBV_ModeBV | Property | fixedByteSequence | 2 | 2 |
| 0x15 | View | Property | int8 | 1 | 1 |
| 0x16 | Car_State | Property | int8 | 1 | 0 |
| 0x17 | Color | Property | fixedByteSequence | 2 | 2 |
| 0x18 | Contrast | Property | fixedByteSequence | 2 | 2 |
| 0x19 | Brightness | Property | fixedByteSequence | 2 | 2 |
| 0x1A | VPS_Display | Property | fixedByteSequence | 4 | 4 |
| 0x1B | DynamicMode | Property | fixedByteSequence | 2 | 2 |
| 0x1C | Overlay | Property | fixedByteSequence | 2 | 2 |
| 0x1D | CameraStates | Property | fixedByteSequence | 4 | 0 |
| 0x1E | 3D_BV_Config | Property | fixedByteSequence | 3 | 0 |
| 0x1F | 3D_BV | Property | fixedByteSequence | 4 | 4 |
| 0x20 | ExtCam_Cfg | Property | fixedByteSequence | 2 | 2 |
| 0x21 | ExtCam_ManAct | Property | fixedByteSequence | 2 | 2 |
| 0x22 | SysState | Property | fixedByteSequence | 2 | 2 |
