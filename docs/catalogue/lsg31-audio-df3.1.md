# LSG 0x31 Audio, DF 3.1

Sizes are bytes; `var` = up to the size. **Car** = logged in car, sent by the module.

| FCT | Name | Class | Type | TX | RX | Car |
|---|---|---|---|---|---|---|
| 0x01 | GetAll | Cache | byteSequence | 304 var | 0 | yes |
| 0x02 | BAP-Config | Property | fixedByteSequence | 6 | 0 | yes |
| 0x03 | FunctionList | Property | byteSequence | 8 var | 0 | yes |
| 0x04 | HeartBeat | Property | int8 | 1 | 0 | yes |
| 0x0E | FSG-Setup | Property | int8 | 1 | 0 | yes |
| 0x0F | FSG-OperationState | Property | int8 | 1 | 0 | yes |
| 0x10 | Source | Property | fixedByteSequence | 4 | 0 | yes |
| 0x11 | SourceText | Property | byteSequence | 49 var | 0 | yes |
| 0x12 | Volume | Property | fixedByteSequence | 2 | 0 | yes |
| 0x13 | Mute | Property | int8 | 1 | 1 | yes |
| 0x14 | SourceState | Property | int8 | 1 | 0 | yes |
| 0x15 | NowPlaying | Property | byteSequence | 150 var | 0 | yes |
| 0x16 | List | Array | byteSequence | 319 var | 5 | yes |
| 0x17 | DedicatedAudioControl | Method | int8 | 1 | 3 | yes |
| 0x18 | GeneralInfoSwitches | Property | int8 | 1 | 1 | yes |
| 0x19 | TP_Info | Property | byteSequence | 35 var | 0 |  |
| 0x1A | Announcement | Property | byteSequence | 32 var | 0 | yes |
| 0x1B | AnnouncementOk | Method | int8 | 1 | 0 |  |
| 0x1C | StatusMessage | Property | int8 | 1 | 0 | yes |
| 0x1D | InfoListType | Property | int8 | 1 | 0 | yes |
| 0x1F | ChangeInfoListType | Method | int8 | 1 | 1 |  |
| 0x20 | SaveStation | Method | int8 | 1 | 4 |  |
| 0x21 | ChangeTunerVariant | Method | int8 | 1 | 1 |  |

## Layouts

### 0x16 List

array, header `arrayheader`, up to 6 elements:
- prefix on status:
  - `Total`: u16
- element:
  - `Icon`: u8 (ra_in [0, 2])
  - `Text`: string (max 48; ra_in [0, 1])

## Records

- `arrayheader`:
  - `Mode`: nibble_hi (bits 0 Shift, 1 Backward, 2 PosTransmitted, 3 IndexSize16)
  - `RecordAddress`: nibble_lo
  - `Start`: index
  - `Elements`: index
