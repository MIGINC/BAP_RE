# LSG 0x33 eCall, DF 3.5

Sizes are bytes; `var` = up to the size. **Sim** = simulated at the application layer: a MIB2 decoded the value, but it skipped the BAP protocol.

The two marked Sim only count as StatusAck, see [MIB2 Results](../protocol/mib2.md#statusack). **0x10** byte 1 asks for one of four audio paths (1–4; 0 and 5 ask for none). Once it's up the MIB2 writes the number back in byte 0 (`04 00`), and `05 00` after the release. **0x11** byte 0 is state (high nibble) and call type (low nibble). With state 1, types 1, 4, 6, 7 and 9 opened audio path 4 and type 8 path 3.

| FCT | Name | Class | Type | TX | RX | Sim |
|---|---|---|---|---|---|---|
| 0x01 | GetAll | Cache | byteSequence | 146 var | 0 |  |
| 0x02 | BAP-Config | Property | fixedByteSequence | 6 | 0 |  |
| 0x03 | FunctionList | Property | byteSequence | 8 var | 0 |  |
| 0x04 | HeartBeat | Property | int8 | 1 | 0 |  |
| 0x0D | FSG_Control | Property | int8 | 1 | 1 |  |
| 0x0E | FSG-Setup | Property | fixedByteSequence | 3 | 0 |  |
| 0x0F | FSG-OperationState | Property | int8 | 1 | 0 |  |
| 0x10 | AudioState | Property | fixedByteSequence | 2 | 2 | yes |
| 0x11 | CallState | Property | byteSequence | 8 var | 8 | yes |
| 0x12 | HangupCall | Method | int8 | 1 | 1 |  |
| 0x13 | AcceptCall | Method | int8 | 1 | 0 |  |
| 0x14 | DisconnectReason | Property | int8 | 1 | 0 |  |
| 0x15 | RegisterState | Property | fixedByteSequence | 4 | 0 |  |
| 0x16 | NetworkProvider | Property | byteSequence | 82 var | 0 |  |
| 0x17 | SignalQuality | Property | int8 | 1 | 0 |  |
| 0x18 | ServiceRequest | Property | int32 | 4 | 0 |  |
| 0x19 | ServiceControl | Method | int8 | 1 | 2 |  |
| 0x1A | ServiceState | Property | fixedByteSequence | 6 | 0 |  |
| 0x1B | SupportedServices | Property | fixedByteSequence | 6 | 0 |  |
| 0x1C | FunctionalRestrictions | Property | fixedByteSequence | 6 | 0 |  |
