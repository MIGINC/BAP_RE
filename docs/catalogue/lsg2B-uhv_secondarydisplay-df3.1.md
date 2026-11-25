# LSG 0x2B UHV_SecondaryDisplay, DF 3.1

Sizes are bytes; `var` = up to the size. **Car** = logged in car, sent by the module.

| FCT | Name | Class | Type | TX | RX | Car |
|---|---|---|---|---|---|---|
| 0x01 | GetAll | Cache | byteSequence | 44 var | 0 | yes |
| 0x02 | BAP_Config | Property | fixedByteSequence | 6 | 0 | yes |
| 0x03 | FunctionList | Property | byteSequence | 8 var | 0 | yes |
| 0x04 | HeartBeat | Property | int8 | 1 | 0 | yes |
| 0x0F | FSG_OperationState | Property | int8 | 1 | 0 | yes |
| 0x10 | ASG_Capabilities | Property | fixedByteSequence | 2 | 2 | yes |
| 0x11 | ASG_Config | Property | fixedByteSequence | 2 | 2 | yes |
| 0x12 | MFL_BlockDefinition | Property | byteSequence | 8 var | 8 | yes |
| 0x13 | FrameStatus | Property | fixedByteSequence | 6 | 6 | yes |
| 0x14 | FrameData | Array | byteSequence | 685 var | 4 | yes |
| 0x15 | FrameDataAck | Property | int8 | 1 | 1 | yes |
| 0x16 | ScrollBar | Property | fixedByteSequence | 4 | 0 | yes |
| 0x17 | LsgStatus | Property | int8 | 1 | 1 | yes |

## Layouts

### 0x14 FrameData

array, header `arrayheader`, up to 7 elements:
- prefix on get:
  - `FrameId`: u8
- prefix on status:
  - `FrameId`: u8
  - `ListElementOnTop`: u8
  - `ListOperation`: u8 (values 0x0 none, 0x1 list up, 0x2 list down, 0x3 page flip up, 0x4 page flip down, 0x5 fast scroll up, 0x6 fast scroll down, 0x7 fast page flip up, 0x8 fast page flip down, 0x9 top, 0xA bottom, 0xB menu change, 0xC enter menu, 0xD leave menu)
- element:
  - `Type`: u8 (ra_mask 2; values 0x0 string, 0x1 directory, 0x2 file name, 0x3 phone number, 0x4 menu item, 0x5 check box, 0x6 radio button, 0x7 speller, 0x8 sub-menu)
  - `Attributes`: u8 (ra_mask 4; bits 7 visible, 3 activated, 2 selected, 1 focused, 0 selectable)
  - `Name`: string (max 93; ra_mask 1)

## Records

- `arrayheader`:
  - `Mode`: nibble_hi (bits 0 Shift, 1 Backward, 2 PosTransmitted, 3 IndexSize16)
  - `RecordAddress`: nibble_lo
  - `Start`: index
  - `Elements`: index
