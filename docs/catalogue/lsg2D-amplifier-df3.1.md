# LSG 0x2D Amplifier, DF 3.1

Sizes are bytes; `var` = up to the size.

| FCT | Name | Class | Type | TX | RX |
|---|---|---|---|---|---|
| 0x01 | GetAll | Cache | byteSequence | 26 var | 0 |
| 0x02 | BAP-Config | Property | fixedByteSequence | 6 | 0 |
| 0x03 | FunctionList | Property | byteSequence | 8 var | 0 |
| 0x04 | HeartBeat | Property | int8 | 1 | 0 |
| 0x0D | SoundOn | Property | int8 | 1 | 1 |
| 0x0E | FSG-Setup | Property | fixedByteSequence | 2 | 0 |
| 0x0F | FSG-OperationState | Property | int8 | 1 | 0 |
| 0x10 | PowerAmps | Method | int8 | 1 | 1 |
| 0x11 | Sources | Array | byteSequence | 11 var | 11 |
| 0x12 | SrcVolumeOffset | Array | byteSequence | 29 var | 29 |
| 0x13 | GALA | Property | int8 | 1 | 1 |
| 0x14 | SrcGALA | Array | byteSequence | 29 var | 29 |
| 0x15 | Volume | Method | int8 | 1 | 7 |
| 0x16 | VolumeNow | Property | fixedByteSequence | 4 | 0 |
| 0x17 | Balance | Array | byteSequence | 29 var | 29 |
| 0x18 | Fader | Array | byteSequence | 29 var | 29 |
| 0x19 | Bass | Array | byteSequence | 29 var | 29 |
| 0x1A | Middle | Array | byteSequence | 29 var | 29 |
| 0x1B | Treble | Array | byteSequence | 29 var | 29 |
| 0x1C | InputSetting_Loudness | Array | byteSequence | 11 var | 11 |
| 0x1D | Subwoofer | Array | byteSequence | 11 var | 11 |
| 0x1E | Preset_RoomMode | Array | byteSequence | 11 var | 11 |
| 0x1F | Fct_1F | Array | byteSequence | 11 var | 11 |
| 0x20 | Fct_20 | Array | byteSequence | 11 var | 11 |
| 0x21 | Fct_21 | Array | byteSequence | 11 var | 11 |
| 0x22 | Equalizer5Band | Array | byteSequence | 55 var | 55 |
| 0x23 | Fct_23 | Array | byteSequence | 11 var | 11 |
| 0x24 | Fct_24 | Array | byteSequence | 17 var | 17 |
| 0x25 | MemorySave | Method | int8 | 1 | 1 |
| 0x26 | MemoryRecall | Method | int8 | 1 | 1 |
| 0x27 | Fct_27 | Method | int8 | 1 | 9 |
