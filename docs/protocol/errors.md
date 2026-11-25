# Errors

There are two kinds:

1. **Errors on the wire.** The FSG answers a request with opcode 7 and a one-byte code.
2. **Errors inside a module.** A transfer doesn't reassemble, a heartbeat stops, a request times out. Nothing goes on the bus. The receiver drops the transfer, and the requester's retry asks again.

## Codes

| Code | Meaning | Status |
|---|---|---|
| 0x00 | no error | – |
| 0x41 | value out of range | Untested |
| 0x83 | data invalid, the value doesn't fit what the function expects | Tested with cluster |

0x83 came from my own emulated FSG, refusing a real cluster write because the frame was padded to DLC 8 (see [Frames on the Wire](frames.md)). So if you see 0x83, check the length before you blame the data.

## Frame form

**The error uses the frame form of the function.** On a segmented function, a MIB2 ignored a single-frame error and kept retrying (tested with cluster). What it does with each error is on [MIB2 Results](mib2.md#errors).

None of my car logs has an opcode 7 in it. If you have a capture that does, I'd like to see it.

Related: [Session Lifecycle](session-lifecycle.md) · [Opcodes and Function Classes](opcodes.md) · [MIB2 Results](mib2.md)
