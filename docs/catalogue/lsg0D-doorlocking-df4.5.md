# LSG 0x0D Doorlocking, DF 4.5

Sizes are bytes; `var` = up to the size. **Sim** = simulated at the application layer: a MIB2 decoded the value, but it skipped the BAP protocol.

| FCT | Name | Class | Type | TX | RX | Sim |
|---|---|---|---|---|---|---|
| 0x01 | GetAll | Cache | byteSequence | 149 var | 0 |  |
| 0x02 | BAP-Config | Property | fixedByteSequence | 6 | 0 |  |
| 0x03 | FunctionList | Property | byteSequence | 8 var | 0 |  |
| 0x04 | HeartBeat | Property | int8 | 1 | 0 |  |
| 0x0D | DoorLockingSetFactoryDefault | Property | int8 | 1 | 1 |  |
| 0x0E | FSG-Setup | Property | fixedByteSequence | 3 | 0 | yes |
| 0x0F | FSG-OperationState | Property | int8 | 1 | 0 |  |
| 0x10 | ComfortOpen | Property | fixedByteSequence | 2 | 2 | yes |
| 0x11 | CentralLocking | Property | fixedByteSequence | 2 | 2 | yes |
| 0x12 | RainClosing | Property | int8 | 1 | 1 | yes |
| 0x13 | TheftProtectionSensorSetup | Property | fixedByteSequence | 2 | 2 | yes |
| 0x14 | AcknowledgeHonk | Property | int8 | 1 | 1 | yes |
| 0x15 | RearBlind | Property | int8 | 1 | 1 | yes |
| 0x16 | TurnIndicatorRepeat | Property | int8 | 1 | 1 | yes |
| 0x17 | AutoLock | Property | fixedByteSequence | 2 | 2 | yes |
| 0x18 | AutoUnLock | Property | fixedByteSequence | 1 | 1 | yes |
| 0x19 | MirrorFoldIn | Property | int8 | 1 | 1 | yes |
| 0x1A | DoorLockingMessage | Property | fixedByteSequence | 6 | 0 | yes |
| 0x1B | DoorLockingLockStatus | Property | fixedByteSequence | 2 | 0 | yes |
| 0x1C | DoorLockingWindowStatus | Property | fixedByteSequence | 3 | 0 |  |
| 0x1D | BootOpen | Property | int8 | 1 | 1 | yes |
| 0x1E | BootClose | Property | int8 | 1 | 1 | yes |
| 0x1F | DoorLockingRemoteLockUnlock | Method | byteSequence | 11 var | 6 | yes |
| 0x20 | DoorLockingRemoteLockUnlockAuthentification | Property | byteSequence | 42 var | 0 | yes |
| 0x21 | DoorLockingRemoteLockUnlockSignature | Property | byteSequence | 36 var | 36 | yes |
| 0x22 | DoorLockingRemoteHorn | Property | fixedByteSequence | 3 | 3 | yes |
| 0x23 | DoorLockingRemoteBlinking | Property | fixedByteSequence | 3 | 3 | yes |
| 0x24 | UserProfileList | Array | byteSequence | 289 var | 75 |  |
| 0x25 | DoorLockingActiveUser | Property | int8 | 1 | 1 | yes |
| 0x26 | DoorLockingUserProfileOnOff | Property | fixedByteSequence | 2 | 2 | yes |
| 0x27 | DoorLockingUserProfileControl | Method | fixedByteSequence | 4 | 4 | yes |
| 0x28 | DoorLockingViewOptions | Property | fixedByteSequence | 6 | 0 | yes |
| 0x29 | DoorLockingWindowAutoClose | Property | int8 | 1 | 1 | yes |
| 0x2A | DoorlockingBlindsControl | Property | fixedByteSequence | 2 | 2 |  |
