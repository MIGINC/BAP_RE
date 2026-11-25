# LSG_0x33_unknown_eCall_MQB

**Trace file**: `LSG_0x33_unknown_eCall_MQB.crtd`
**Part number**: unknown
**LSG**: 0x33 (eCall)
**Vehicle**: unknown, I'm not sure it was a car
**Platform**: MQB, 29-bit IDs
**Status**: Untested

From the first version of this repo. One GetAll and the StatusAll that answers it, 22 frames. The StatusAll announces 146 bytes and the log ends at 144, so the last frame is missing. Decoded in the [MQB notes](../../docs/platforms/mqb.md).

| ID | Direction |
|---|---|
| 0x17333300 | ASG → FSG |
| 0x17333310 | FSG → ASG |
