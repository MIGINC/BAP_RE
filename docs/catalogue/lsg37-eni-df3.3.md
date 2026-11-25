# LSG 0x37 ENI, DF 3.3

Sizes are bytes; `var` = up to the size.

| FCT | Name | Class | Type | TX | RX |
|---|---|---|---|---|---|
| 0x01 | GetAll | Cache | byteSequence | 38 var | 0 |
| 0x02 | BAP-Config | Property | fixedByteSequence | 6 | 0 |
| 0x03 | FunctionList | Property | byteSequence | 8 var | 0 |
| 0x04 | HeartBeat | Property | int8 | 1 | 0 |
| 0x0D | FSG_Control | Property | int8 | 1 | 1 |
| 0x0E | FSG-Setup | Property | int8 | 1 | 0 |
| 0x0F | FSG-OperationState | Property | int8 | 1 | 0 |
| 0x10 | DestinationsList | Array | byteSequence | 589 var | 587 |
| 0x11 | DestinationListASGCapacity | Property | int16 | 2 | 2 |
| 0x12 | TriggerRemoteProcess | Method | int8 | 1 | 257 |
| 0x13 | RemoteProcessCommands | Property | fixedByteSequence | 6 | 0 |
| 0x14 | RemoteProcessState | Property | fixedByteSequence | 4 | 0 |
| 0x15 | UserList | Array | byteSequence | 740 var | 4 |
| 0x16 | ServiceList | Array | byteSequence | 173 var | 172 |
| 0x17 | ActiveMonitorings | Property | int32 | 4 | 0 |
| 0x18 | PrivacySetup | Property | fixedByteSequence | 2 | 2 |
