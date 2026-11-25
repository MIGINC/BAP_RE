# MIB2 Results

What a MIB2 does as an ASG, collected here so the other pages can stick to the protocol. The other direction, what it writes, is on the [ASG requests](../catalogue/asg-requests-mib2.md) page.

Unless a section says otherwise, this is an emulated MIB2 with stock firmware and simulated time (**tested with cluster**). **Simulated** sections are tagged.

## Version check

Only BAP_Config was changed:

| BAP_Config has | Tried on | Result |
|---|---|---|
| DF minor lower or higher | 0x1A (3.1 → 3.0, 3.2, 3.9), 0x01 (3.3 → 3.1, 3.4), 0x0B (5.5 → 5.1, 5.3) | accepted |
| DF major wrong | 0x1A and 0x01 (2.x, 4.x), 0x0B 5.5 (4.x, 6.0) | rejected |
| BAP minor higher | 0x1A (3.1, 3.2), 0x01 and 0x0B at DF 3.1 (3.1), all on a 3.0 LSG | accepted |
| BAP minor lower | 0x0B 5.5 (3.0 on a 3.1 LSG) | accepted |
| BAP major wrong | 0x1A (2.0, 4.0), 0x01 (4.0) | rejected |
| LSG class wrong | 0x1A (1B) | rejected |
| LSG sub-class 01 | 0x1A | accepted |

- "Accepted" means FunctionList and GetAll went through. The minors only changed the announced number, the payloads stayed the same.
- A rejected module gets nothing back on CAN: no Get, no GetAll. The MIB2 checks again on every BAP_Config heartbeat. On 0x1A, a Reset with the right version brought it up straight away.
- I also sent camera payloads at 5.1 and 5.3. The MIB2 reads them with its 5.5 definition and decodes them the same way. The functions are on the [5.1](../catalogue/lsg0B-rvc-df5.1.md) and [5.3](../catalogue/lsg0B-rvc-df5.3.md) pages.
- 0x0B is accepted at DF majors 3 and 5. A module that announced 3.x but sent the 5.5 FunctionList was rejected after the FunctionList. I don't know why. Extra FunctionList bits alone are accepted (tried on 0x1A and on 0x0B at 3.1).
- **Simulated:** eCall (0x33 at 3.5) is off by default in the unit I emulate, so it was enabled for the test. It follows the same rules: DF 3.11, BAP minor 0 or 2 and sub-class 01 accepted, DF major 4 and BAP major 4 rejected.
- **Simulated:** 0x02, 0x1D and 0x37 are off in the same way. Nothing goes out until they're enabled, then they come up normally. I don't know what enables them on a real unit.

## Start-up and StatusAll

The MIB2 asks for FunctionList first and then GetAll, 50 ms apart.

With a StatusAll that's off (0x01 and 0x1A, both with a prefixed FunctionList):

| StatusAll | Result |
|---|---|
| two bytes too long | accepted, values right |
| no prefix on FunctionList | accepted, values right. I don't know why |
| prefix sent twice | accepted, but the values after it are wrong or missing |
| two bytes too short | rejected. It then asks for a property with Get when it needs one |

So a StatusAll that's accepted can still be wrong.

## Retries

Tried on 0x01, 0x1A and 0x2D:

| Request | Unanswered |
|---|---|
| Get BAP_Config | 5 requests, 500 ms apart, then 20 s until the next burst |
| Get FunctionList | 5 requests, 500 ms apart, then it gives up. A FunctionList heartbeat during the retries didn't count as the answer |
| GetAll | 5 requests, 500 ms apart, then it gives up. One gap was about 1 s, around a BAP_Config heartbeat |
| SetGet, property | 5 requests, 500 ms apart (0x01, 0x1A) |
| GetArray, StartResult | 5 requests, 600 ms apart (0x2D) |
| SetGetArray, segmented | 5 requests, 650 ms apart, so 600 ms from its last frame (0x2D) |
| Get on a cached property | nothing goes on the bus, the MIB2 answers it from its cache |

- After the fifth request, nothing more went out in the 20 s or so I watched.
- On a method, opcode 4 (Result) and opcode 5 both stopped the retries. Opcode 6 was ignored. Opcode 3 (Processing) wasn't tried.
- **Giving up on GetAll isn't always the end.** 0x1A still came up at 14 to 16 s, once the rotation had been through every property. I think the heartbeats filled the cache. 0x01 has a longer rotation and took 44 s.
- **Giving up on FunctionList is the end.** 0x01 still wasn't up after 90 s.

