# Generic Functions (FCT 0x01–0x0F)

Function IDs split into two blocks:

- **0x00–0x0F: generic.** Same meaning on every LSG. This is BAP housekeeping: version, capabilities, heartbeat, operating state.
- **0x10–0x3F: the LSG's own.** 0x10 on the phone is something completely different from 0x10 on the tyre pressure module. You need that LSG's function catalogue (or a lot of captures) to know what they are.

| FCT | Name | Class | What it carries |
|---|---|---|---|
| 0x00 | – | – | reserved |
| 0x01 | GetAll | Cache | request: nothing. answer: StatusAll, every cached property |
| 0x02 | BAP_Config | Property | 6 bytes: protocol version, LSG class, LSG version |
| 0x03 | FunctionList | Property | bitmap of implemented functions, 8 bytes or cut to 6, see below |
| 0x04 | HeartBeat | Property | 1 byte: heartbeat period in 100 ms steps |
| 0x05–0x0C | – | – | unused on every LSG in the [catalogue](../catalogue/README.md) |
| 0x0D | system switch | Property | 1 byte, name varies per LSG, see below |
| 0x0E | FSG-Setup | Property | size and content vary by device |
| 0x0F | FSG_OperationState | Property | 1 byte enum |

## 0x01 GetAll / StatusAll

The ASG sends GetAll (opcode 1) with no payload. The FSG answers StatusAll (opcode 4): all its cached properties **concatenated in FCT order**. In practice that's the same set of properties it sends in its heartbeat. Variable-length values (byte streams, strings) get a **2-byte little-endian length** in front. Fixed-size ones don't.

Here's the RDK's 39-byte StatusAll from [Frames on the Wire](frames.md), pulled apart:

| Bytes | FCT | Value |
|---|---|---|
| `03 00 07 00 04 00` | 0x02 BAP_Config | BAP 3.0, LSG class 7, LSG version 4.0 |
| `08 00` + `38 07 D9 00 00 00 00 00` | 0x03 FunctionList | length 8 (LE), then the bitmap |
| `0A` | 0x04 HeartBeat | 10 × 100 ms = 1 s |
| `10` | 0x0D | SystemOnOff |
| `06 02` | 0x0E FSG-Setup | |
| `00` | 0x0F OperationState | normal operation |
| `00 00 00 00 50 00` | 0x10 | TPMS status |
| `00 1B 1B 1B 1B FF` | 0x11 | measured pressures |
| `00 1C 1C 1C 1C 1C` | 0x13 | target pressures |

That's 39 bytes exactly. RDK functions 0x14 and 0x17 aren't in it, they go out separately.

One gotcha: the length prefix depends on the value's **data type**, not on the function class. The RDK declares FunctionList as a variable-length byte stream, so it gets the prefix. Other LSGs declare it fixed and have none, see [0x03](#0x03-functionlist).

