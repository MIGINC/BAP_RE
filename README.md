# BAP Protocol Reverse Engineering

Documentation and analysis of **BAP**, the *Bedien- und Anzeigeprotokoll* ("operating and display protocol"), for PQ and MQB platforms. It's how a VW Group function module (phone, tyre pressure, parking sensors, ACC, rear camera…) gets its state and settings onto a display module (the cluster or the radio).

This is a rewrite. Parts of the first version were wrong, see [what changed](#what-changed). LSGs, CAN IDs and the main claims now have a [status tag](#status-tags).

**GitHub Discussions are open.** Share CAN traces, ask questions, send corrections. See [CONTRIBUTING](CONTRIBUTING.md).

## Protocol overview

- **Event driven, with a heartbeat**: modules report changes, plus one property per heartbeat period
- **Fixed 16-bit header** with opcode, LSG and function ID
- **Segmentation** for anything that needs it
- **Versioned**: each LSG has its own version (DF), which decides its functions
- **Same framing on PQ and MQB**, only the CAN IDs differ

## Acronyms & definitions

- **BAP** – Bedien- und Anzeigeprotokoll (operating and display protocol)
- **DDP** – Display Data Protocol (predecessor to BAP)
- **FSG** – Funktionssteuergerät (function control unit), owns the data
- **ASG** – Anzeigesteuergerät (display control unit), shows it and lets the driver change it
- **LSG** – Logisches Steuergerät (logical control unit), one function group. A box can host several
- **FCT** – function ID inside an LSG
- **DF** – the LSG version, from BAP_Config
- **RDK** – Reifendruckkontrolle, the tyre pressure module
- **RNS / MIB2** – VW radios, RNS 510 and the later MIB2
- **Cluster / Kombi** – the instrument cluster
- **MFL** – the steering wheel buttons (Multifunktionslenkrad)
- **UHV** – the factory phone module (Universelle Handyvorbereitung)
- **Coding** – a module's configuration, set with a diagnostic tool
- **Placard** – the recommended tyre pressures on the door sticker

## BAP on one screen

```plaintext
BAP header (every message, 16 bits)
  byte 0:  0 | O O O | L L L L      O = opcode (3)   L = LSG (6, split 4 + 2)
  byte 1:  L L | F F F F F F        F = function ID (6)

Single frame        [hdr hi][hdr lo][0-6 data]             length = CAN DLC
Start frame         [10CC LLLL][LLLLLLLL][hdr hi][hdr lo][4 data]   L = 12-bit payload length
Continuation        [11CC SSSS][7 data]                    C = channel 0-3, S = sequence 0-15 (wraps)
```

| Op | Property | Array | Method | Cache | Sent by |
|---|---|---|---|---|---|
| 0 | Set / Reset | SetArray | Start | – | ASG (Reset: FSG) |
| 1 | Get | GetArray | Abort | GetAll | ASG |
| 2 | SetGet | SetGetArray | StartResult | – | ASG |
| 3 | HeartbeatStatus | ChangedArray | Processing | – | FSG |
| 4 | Status | StatusArray | Result | StatusAll | FSG |
| 5 | StatusAck | – | – | – | FSG, few functions |
| 6 | Ack | – | – | – | ASG, answering a 5 |
| 7 | Error | Error | Error | Error | FSG |

A real exchange, cluster and tyre pressure module, logged in car:

```plaintext
0x6C8  11 C1                     cluster: GetAll          (op 1, LSG 0x07, FCT 0x01)
0x6DD  80 27 41 C1 03 00 07 00   RDK:     StatusAll, 39 bytes, segmented
       C0 04 00 08 00 38 07 D9   …
```

Only want to read values from a car? Start with [Connect and listen safely](docs/platforms/connect-and-listen.md), then [0x07 RDK](docs/lsg/0x07-rdk.md).

Emulating a module? Read [Frames](docs/protocol/frames.md), [Session lifecycle](docs/protocol/session-lifecycle.md) and [Arrays](docs/protocol/arrays.md), then the LSG's page, and the bench part of [Connect and listen safely](docs/platforms/connect-and-listen.md#on-a-bench). Most LSGs have a [start-up trace](Traces/PQ/README.md).

Three things that trip everyone up when **emulating** a module:

1. **Segmentation is set per function, not decided by length.** A 3-byte array still goes segmented, and a receiver drops the wrong frame form. [More](docs/protocol/frames.md#segmentation-is-decided-per-function-not-by-length)
2. **The FSG heartbeats and the ASG just listens.** One property per heartbeat period, in rotation. [More](docs/protocol/session-lifecycle.md)
3. **An FSG must always answer a GetArray**, even with zero elements, or the cluster re-asks every 500 ms, in some cases forever. [More](docs/protocol/arrays.md#the-empty-answer)

## Docs

**Protocol**
- [Frames on the wire](docs/protocol/frames.md): header, frame types, channels, segmentation, reassembly
- [Opcodes and function classes](docs/protocol/opcodes.md)
- [Generic functions 0x01–0x0F](docs/protocol/generic-functions.md): GetAll, BAP_Config, FunctionList, HeartBeat, 0x0D
- [Versions](docs/protocol/versions.md): BAP version, DF, what the cluster checks, what changes between versions
- [Session lifecycle](docs/protocol/session-lifecycle.md): start-up, heartbeat, a checklist for emulating an FSG
- [Arrays](docs/protocol/arrays.md)
- [Errors](docs/protocol/errors.md)
- [MIB2 results](docs/protocol/mib2.md): version checks, retries, errors, ASG_ID and TAID

**Platforms**
- [Connect and listen safely](docs/platforms/connect-and-listen.md): bus, bit rate, transceiver, listen-only
- [CAN ID map](docs/platforms/can-id-map.md): CAN ID → LSG → FSG/ASG, PQ and MQB (raw: [data/](data/))
- [PQ CAN channels](docs/platforms/pq-can-channels.md) and [PQ network map](docs/platforms/pq-network-map.md)
- [MQB notes](docs/platforms/mqb.md)

**LSGs**
- [LSG index](docs/lsg/README.md)
- [Function catalogue](docs/catalogue/README.md): functions, types and sizes per LSG and DF, Markdown and [JSON](data/catalogue)
- [ASG requests (MIB2)](docs/catalogue/asg-requests-mib2.md): what a MIB2 writes when a setting changes
- [0x07 RDK, tyre pressure](docs/lsg/0x07-rdk.md)
- [0x08 Interior light](docs/lsg/0x08-innenlicht.md)
- [0x2B Secondary Display](docs/lsg/0x2b-secondary-display/README.md): phone text on the cluster

**Traces**
- [Traces/](Traces/README.md)

## Status tags

| Tag | Meaning |
|---|---|
| **Tested with cluster** | tested against a real or emulated cluster or MIB2 |
| **Logged in car** | it's in a capture from a real car |
| **Simulated (application layer)** | values put into an emulated MIB2 above BAP, and decoded. Says what a value means, says nothing about framing, versions, timing or CAN IDs |
| **Untested** | none of these yet. Treat it as a lead, not a fact |

Cluster tests are tagged per LSG, so an individual function on a tested LSG may not have been exercised. Car captures are tagged per function in the catalogue.

## What changed

- Continuation frames are a 2-bit channel and a 4-bit sequence, not a 6-bit index
- Start frames carry a 12-bit length and the header, then 4 payload bytes
- The ASG doesn't answer heartbeats
- MQB keeps the 2-byte header. The LSG is in the CAN ID as well, not instead
- 0x1A is Side Assist, Lane Assist is 0x19
- MQB 0x1A: suffix 00 is the MIB and 01 is the cluster. The first version had them the other way round
- A failed multi-frame transfer isn't requested again with the Error opcode
- 0x10 ASG_Capabilities and 0x11 ASG_Config belong to the Secondary Display only, they aren't generic
- RDK unit `01` is psi, raw ÷ 2. It was 40 counts per bar
- The RDK has no functions 0x28 to 0x3D
- MQB 0x08 is the interior light, the first version said exterior

New since then: versions, FCT 0x0D, opcodes 5 and 6 (StatusAck and Ack), the function catalogue and the CAN ID map

## Research status

| Area | State |
|---|---|
| PQ framing, header, opcodes | done |
| Start-up, GetAll, heartbeat | done, emulated FSGs reach the running state on a cluster |
| Arrays | header known, some records decoded |
| Error handling | little seen on the wire |
| Function names | varies by LSG, see the catalogue |
| MQB | framing confirmed from one trace, IDs mostly untested |

## MQB data source & accuracy

Much of the MQB ID mapping came from **Drive2 (user VanHighlander)** and is still being verified. PRs are welcome.

## Safety

Emulate on a bench, and in a car only listen. See [Connect and listen safely](docs/platforms/connect-and-listen.md).

## License

MIT, see [LICENSE](LICENSE).

## Acknowledgments

- Karl Dietmann's BAP research (2009–2012)
- kisim project (BSD-3), https://github.com/tmbinc/kisim
- revag-bap contributors (GPLv2), https://github.com/norly/revag-bap/
- ronaldocordeiro, for the Audi TPMS retrofit (0x07 tyre temperature), https://github.com/ronaldocordeiro/Audi_A3_TPMS_on_MIB1
- Drive2 user VanHighlander, for the MQB CAN ID table