## Watchdog

With a 1 s heartbeat it fired 5 s after the last message (0x01 and 0x1A). That looks like 5 periods, but I only tried the one. About 0.9 s later it asked for BAP_Config, in the five-and-a-pause pattern above.

## Errors

Tried on 0x01, 0x1A and 0x2D with error code 0x22, and 0x11 as well on the array:

| Error in answer to | What it does |
|---|---|
| SetGet on a property, StartResult on a method | stops at once, no retry |
| GetArray | stops at once, if the error is segmented |
| GetAll | stops at once, if the error is segmented |
| Get FunctionList | stops, doesn't ask again. The LSG didn't come up in 30 s |

On 0x2D's array and on GetAll, a single-frame error was ignored and all five requests went out. A single-frame StatusArray was ignored too.

After an error on GetAll it didn't ask again, but the LSG could still come up from the heartbeats: 0x2D after 8 s, 0x1A after 14 s.

## StatusAck

**Simulated (application layer).** On eCall (0x33) 0x10 and 0x11 the MIB2 Acks with the same bytes, and only acts on a StatusAck. A plain Status did nothing, and a StatusAck with the same value straight after it was then ignored too.

## ASG_ID and TAID

The layout is in [Arrays](arrays.md#newer-arrays-asg_id-and-taid). Everything here is **simulated**, and it really is per function.

### Light colours, 0x08 DF 5.1

Function 0x2A, 30 colours, 3 bytes (R G B) each. The total is **one byte** here:

```plaintext
MIB2  12 2A | 18 00 00 0A            ASG 1, TAID 8: 10 elements from 0
FSG   42 2A | 18 1E 00 00 0A | …     same byte back, total 30, 10 colours
MIB2  12 2A | 19 10 09 0A            TAID 9: the 10 after position 9
MIB2  12 2A | 1A 10 13 0A            TAID 10: the 10 after position 19
```

(The answer goes segmented, the start frame isn't shown.) The FSG is my own. Trust it for what the MIB2 sends and accepts, not for framing, timing or CAN IDs.

- **TAID** counts 1 to 15 and skips 0, one step per request.
- **Wrong TAID**: the MIB2 doesn't accept the answer. It asked again with the same TAID about every 2 s, five times in total, and never asked for the next page.
- **Wrong ASG_ID**: with 2 the records were dropped (the total was still taken). 0, 1 and 8 were passed on. 8 is the top bit the FSG sets on a change.
- **ChangedArray** is `[mode][start][elements]` with no ASG_ID and TAID byte. With the byte in front, the fields were misread. In both cases the MIB2 fetched the whole list again. It has to go segmented: a single-frame ChangedArray was ignored.

### Online destinations, 0x37 DF 3.3

**Simulated (application layer)**, my own FSG above the MIB2's BAP stack. The requests at start:

```plaintext
0x15   11 00 00 03       ASG 1, TAID 1: 3 elements from 0
0x16   11 01 00 01       ASG 1, TAID 1, mode 01: 1 element from 0
0x1E   12 01 01 00 05    ASG 1, TAID 2, then 4 bytes instead of 3
```

0x1E is only read by itself if the FunctionList goes past the 3.3 one. Its layout isn't known beyond the first byte.

- **TAID counts per function** here, so 0x15 and 0x16 both start at 1.
- **Wrong TAID or wrong ASG_ID** (0, 2 or 8): the same request came four more times, 5 s apart, and then it gave up. So this one rejects ASG_ID 8, which the light colour list passed on.
- **ChangedArray** `00 00 FF`, without the byte, made it read the list again with the next TAID.

### Others

Request prefix only, simulated (application layer):

| LSG | Functions | ASG_ID |
|---|---|---|
| 0x0D, DF 4.5 | 0x24 | 3 |
| 0x14 | 0x10 | 1 |
| 0x23 | 0x10 | **no such byte** |
| 0x23 | 0x15, 0x16 | 1 |
| 0x25 | 0x19, 0x1A | 1 |
| 0x27 | 0x10 to 0x12 | 1 |

Related: [Versions](versions.md) · [Session Lifecycle](session-lifecycle.md) · [Arrays](arrays.md) · [Errors](errors.md)
