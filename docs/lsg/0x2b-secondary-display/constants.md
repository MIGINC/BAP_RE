# Secondary Display Constants and Definitions

The values used on LSG 0x2B. Visible text, frame IDs and the frame handshake are tested with cluster. The rest are untested unless it says otherwise. For what each function carries, see [Secondary Display Overview](README.md).

## Frame IDs

```plaintext
invalid                               = 0x00
1 to 7                                = 0x01 … 0x07
any frame                             = 0xFE
not available                         = 0xFF
```

`0xFE` isn't allowed as the focused frame ID in FrameStatus.

## FrameStatus

**Relative priority** (byte 1):
```plaintext
low                                   = 0x00
high                                  = 0x01
```

**ASG response** (byte 4, from the cluster):
```plaintext
made invisible                        = 0x00      cluster set the frame invisible
made visible                          = 0x01      cluster set the frame visible
queued up                             = 0x02      request queued
request visible                       = 0x03      cluster asks for the frame to be visible
no response                           = 0xFF
```

**FSG request** (byte 5, from the phone):
```plaintext
request invisible                     = 0x00
request visible                       = 0x01
accept ASG request                    = 0x02
refuse ASG request                    = 0x03
request invisible menu quit short     = 0x04
request invisible menu quit long      = 0x05
no request                            = 0xFF      the only value allowed when the cluster writes
```

## ASG_Capabilities and ASG_Config

```plaintext
small                                 = 0x00
medium                                = 0x01
large                                 = 0x02      the phone treats this as medium

version 1.0                           = display protocol 1.0      sent as [01][00]
version 1.1                           = display protocol 1.1      sent as [01][01]
```

## MFL key blocks

```plaintext
no keys                               = 0x00
all shared MFL keys                   = 0x01
up down                               = 0x02
left right                            = 0x03
up down left right ok                 = 0x04
rotary 1                              = 0x05
rotary 2                              = 0x06
ok                                    = 0x07
back                                  = 0x08
```

## FrameData cells

**Cell type**:
```plaintext
any string                            = 0x00
directory                             = 0x01
file name                             = 0x02
phone number                          = 0x03
menu item                             = 0x04
check box                             = 0x05
radio button                          = 0x06
speller                               = 0x07
sub menu                              = 0x08
```

**Cell attributes**, bit flags. `0x80` visible is tested with cluster. `0x00`, `0x80`, `0x81` and `0x82` are logged in car, the rest are untested:
```plaintext
visible                               = 0x80
activated                             = 0x08      checkbox / radio selected
selected                              = 0x04      OK pressed on it
focused                               = 0x02      highlighted
selectable                            = 0x01
```

So a visible, selectable menu item is `0x81`, and a focused, ticked checkbox is `0x8A`.

**recordAddress** (low nibble of the array header's mode byte):
```plaintext
name                                  = 0x01
type                                  = 0x02
attr                                  = 0x04
```

The rest of the mode byte is on the [Arrays](../../protocol/arrays.md) page.

## List operations

Sent in FrameData's "list operation" byte to animate the list:

```plaintext
no list operation                     = 0x00
list up                               = 0x01
list down                             = 0x02
page flip up                          = 0x03
page flip down                        = 0x04
fast scrolling up                     = 0x05
fast scrolling down                   = 0x06
fast page flip up                     = 0x07
fast page flip down                   = 0x08
moving to top of list                 = 0x09
moving to bottom of list              = 0x0A
menu change                           = 0x0B      v1.10+
enter menu                            = 0x0C      v1.11
leave menu                            = 0x0D      v1.11
```

The car log has `0x00`, `0x01` and `0x02`. The rest are untested.

## ScrollBar attributes (untested)

```plaintext
show arrows                           = 0x01
endless list                          = 0x02
is popup                              = 0x04
```

## Limits

Untested, except the 7 (logged in car, `47 00 07`):

```plaintext
12   cells per frame
7    cells per FrameData message, the most I've seen
8    key blocks in MFL_BlockDefinition
32   characters in a phone number cell
93   bytes in a cell's name
```

Related: [Secondary Display Overview](README.md) · [Icon Mappings](icons.md) · [Arrays](../../protocol/arrays.md)
