# Versions

Two modules with the same LSG ID don't have to speak the same payloads. The version tells you which ones you've got.

## Three version numbers

| Version | Where | What it covers |
|---|---|---|
| BAP version | BAP_Config bytes 0–1 | The protocol itself. 3.0 or 3.1 |
| LSG version (DF) | BAP_Config bytes 4–5 | The function set of that one LSG. The [catalogue](../catalogue/README.md) is split by it |
| Display protocol | 0x2B function 0x11 | Secondary Display only, 1.0 or 1.1. Not the DF, see [Secondary Display](../lsg/0x2b-secondary-display/README.md) |

So `03 01 0B 00 05 01` is BAP 3.1, LSG class 0x0B, DF 5.1.

## What the display compares

### Cluster

- The **DF major** has to match and the DF minor isn't checked. Untested as a deliberate mismatch on the cluster.
- The **BAP minor** must not be higher than the cluster's own definition. A 3.1 module on an LSG it knows at 3.0 is a version mismatch (tested with cluster).
- On that BAP minor mismatch the cluster sends no GetAll and no SetGet, and the LSG never runs. It stores a "wrong software version" fault, B200100 (tested with cluster). The module can look perfectly alive on the bus and still not be used. I'd expect the same for the wrong DF major, but haven't tried it.
- Some LSGs have more than one definition in the cluster, for example the RDK at 3.0 and 4.3 and parking at 3.1 and 4.1. It picks one from what the FSG reports. Parking at 4.1 gets its settings menu entry, and at 3.1 it doesn't (tested with cluster).

### MIB2

On the LSGs I tried, the MIB2 checks the BAP major, the DF major and the LSG class, but not the minors (emulated, tested with cluster). The full table is on [MIB2 Results](mib2.md#version-check).

So the **BAP minor** is where they differ: the MIB2 takes a 3.1 module on a 3.0 LSG, and the cluster doesn't. Coming up on the radio doesn't mean it'll come up on the cluster.

A module the MIB2 rejects gets nothing back on CAN: no Get, no GetAll.

## Minor adds, major changes

I compared each DF in the catalogue with the next one I have for the same LSG: 25 pairs, functions 0x0D and up. This is a table comparison, so it's **untested** on the wire.

| Step | Pairs | Functions removed | Class, type or size changed |
|---|---|---|---|
| Minor (3.0 → 3.4) | 19 | none | none in 17 of 19 |
| Major (3.3 → 4.1) | 6 | none | 5 of 6 |

The two minor exceptions are sizes only: VZA 0x10 sends up to 123 bytes instead of 120, and Amplifier 0x15 accepts 7 instead of 16. I don't know whether those belong to the DF or to the modules.

A minor step usually just adds functions at the end: ACC 3.0 → 3.4 adds 0x15–0x1B, SWA 3.0 → 3.1 adds 0x16–0x19 and 3.4 adds 0x1A–0x1D.

A major step changes payloads **under the same function ID**, very often one byte becoming two:

| LSG | Step | Changes |
|---|---|---|
| RDK | 3.0 → 4.0 | 0x0E: 1 → 2 bytes |
| Aussenlicht | 4.3 → 5.4 | 0x10 and 0x1A: 1 → 2 bytes, 0x16: 2 → 3 |
| OPS | 3.3 → 4.1 | 0x15 and 0x18: 1 → 2 bytes, 0x0E: 3 → 6, 0x12 and 0x13: 4 → 6, 0x19: 1 → 6, 0x1A: 2 → 6, 0x1B and 0x1C: 4 → 2 |
| RVC | 3.1 → 5.1 | 0x11–0x14: 1 → 2 bytes |
| Doorlocking | 3.3 → 4.5 | 0x10, 0x11, 0x13 and 0x1B: 1 → 2 bytes, 0x1A: 1 → 6, 0x1C: 6 → 3, 0x1D: 2 → 1, 0x1E: 3 → 1, 0x1F: Property → Method |

(Bytes are what the FSG sends. What it accepts changes on some of these too.) Innenlicht 3.0 → 5.1 is the exception: it only adds functions.

That's only what the versions I have look like, and there are gaps between them.

## Minor can still matter

A minor that isn't checked in the handshake can still change how a function is read. These are **untested** leads:

- ACC 0x1A is only used from 3.4.
- AWV 0x15 is only used from 3.2.
- RDK 0x17: at 4.0 load codes 1 and 2 are swapped compared with 4.3.

## Seen in cars

BAP_Config as sent by the modules, all on PQ (logged in car):

| LSG | BAP | DF | | LSG | BAP | DF |
|---|---|---|---|---|---|---|
| 0x01 Klima1 | 3.0 | 3.3 | | 0x0E Mirror | 3.0 | 3.0 |
| 0x05 ACC | 3.0 | 3.4 | | 0x19 LDW | 3.0 | 4.0 |
| 0x07 RDK | 3.0 | 4.0 | | 0x1A SWA | 3.0 | 3.4 |
| 0x08 Innenlicht | 3.0 | 3.0 | | 0x1B AWV | 3.0 | 3.2 |
| 0x09 Aussenlicht | 3.0 | 4.3 | | 0x21 VZA | 3.0 | 3.0 |
| 0x0A OPS | 3.1 | 4.1 | | 0x2B UHV_SecondaryDisplay | 3.0 | 3.1 |
| 0x0B RVC | 3.1 | 5.1 | | 0x31 Audio | 3.0 | 3.1 |
| 0x0D Doorlocking | 3.0 | 3.3 | | 0x32 Navigation | 3.0 | 3.0 |

So "PQ is 3.x and MQB is 4.x" doesn't hold. The DF belongs to the module, not the car.

## ASG_ID and TAID

Some newer array functions put an ASG_ID and TAID byte in front of the array header. That depends on the function and its DF, not on the platform. A 4.x DF doesn't mean it's there: the RDK's array at 4.0 doesn't have it. I don't know at which DF each function gained it. Layout in [Arrays](arrays.md).

Related: [Generic Functions](generic-functions.md) · [Session Lifecycle](session-lifecycle.md) · [MIB2 Results](mib2.md) · [Function catalogue](../catalogue/README.md)
