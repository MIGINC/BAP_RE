# LSG 0x1D LSG_1D, DF 3.3

Sizes are bytes; `var` = up to the size. **Sim** = simulated at the application layer: a MIB2 decoded the value, but it skipped the BAP protocol.

| FCT | Name | Class | Type | TX | RX | Sim |
|---|---|---|---|---|---|---|
| 0x01 | GetAll | Cache | byteSequence | 26 var | 0 |  |
| 0x02 | BAP-Config | Property | fixedByteSequence | 6 | 0 |  |
| 0x03 | FunctionList | Property | byteSequence | 8 var | 0 |  |
| 0x04 | HeartBeat | Property | int8 | 1 | 0 |  |
| 0x0D | Fct_0D | Property | int8 | 1 | 0 |  |
| 0x0E | FSG-Setup | Property | int8 | 1 | 0 |  |
| 0x0F | FSG-OperationState | Property | int8 | 1 | 0 | yes |
| 0x10 | HybridEnergyFlowState | Property | int8 | 1 | 0 | yes |
| 0x11 | HybridCharge | Property | int8 | 1 | 0 | yes |
| 0x12 | Fct_12 | Property | int8 | 1 | 0 |  |
| 0x13 | HybridRecoveredEnergy | Property | int8 | 1 | 0 | yes |
| 0x14 | Fct_14 | Property | fixedByteSequence | 2 | 0 |  |
