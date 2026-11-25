# LSG 0x0D Doorlocking, DF 3.3

Sizes are bytes; `var` = up to the size. **Car** = logged in car, sent by the module. **Sim** = simulated at the application layer: a MIB2 decoded the value, but it skipped the BAP protocol.

| FCT | Name | Class | Type | TX | RX | Car | Sim |
|---|---|---|---|---|---|---|---|
| 0x01 | GetAll | Cache | byteSequence | 39 var | 0 | yes |  |
| 0x02 | BAP-Config | Property | fixedByteSequence | 6 | 0 | yes |  |
| 0x03 | FunctionList | Property | fixedByteSequence | 8 | 0 | yes |  |
| 0x04 | HeartBeat | Property | int8 | 1 | 0 | yes |  |
| 0x0D | DoorLockingSetFactoryDefault | Property | int8 | 1 | 1 | yes |  |
| 0x0E | FSG-Setup | Property | fixedByteSequence | 3 | 0 | yes |  |
| 0x0F | FSG-OperationState | Property | int8 | 1 | 0 | yes |  |
| 0x10 | ComfortOpen | Property | int8 | 1 | 1 |  |  |
| 0x11 | AutoLock | Property | int8 | 1 | 1 | yes |  |
| 0x13 | TheftProtectionSensorSetup | Property | int8 | 1 | 1 | yes |  |
| 0x15 | RearBlind | Property | int8 | 1 | 1 |  | yes |
| 0x18 | AutoUnLock | Property | fixedByteSequence | 1 | 1 | yes |  |
| 0x1A | DoorUnlockMode | Property | int8 | 1 | 1 | yes |  |
| 0x1B | WindowOperation | Property | int8 | 1 | 1 | yes |  |
| 0x1C | DoorLockingWindowStatus | Property | fixedByteSequence | 6 | 0 | yes |  |
| 0x1D | BootOpen | Property | fixedByteSequence | 2 | 0 |  | yes |
| 0x1E | BootClose | Property | fixedByteSequence | 3 | 0 |  | yes |
| 0x1F | DoorLockingRemoteLockUnlock | Property | int8 | 1 | 1 | yes |  |
