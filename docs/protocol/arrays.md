# Arrays

Arrays are lists the ASG pages through a few items at a time: call lists, the phonebook, the lines on a cluster menu page.

- The ASG asks for a window (**GetArray**).
- The FSG answers with that window (**StatusArray**).
- The FSG can also push an update unasked (**ChangedArray**).

## The array header

Every array request and answer carries a small header:

```plaintext
indexSize = 0 (3 bytes):   [ mode ][ start ][ elements ]
indexSize = 1 (5 bytes):   [ mode ][ start lo ][ start hi ][ elements lo ][ elements hi ]
```

16-bit values are **little-endian**, as everywhere else in BAP.

The mode byte is four flags and a 4-bit *record address*:

```plaintext
bit   7           6             5           4       3 2 1 0
    indexSize | transmitPos | direction | shift | recordAddress
```

| Field | Meaning |
|---|---|
| **indexSize** | 0 = start/elements/positions are 8-bit; 1 = 16-bit. Senders switch to 16-bit once start or elements goes above 127 |
| **transmitPos** | in answers: each record is prefixed by its position in the list |
| **direction** | 0 = forward from start; 1 = backward (the ASG scrolling up) |
| **shift** | start from the *neighbour* of `start`, not `start` itself |
| **recordAddress** | which fields of each record to send. Defined per function, not by the protocol |

`shift` and `direction` together with `start = 0` mean "give me the last N entries". That's how you jump to the end of a list.

Positions on the wire are **0-based** and the answer always comes back in ascending order, even for a backward window. `start` isn't 0-based everywhere. The RDK's size list is asked for as `04 01 05` and answered `44 01 02` with positions 0 and 1 (logged in car), so there it counts from 1.

The MIB2 pages forward with `shift`: `00 00 0A`, then `10 09 0A` (10 elements after position 9) and `10 13 0A`. It takes the answers as starting at positions 10 and 20 (simulated, see [MIB2 Results](mib2.md#light-colours-0x08-df-51)).

The switch to 16-bit above 127 and the backward window are **untested**.

### recordAddress is per function

This nibble is a bitmap of which parts of each record you want, and each function decides what the bits mean. On the Secondary Display's FrameData it's `1` name, `2` type, `4` attributes, so `7` means all three. On the phonebook the same nibble means something else. You need the function's own definition.

## A real one

My Secondary Display emulator answers the cluster's FrameData request with a header of:

```plaintext
47 00 04
│  │  └─ 4 elements
│  └─ starting at 0
└─ 0100 0111: transmitPos = 1, recordAddress = 7 (name + type + attributes)
```

The cluster is happy with that and draws the page. See [Secondary Display Overview](../lsg/0x2b-secondary-display/README.md) for the records that follow.

## Strings inside records

Text fields in array records have their own length encoding, which isn't the same as the one in StatusAll. Short ones are logged in car, the long form is untested:

```plaintext
length <= 127:  [ len ][ bytes… ]
length >  127:  [ 0x80 | len_hi ][ len_lo ][ bytes… ]
```

Compare StatusAll, where a variable-length value always gets a 2-byte little-endian length. Don't mix the two up.

## The empty answer

This is the rule that bit me. **An FSG must always answer a GetArray**, even when it has nothing to send. The answer is a bare 3- or 5-byte header with the request's start and `elements = 0`. The mode differs (both logged in car):

| Module | Request | Empty answer | Mode |
|---|---|---|---|
| Navigation | `00 00 03` | `00 00 00` | echoed |
| RDK | `04 03 05` | `44 03 00` | transmitPos set, as in its normal answers |

The header comes after the function's own prefix, if it has one. On the Secondary Display's FrameData, `07 00 00 47 00 00` is accepted (tested with cluster, emulated).

Stay silent and the cluster asks again every 500 ms. On the RDK's size list it went on forever. On FrameData (emulated) it stopped after five and left the page. A MIB2 also gives up after five, see [MIB2 Results](mib2.md#retries). An answer in the wrong frame format (see [Frames on the Wire](frames.md)) gives exactly the same symptom, which is why this took me so long to pin down.

## Newer arrays: ASG_ID and TAID

None of my car captures have this. I have exchanges from a MIB2 on two LSGs (simulated, see [MIB2 Results](mib2.md#asg_id-and-taid)), and the rest is **untested**. Some newer array functions put one more byte in front on GetArray and StatusArray, but not on ChangedArray. List functions also add a total to the StatusArray:

```plaintext
GetArray      [ ASG_ID | TAID ][ array header ]
StatusArray   [ ASG_ID | TAID ][ total elements ][ array header ][ records… ]
ChangedArray  [ array header ][ records… ]
```

- **ASG_ID** (upper nibble) says which display is asking: 1 = cluster, 2 = head-up display and so on. Several ASGs can page through the same list without getting each other's answers.
- **TAID** (lower nibble) is a transaction ID that pairs a GetArray with its StatusArray. 0 is reserved for FSG-initiated answers, so an ASG shouldn't use it.
- **Total elements** is the length of the whole list, not of this answer. It's a 16-bit word in most functions, but a byte in some.

That's the plain list case. Some functions put fields of their own between the TAID byte and the array header (lane guidance does, instead of a total), and some do it in the GetArray too. Check the function, don't assume.

The FSG echoes ASG_ID and TAID back. When a StatusArray answers a SetGetArray and the data actually changed, it also sets the top bit of ASG_ID so that *every* ASG picks up the change.

The PQ arrays I have logged have no such byte (logged in car: navigation, audio, phone and tyre sizes). Functions can still have their own fields in front: the phone's FrameData has three.

Related: [Opcodes and Function Classes](opcodes.md) · [Secondary Display Overview](../lsg/0x2b-secondary-display/README.md) · [Frames on the Wire](frames.md) · [MIB2 Results](mib2.md)
