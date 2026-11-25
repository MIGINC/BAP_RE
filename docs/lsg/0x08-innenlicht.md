# 0x08 Innenlicht (Interior Light)

Two generations: [DF 3.0](../catalogue/lsg08-innenlicht-df3.0.md) is the PQ one (tested with cluster, logged in car), [DF 5.1](../catalogue/lsg08-innenlicht-df5.1.md) is the MQB one, with ambient colour and zones. Everything below is DF 5.1 and **untested** against a cluster or a car, so treat it as a lead.

| | CAN ID (MQB) | Name |
|---|---|---|
| BCM → all | 0x17330810 | BAP_Innenlicht_FSG_01 |
| MIB → BCM | 0x17330800 | BAP_Innenlicht_ASG_01 |

The BAP_Config should read `03 00 08 00 05 01`.

## Functions

The 2-byte functions are `[value, 01]`.

| FCT | Name | Size | Payload |
|---|---|---|---|
| 0x0D | SetFactoryDefault | 1 | bit 0 = restore defaults |
| 0x0E | FSG-Setup | 41 | the zone layout, below |
| 0x10 | Colour | 2 | colour 1–6, for cars without RGB |
| 0x11 | AmbianceLighting | 2 | 0 off, 1 on, 2 on and follows the drive profile, 3 privacy |
| 0x15 | Brightness | 3 | `[on/off, level 0–100, 01]` |
| 0x16 | IlluminationSet1 | 2 | level 0–100, **all zones** |
| 0x17–0x1D | IlluminationSet2–8 | 2 | level 0–100, one zone each |
| 0x1E | IntLightConfig | 14 | Get only, which colours and light points are available |
| 0x20 | AmbientLightColor | 4 | `[R, G, B, 01]`, the current colour |
| 0x2A | RGBColorList | Array | 30 colours |

The FunctionList is masked by coding, so a real car announces fewer functions than the table has.

## Zones (0x0E)

One byte of flags and then eight 5-byte slots:

```plaintext
[flags] [id m0 m1 m2 00] x 8
```

- **Slot k is zone k**, and its brightness is FCT `0x16 + k − 1`. A slot with id `FF` doesn't exist.
- **flags** bit 0 = "automatic" is available, bit 1 = "privacy" is available.
- **m0–m2** is a bitmap of the light points in that zone.

| id | Zone | id | Zone |
|---|---|---|---|
| 01 | roof front | 08 | doors front |
| 02 | roof rear | 09 | rear doors + rear footwell |
| 03 | footwell front | **10** | **all zones** |
| 04 | footwell rear | 11 | footwell front + rear |
| 05 | footwell front + rear | 12 | front |
| 06 | cockpit | 14 | roof front + rear |
| 07 | doors front + rear | 15 | centre console |

Slot 1 is normally id `10`. Without it there's no ambient menu. If you're following the ambient colour from another device, 0x20 bytes 0–2 is the colour and 0x17 up are the levels. 0x11 isn't an on/off bit: 1, 2 and 3 all mean on.

Related: [LSG Index](README.md) · [CAN ID Map](../platforms/can-id-map.md) · [Generic Functions](../protocol/generic-functions.md)
