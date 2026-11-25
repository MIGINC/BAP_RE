# 0x07 RDK (Tyre Pressure)

The RDK (*Reifendruckkontrolle*) is the tyre pressure module, and it's the LSG I've taken furthest. **Tested with cluster, logged in car.**

| | CAN ID | Name | Bus |
|---|---|---|---|
| RDK → cluster | 0x6DD | BAP_RDK | ECAN or KCAN, whichever the module is on |
| cluster → RDK | 0x6C8 | BAP_ASG_01 | forwarded to both |

Those are the PQ IDs. My logs of it are on ECAN, from a Passat B7. On MQB the RDK seems to be on 0x17330710 (from the [Audi project](#on-a-real-audi), Untested).

Before you connect anything, read [Connect and Listen Safely](../platforms/connect-and-listen.md).

## Does your module have pressures?

Check two things it sends:

1. **0x02 BAP_Config**, bytes 4–5, the DF. Mine sends `03 00 07 00 04 00`: BAP 3.0, LSG class 7, DF 4.0. [DF 4.0](../catalogue/lsg07-rdk-df4.0.md) and [DF 4.3](../catalogue/lsg07-rdk-df4.3.md) have pressures. **[DF 3.0](../catalogue/lsg07-rdk-df3.0.md) doesn't**, only the status.
2. **0x03 FunctionList**, bit 17 (0x11) and bit 19 (0x13). If a bit is clear, the module doesn't offer it.

If nothing from 0x6DD arrives in a minute with the ignition on, you're on the wrong bus or the car has no RDK. A car with the indirect (ABS based) system has no wheel pressures to read.

## Reading it

You don't need to send anything. The RDK goes round nine properties at one a second, so 0x11 comes **every 9 s** (logged in car), and sooner when a value changes. If you join late, wait for one rotation.

- Only use frames from 0x6DD.
- Take opcode 3 (HeartbeatStatus) and opcode 4 (Status). They carry the same value.
- The StatusAll (0x01, opcode 4) has the values too, but only goes out when the cluster asks.
- Blank the display if 0x11 stops. I'd use 30 s, about three rotations. That number is only a suggestion.

There's a [trace](../../Traces/PQ/LSG_0x07_RDK_Passat_B7.md) with expected values to test your decoder on.

## Functions

My module's FunctionList (`38 07 D9 00 00 00 00 00`, logged in car) says it offers these:

| FCT | Name | Payload | Notes |
|---|---|---|---|
| 0x02–0x04 | generic | | see [Generic Functions](../protocol/generic-functions.md) |
| 0x0D | SystemOnOff | 1 byte | `10` in the car. Bit 4 is the on/off a MIB2 writes at DF 4.3 (simulated, application layer) |
| 0x0E | FSG-Setup | 2 bytes | `06 02` in the car |
| 0x0F | FSG-OperationState | 1 byte | |
| 0x10 | TPMS_Status | 6 bytes | per-wheel status, see [Status](#status) |
| 0x11 | TirePressure | 6 bytes | `[unit][FL][FR][RL][RR][spare]`, `FF` = no value |
| 0x13 | TargetPressure | 6 bytes | same layout; the recommended (door placard) value, follows the load setting |
| 0x14 | TireSizeList | Array | the menu of tyre sizes, sent on request |
| 0x17 | WheelSet | Method, 2 bytes | `[size index][load]`, load `11` full, `12` normal |

Everything but 0x14 and 0x17 is in the heartbeat rotation and the StatusAll. 0x14 only goes out when the cluster asks. 0x17 (`41 D7 01 12`) also comes unasked, alongside each 0x14 answer (logged in car). 0x17 is a Method, so the opcode 2 I first read as SetGet is StartResult.

## Pressures and units

0x11 and 0x13 are both `[unit][FL][FR][RL][RR][spare]`, and `FF` is no value:

| Unit byte | Unit | Scale | Evidence |
|---|---|---|---|
| `00` | bar | raw ÷ 10 | Tested with cluster, and fits the log below |
| `01` | psi | raw ÷ 2 | fits the log below, a MIB2 reads it as psi. Not checked against a gauge |
| `02` | kPa | unknown | a MIB2 reads the code as kPa (Untested) |

**The unit follows the car's unit setting.** That goes out on plain CAN, **0x60E `mEinheiten`**, byte 0 bits 4–5: `0` bar, `1` psi, `3` kPa. (kPa is `3` there and `02` in BAP.) Here's the RDK switching over, at ignition on:

```plaintext
572.404  0x6DD  C3 50 00 00 1B 1B 1B 1B    StatusAll: pressures in unit 00, 1B = 2.7 bar
572.454  0x6DD  C4 FF 00 1C 1C 1C 1C 1C    targets in unit 00, 1C = 2.8 bar
572.759  0x60E  98 01                      first mEinheiten: 0x98 bits 4-5 = 1, psi
572.804  0x6DD  41 D1 01 4E 4E 4E 4E FF    0x11 again, unit 01: 4E = 39 psi = 2.69 bar
572.854  0x6DD  41 D3 01 52 52 52 52 52    0x13 again, unit 01: 52 = 41 psi = 2.83 bar
```

All of that is logged in car. So:

- Read byte 0 of every message. It can change while you're running.
- If you don't know the unit, show the raw value or nothing. Don't guess a scale.
- The cluster doesn't convert. Change its pressure unit and only the label changes, with no BAP request to the RDK (tested with cluster). The RDK has to read 0x60E and send the right unit.
- I've only logged the car set to psi, so a change to kPa isn't in any log.

## Status

0x10 is `[FL][FR][RL][RR][spare][overall]`:

| Wheel byte | Meaning | Evidence |
|---|---|---|
| `00` | OK | Logged in car |
| `30` | low, about 0.2 to 0.3 bar under target | Logged in car, Tested with cluster |
| `40` | low, about 1.1 bar under target | Tested with cluster |
| `50` | no pressure | Tested with cluster |
| `60` | no sensor learned | Tested with cluster |

- The cluster's low-pressure warning and the orange wheel follow it.
- The spare byte was `50` in every frame I have. That car has no spare sensor.
- The overall byte was `00` with everything OK and `03` in the case below. I've also seen `04` and `0E`. Don't build a warning on it.

In the car, switching to full load raised the rear target to 49 psi with 42 psi in the tyres, and both rear wheels went to `30`:

```plaintext
normal load   0x13 = 01 52 52 52 52 52    0x10 = 00 00 00 00 50 00
full load     0x13 = 01 5A 5A 62 62 62    0x10 = 00 00 30 30 50 03
```

It doesn't replace the car's own warning.

## Target pressure, not temperature

I had 0x13 down as *tyre temperature*. This is from a controlled capture in the car:

```plaintext
0x17 = 01 11  (size 1, full load)     0x13 = 01 5A 5A 62 62 62
0x17 = 01 12  (size 1, normal)        0x13 = 01 52 52 52 52 52
0x17 = 02 11  (size 2, full load)     0x13 = 01 5A 5A 62 62 62
```

Changing the size doesn't move 0x13. Changing the load does, and the rear rises twice as far as the front, which is what a door placard does. **0x13 is the target (placard) pressure.**

## The tyre size list

Cluster and RDK on a Passat, just after start-up, 0x14 TireSizeList (logged in car). Times are in seconds, and only 0x6C8 and 0x6DD are shown:

```plaintext
572.489  0x6C8  80 03 11 D4 04 01 05      cluster: GetArray, recordAddress 4, start 1, 5 elements
572.504  0x6DD  80 23 41 D4 44 01 02 00   RDK: StatusArray, 35 bytes, start 1, 2 elements, pos 0
572.554  0x6DD  41 D7 01 12               0x17, in the middle of the transfer
572.604  0x6DD  C0 0A 32 33 35 2F 34 30   length 10, "235/40"
572.654  0x6DD  C1 20 52 31 38 01 12 32   " R18", pos 1, length 18, "2"
572.704  0x6DD  C2 33 35 2F 34 30 20 52   "35/40 R"
572.754  0x6DD  C3 31 38 20 43 6F 6D 66   "18 Comf"
572.804  0x6DD  41 D1 01 4E 4E 4E 4E FF   0x11
572.854  0x6DD  41 D3 01 52 52 52 52 52   0x13
572.903  0x6DD  C4 6F 72 74               "ort"
572.920  0x6C8  80 03 11 D4 04 03 05      cluster: GetArray, start 3
572.954  0x6DD  41 D7 01 12               0x17
573.004  0x6DD  80 03 41 D4 44 03 00      RDK: 0 elements, end of the list
```

| Position | Text |
|---|---|
| 0 | `235/40 R18` |
| 1 | `235/40 R18 Comfort` |

- Each record is `[position][length][ASCII]`. Both entries are the same size, the second is the Comfort one.
- The cluster asks from 1, but the positions in the answer start at 0. The size index in 0x17 was only ever `01` or `02` here, which fits these two entries counted from 1.
- Single frames (0x17, 0x11, 0x13) go out **in the middle** of the segmented answer, on the same CAN ID. A decoder mustn't drop the transfer when that happens.
- Even the 3-byte requests and the empty answer go segmented, see [Frames on the Wire](../protocol/frames.md).

## Tyre temperature is 0x12, masked off outside Audi

The temperature is there, in 0x12 TireTemperature. On my VW module its bit is clear in the FunctionList, so it's never sent. On this Audi module it's set:

| Module | FunctionList starts | 0x12 |
|---|---|---|
| VW (logged in car) | `38 07 D9 00` | clear |
| Audi 8S0 module, MQB, in an A3 ([ronaldocordeiro](https://github.com/ronaldocordeiro/Audi_A3_TPMS_on_MIB1)) | third byte `E1` | set, 0x13 and 0x14 clear |

With that bit set in the FunctionList of a VW DF 4.3 module it reads `38 07 F9 00`, and 0x12 joins the rotation and the StatusAll (tested with cluster):

```plaintext
0x6DD  31 D2 00 50 55 5A 5F FF    FL 20, FR 25, RL 30, RR 35 °C, no spare
```

- Layout: `[unit][FL][FR][RL][RR][spare]`, the same as the pressures. With unit `00` a wheel is **°C + 60**, and `FF` = no value.
- `BB` (127 °C) is what a wheel reads on that VW module until its sensor has sent something. The Audi 8S0 sends `00` there. Treat both as no value.
- The SW1104 PQ cluster takes the frames and draws nothing. A stock MIB2 tyre page doesn't show them either. With 0x12 enabled for the page, a MIB2 showed the right temperature per wheel (tested with cluster).
- Unit `01` is °F, **raw × 2 − 76**. The scale fits the two points in the Audi project below (`00` shows as −76 °F, decimal 38 as 0 °F), I haven't tested it.

### On a real Audi

[Audi_A3_TPMS_on_MIB1](https://github.com/ronaldocordeiro/Audi_A3_TPMS_on_MIB1) is an Arduino between an Audi 8S0 module and an A3 with MIB1. The author worked from the raw messages in the car, and it matches:

- On 0x17330710, their message "210" is `D2` (0x12), "209" is 0x11 and "211" is 0x13. The wheel order and unit byte are the same.
- The MIB1 shows raw 0 as −60 °C, so it's °C + 60 there too. The screen photo has temperatures per wheel.
- The VW 5Q0 module handles temperature internally and doesn't send it.

The byte they found by trial and error, changed from `E1` to `F1`, sits where the third FunctionList byte is, in both the 0x03 Status and the StatusAll (my reading, not theirs):

```plaintext
E1 = 1110 0001   -> 0x10, 0x11, 0x12, 0x17          (8S0, stock)
F1 = 1111 0001   -> 0x10, 0x11, 0x12, 0x13, 0x17    (works on MIB1)
```

The extra bit announces 0x13, which would be why the page then needs an injected 0x13. With the stock `E1` this MIB1 doesn't show the page at all. With the 5Q0's list it ignores 0x12 even when it's sent. So the display goes by the FunctionList.

## Emulating one

This is what the real one does at start-up. `>` is the RDK, `<` the cluster:

```plaintext
> 01 C2 03 00 07 00 04 00          BAP_Config, Reset
< 11 C1                            GetAll
> 41 CE 06 02                      FSG-Setup, Status
> 80 27 41 C1 …                    StatusAll, 39 bytes, segmented
< 80 03 11 D4 04 01 05             GetArray 0x14, segmented
> 80 23 41 D4 44 01 02 …           StatusArray, segmented
< 80 03 11 D4 04 03 05             the next window
> 80 03 41 D4 44 03 00             empty answer, still segmented
> 31 C2 … 31 D3                    then the rotation, one a second
```

The full bytes are in the [trace](../../Traces/PQ/LSG_0x07_RDK_Passat_B7.md), and the rules are in [Session Lifecycle](../protocol/session-lifecycle.md#what-this-means-if-youre-emulating-an-fsg).

**Only do this on a bench.** Two modules sending 0x6DD is a fault, also when one comes through the gateway. I don't know which clusters need coding before they show the tyre page.

## Open questions

- 0x15 and 0x1A have no name. 0x18 only has one at DF 3.0.
- The load codes of 0x17: `11` and `12` are at DF 4.0. At 4.3, codes 1 and 2 may be swapped (Untested), see [Versions](../protocol/versions.md#minor-can-still-matter). The upper nibble is unknown.
- A full log of 0x12 from a real Audi module.
- The unit `01` scale against a gauge, and a log of a car set to kPa.
- The overall byte of 0x10.

Related: [LSG Index](README.md) · [Frames on the Wire](../protocol/frames.md) · [Session Lifecycle](../protocol/session-lifecycle.md) · [Connect and Listen Safely](../platforms/connect-and-listen.md)