A StatusAll can be accepted and still be read wrong. What a MIB2 does with a bad one is on [MIB2 Results](mib2.md#start-up-and-statusall).

## 0x02 BAP_Config

Six bytes, in this order:

```plaintext
[BAP major][BAP minor][LSG class][LSG sub-class][LSG version major][LSG version minor]
```

The BAP version is 3.0 or 3.1. Both turn up on PQ and on MQB, so it doesn't tell you the platform. The LSG class usually matches the LSG ID, but not always. The **LSG version** (DF) says which version of that LSG's function set the FSG implements. The [catalogue](../catalogue/README.md) pages are split by it.

The display checks this, see [Versions](versions.md).

## 0x03 FunctionList

A bitmap of the functions this FSG implements. Bits are numbered **from the most significant bit of byte 0**, so bit N is function N:

```plaintext
38 07 D9 00 00 00 00 00

38 = 0011 1000   -> bits 2,3,4           (BAP_Config, FunctionList, HeartBeat)
07 = 0000 0111   -> bits 13,14,15        (0x0D, 0x0E, 0x0F)
D9 = 1101 1001   -> bits 16,17,19,20,23  (0x10, 0x11, 0x13, 0x14, 0x17)
```

That lines up exactly with every function the RDK sends. Read LSB first it's nonsense, so the bit order is settled.

It comes in two forms, depending on the LSG and its DF (logged in car; 0x07 here is DF 4.0, and DF 3.0 has it fixed). The catalogue's data type tells you which:

| Form | Catalogue type | LSGs | In StatusAll |
|---|---|---|---|
| segmented, 8 bytes | byteSequence | 0x01, 0x07, 0x0A, 0x0B, 0x21, 0x2B, 0x31, 0x32 | `08 00` + 8 bytes |
| single frame, cut to 6 bytes (`32 03 38 05 80 00 00 00`) | fixedByteSequence | 0x05, 0x08, 0x09, 0x0D, 0x0E, 0x19, 0x1A, 0x1B | 8 bytes, no prefix |

Notice **bit 1 (GetAll) is clear** on the RDK, even though it answers GetAll. Of the 16 LSGs I have logged, 11 send `38` as the first byte and five send `78` with bit 1 set: ACC 0x05, OPS 0x0A, RVC 0x0B, AWV 0x1B and the phone's 0x2B. It isn't a PQ vs MQB split, so copy what the real module sends.

A clear bit 1 proves nothing. A clear bit above 0x0F is the FSG telling you it doesn't offer that function, even if the module has it. The RDK's tyre temperature (0x12) is an example, see [0x07 RDK](../lsg/0x07-rdk.md).

## 0x04 HeartBeat

One byte, the heartbeat period in units of 100 ms (so 0 to 25.5 s). `0A` = 1 second is what I see most. The ASG uses it to size its watchdog: if heartbeats stop for some multiple of this period, it declares the FSG gone. See [Session Lifecycle](session-lifecycle.md).

## 0x0F FSG_OperationState

| Value | Meaning |
|---|---|
| 0x00 | normal operation |
| 0x01 | off / standby |
| 0x02 | meaning unknown, seen on climate and a rear camera (logged in car) |
| 0x03 | initialising |
| 0x0E | function inactive |
| 0x0F | defective |

## 0x0E FSG-Setup

Says what the device is set up to do, so the display knows which menu rows to offer. Size depends on the device: 1, 2, 3, 6 and 41 bytes all exist. Camera, parking and RDK have it. ACC, Side Assist and the Secondary Display don't.

## 0x0D, the system switch

For a long time this was the function nobody names. Across the catalogue it's on 18 LSGs, nearly always 1 byte both ways, and it comes in two kinds:

| Kind | Where | Names |
|---|---|---|
| on/off | assistance and comfort | RDK SystemOnOff, MKESystemOnOff, LDWHCASystemOnOff, AirconSystemOnOff, amplifier SoundOn |
| factory reset | body electronics and camera | interior light, exterior light, wipers, locking and mirrors `SetFactoryDefault`, camera FactorySettings |

Lane Assist's is logged in car: `01` is on, `00` is off. Side Assist has no 0x0D. Its switch is 0x13, and it's the other way round, `00` is on (logged in car). Where the catalogue still says `Fct_0D` I expect one of the two kinds, but that's untested.

## 0x10 and up: common shapes

The device functions are numbered upwards from 0x10, and most fall into a few shapes. The names are still per LSG, but the shape is a good first guess for an unknown one:

| Shape | Usually |
|---|---|
| Property, 2 bytes both ways | a setting: `[value][available]` |
| Property, 1 byte both ways | an on/off or enum setting |
| Property, 6 bytes, Get only | a status record |
| Property, 1 byte, Get only | a status or message byte |
| Array | a list |

Settings make up about 70 % of everything above 0x10.

Related: [Session Lifecycle](session-lifecycle.md) · [Opcodes and Function Classes](opcodes.md) · [0x07 RDK (Tyre Pressure)](../lsg/0x07-rdk.md)
