# Frames on the Wire

BAP rides on ordinary CAN frames. On PQ each FSG/ASG pair gets its own pair of 11-bit IDs (see [PQ CAN Channels](../platforms/pq-can-channels.md)).

## The BAP header

Every BAP message carries a 16-bit header:

```plaintext
byte 0:  0 | O O O | L L L L        O = opcode (3 bits)
byte 1:  L L | F F F F F F          L = LSG ID (6 bits, split 4 + 2)
                                    F = FCT ID (6 bits)
```

Bit 7 of byte 0 is zero in a single frame. In the segmented forms it flags the frame type, as shown further down.

Let's decode a real one. The tyre pressure module (RDK) answering a wheel-size request sends `41 D4`:

```plaintext
0x41 = 0 100 0001   -> opcode 4 (Status), LSG high bits 0001
0xD4 = 11 010100    -> LSG low bits 11, FCT 0x14

LSG = 0001 11 = 0x07 (RDK), FCT = 0x14, opcode = Status
```

## Three frame types

The top two bits of byte 0 tell you what you're looking at:

| Byte 0 | Frame |
|---|---|
| `0xxx xxxx` | single frame (the header itself starts here) |
| `10xx xxxx` | start of a segmented message |
| `11xx xxxx` | continuation of a segmented message |

### Single frame

```plaintext
[ header hi ][ header lo ][ up to 6 data bytes ]
```

There's **no length field**. The receiver takes the length from the CAN DLC. That works for fixed-size values, but variable-length functions can't use this form.

The Passat B7 cluster sends some requests at DLC 8 with stale padding bytes on the end. A SetGet to Side Assist arrives as `26 93 01 FF A8 69 03 00`, which is really just `26 93 01`. Take the size the function is declared with and ignore the extra bytes. Insist on an exact length and you'll reject valid writes from the cluster.

### Start frame

```plaintext
byte 0:  1 0 | C C | N N N N        C = logical channel (0-3)
byte 1:  N N N N N N N N            N = payload length, 12 bits (max 4095)
byte 2:  BAP header hi
byte 3:  BAP header lo
byte 4-7: first 4 payload bytes
```

The length counts payload bytes only, not the header. If it's 4 or less, the start frame **is the whole message** and no continuation follows.

### Continuation frame

```plaintext
byte 0:  1 1 | C C | S S S S        S = sequence number, 0-15, wraps to 0
byte 1-7: next 7 payload bytes
```

The first continuation is sequence 0, so on channel 0 you'll see `C0 C1 C2 … CF C0 C1 …`. Long transfers just wrap.

### Logical channels

Each CAN ID gets four channels, so a sender can start a second segmented message while a long one is still in flight. Receivers keep one reassembly buffer per **(CAN ID, channel)**.

Most traffic is on channel 0 (`0x80`/`0xC0`), but a receiver that only handles channel 0 loses these (logged in car):

- channel 1: the StatusAll of 0x09, 0x0E and 0x1B on 0x6DF, 0x6DE and 0x674 (`90 23 42 41 …` then `D0 …`)
- channel 1: the cluster's 0x12 SetGet to 0x2B (`90 08 2A D2 …` then `D0 …`), and some audio and navigation
- channel 2: some audio and navigation

## Segmentation is decided per function, not by length

This one cost me a lot of bench time. The obvious rule is "more than 6 bytes, segment it". It's wrong. Here's the RDK answering the cluster's wheel-size page with a 3-byte payload:

```plaintext
0x6DD  80 03 41 D4 44 03 00
       │  │  └──┴─ header: Status, LSG 0x07, FCT 0x14
       │  └─ length 3
       └─ start frame, channel 0
```

Three bytes would fit in a single frame, yet it's segmented. The same module sends other 2-byte values from the same LSG as plain single frames.

Segmentation is a **fixed property of each function**, for each direction (tested with cluster):

- The receiver filters on it. A function that should be segmented **drops** a single frame, and the other way around.
- Functions whose length varies (arrays, byte streams) are segmented. Only a start frame can state the length.
- FunctionList has both forms: segmented on some LSGs, a single frame on others. See [Generic Functions](generic-functions.md#0x03-functionlist).

The catalogue's type tells you which form the FSG sends:

| Catalogue says | Form |
|---|---|
| Array, or type `byteSequence` or a string | segmented |
| `fixedByteSequence`, `int8` and other fixed sizes | single frame |

That held for all 124 functions I could check in my car logs (opcodes 3 and 4). For the other direction, I only know the functions I've seen.

Get it wrong and the cluster silently drops your reply. It then asks again about every 500 ms (on the RDK's list, forever). Each reply you send restarts your heartbeat timer, so this looks like a *heartbeat timing* bug. Don't chase the timing.

## A real multi-frame message

The RDK's start-up StatusAll, straight off the bus (timestamps in seconds):

```plaintext
572.204  0x6DD  80 27 41 C1 03 00 07 00    start: 0x27 = 39 bytes, StatusAll LSG 0x07 FCT 0x01
572.254         C0 04 00 08 00 38 07 D9    seq 0
572.304         C1 00 00 00 00 00 0A 10    seq 1
572.354         C2 06 02 00 00 00 00 00    seq 2
572.404         C3 50 00 00 1B 1B 1B 1B    seq 3
572.454         C4 FF 00 1C 1C 1C 1C 1C    seq 4
```

That's 4 bytes in the start frame plus 5 × 7 in the continuations = 39. The 39 bytes are decoded in [Generic Functions](generic-functions.md).

## Edge cases

How I handle these, and it has worked against the cluster:

- **Wrong sequence number**: the transfer is abandoned. No attempt to resync.
- **Continuation with no start frame**: ignored silently.
- **New start frame on a channel that's mid-transfer**: the old transfer is dropped silently, and the new one starts.
- **Truncated frames** (DLC too short for what the length promised): rejected, not patched up.
- **A continuation frame with DLC 1** carries no data and is rejected.

None of these produce an error frame on the bus. See [Errors](errors.md).

## Timing

Two timers matter:

- **Gap between frames** on one CAN ID. The RDK above is on 50 ms, and so are the comfort and driver-assist modules. The phone, audio and navigation IDs are faster, 20 to 30 ms (logged in car). It applies after every frame, not just between continuations.
- **Receive timeout between segments.** I use 500 ms (untested against the cluster's own value), **restarted by every frame**. It isn't a deadline for the whole message. A 700-byte transfer at 50 ms per frame takes five seconds, and that's fine.

## Reassembly, the short version

1. Bit 7 of byte 0 clear: a single frame. Deliver the header and DLC − 2 data bytes.
2. Start frame: drop any transfer on that (CAN ID, channel). Store the length and header, set the sequence to 0, and copy up to 4 bytes.
3. Continuation: check that a transfer is active, the timeout hasn't passed and the sequence matches. Copy up to 7 bytes and step the sequence.
4. Restart the timeout on every frame.
5. All bytes in: deliver.

An ASG or FSG also has to drop the wrong form for the function. A passive listener can take both.

Related: [Opcodes and Function Classes](opcodes.md) · [Generic Functions](generic-functions.md) · [PQ CAN Channels](../platforms/pq-can-channels.md)
