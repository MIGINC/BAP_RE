# MQB Notes

MQB buses are 500 kbit/s high speed. MQB Evo has CAN FD and isn't covered.

Almost all of my BAP work so far has been on PQ, so this page is short and honest about it.

## What the first version got wrong

I said MQB moves the LSG into a 29-bit CAN ID and drops the BAP header, leaving byte 0 as the opcode and byte 1 as the function. My own example disproved it. The MQB capture I used was:

```plaintext
CAN ID 0x17333310
80 92 4C C1 03 01 33 00 …
```

Decode it as PQ-style BAP:

```plaintext
80 92       start frame, channel 0, length 0x092 = 146 bytes
4C C1       0x4C = 0 100 1100 -> opcode 4 (Status…), LSG high 1100
            0xC1 = 11 000001  -> LSG low 11, FCT 0x01
            LSG = 110011 = 0x33, FCT 0x01, opcode 4 = StatusAll
```

A StatusAll for LSG 0x33, which matches the 0x33 in the CAN ID. So the **frame format, the 2-byte header and the segmentation are the same as PQ**.

## What seems to be different

- **CAN IDs.** MQB infotainment BAP uses 29-bit extended IDs with the LSG in them. A community table (Drive2, user VanHighlander) follows a `0x1733 LL xx` pattern: `LL` is the LSG, `xx = 00` is the ASG sending and `10` is the FSG sending (`01` for a second ASG). The same capture has the ASG's request just before the answer above:

  ```plaintext
  38.984  0x17333300  1C C1                     ASG: GetAll, LSG 0x33 (op 1)
  38.985  0x17333310  80 92 4C C1 03 01 33 00   FSG: StatusAll, 146 bytes
  ```

  The same header, the same opcodes, and the same request/answer pattern as PQ. The LSG is simply repeated in the CAN ID so hardware filters can pick it out. I haven't checked the rest of the table myself; it's in the [CAN ID Map](can-id-map.md).
- **Arrays.** Some newer array functions have an ASG_ID / TAID byte in front so several displays can page through the same list. That goes with the function and its DF, not with MQB as such. See [Arrays](../protocol/arrays.md).
- **Versions.** The answer above announces LSG version `03 0B`, DF 3.11, while the [eCall catalogue page](../catalogue/lsg33-ecall-df3.5.md) is 3.5. LSG versions tend to be higher, but PQ modules reach 4.x and 5.x too. A newer ASG carries definitions for several versions. See [Versions](../protocol/versions.md).

If you've got MQB captures with BAP in them and want to help fill this page in, open a Discussion or a PR.

Related: [Frames on the Wire](../protocol/frames.md) · [PQ CAN Channels](pq-can-channels.md)
