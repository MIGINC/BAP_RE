# CAN ID Map

Which CAN ID carries which LSG, and who is on each end. The raw tables are [data/can_map_pq.csv](../../data/can_map_pq.csv) and [data/can_map_mqb.csv](../../data/can_map_mqb.csv). Corrections are welcome as PRs against those.

**Tested with cluster** means an emulated module on that ID was tested against a real or emulated cluster or MIB2. **Logged in car** means the ID is in a capture from a real car, carrying that LSG. For an ID with several LSGs it means at least one of them. **Untested** means neither yet.

IDs can differ between module software versions, and some depend on which bus the module sits on. Bit rates are on [Connect and Listen Safely](connect-and-listen.md).

## PQ (11-bit)

### FSG channels (function modules transmitting)

| CAN ID | Name | LSG | Sender | Peer ID | Peer | Status | Notes |
|---|---|---|---|---|---|---|---|
| `0x66E` | BAP_SHZ | 0x03 | Aux heater (Standheizung) | `0x6C9` | Cluster | Tested with cluster | ICAN |
| `0x674` | BAP_ACC | 0x05, 0x1B | ACC | `0x676` | Cluster | Tested with cluster, Logged in car | ACAN or ECAN. The cluster needs a coding bit before it enables ACC |
| `0x6DD` | BAP_RDK | 0x07 | RDK (tyre pressure) | `0x6C8` | Cluster | Tested with cluster, Logged in car | KCAN or ECAN |
| `0x6DF` |  | 0x08, 0x09, 0x0C | Light / body | `0x6C8` | Cluster | Tested with cluster, Logged in car | KCAN |
| `0x6DA` | BAP_OPS | 0x0A | Parking (PDC / PLA) | `0x676` | Cluster | Tested with cluster, Logged in car | ACAN, also forwarded to ICAN, where the radio asks on 0x67A |
| `0x6DE` |  | 0x0D, 0x0E | Comfort settings | `0x6C8` | Cluster | Tested with cluster, Logged in car | KCAN |
| `0x677` |  | 0x17, 0x22 | ? (0x22 = driver alert) | `0x6C9` | Cluster | Tested with cluster | 0x17 is the drive profile, 0x22 the driver alert. Only the cluster's requests are logged |
| `0x673` | BAP_LDW | 0x19 | Lane Assist (HCA) | `0x676` | Cluster | Tested with cluster, Logged in car | ACAN or ECAN |
| `0x672` | BAP_SWA | 0x1A | Side Assist (SWA) | `0x676` | Cluster | Tested with cluster, Logged in car | ECAN, also forwarded to ICAN on later gateways, where the radio asks on 0x61D |
| `0x679` | BAP_HYB | 0x1D | Hybrid / e-Golf motor (probably) | `0x676` | Cluster | Tested with cluster | ACAN, also forwarded to ICAN. 0x1D as hybrid is a guess |
| `0x69B` | BAP_VZA | 0x21 | VZA (sign recognition camera) | `0x676` | Cluster | Tested with cluster, Logged in car | ECAN, also forwarded to ICAN. The cluster asks on 0x676, the radio on 0x67A |
| `0x66F` | BAP_Telefon_04 | 0x2B | UHV phone module, or the MIB2 PQ itself (Secondary Display) | `0x67C` | Cluster | Tested with cluster, Logged in car | ICAN. A MIB2 PQ answers this itself |
| `0x66C` | BAP_AUDIO | 0x31 | Radio / MIB2 PQ (audio to cluster) | `0x67C` | Cluster | Tested with cluster, Logged in car | ICAN. The radio is the FSG here |
| `0x67D` | BAP_NAVI | 0x32 | Compass module / nav, or the MIB2 PQ itself | `0x67C` | Cluster | Tested with cluster, Logged in car | ICAN. A MIB2 PQ answers this itself |
| `0x6D2` | BAP_Telefon_01 | 0x28 | UHV phone module (Telephone) | `0x6C7` | Radio / RNS | Untested |  |
| `0x6D5` | BAP_Telefon_02 | 0x29 | UHV phone module (Phonebook) | `0x6C6` | Radio / RNS | Untested | the phonebook splits its functions across 0x6D5 and 0x63B |
| `0x63B` | BAP_Telefon_03 | 0x29 | UHV phone module (Phonebook) | `0x6C6` | Radio / RNS | Untested | the phonebook splits its functions across 0x6D5 and 0x63B |
| `0x63C` | BAP_MDI | 0x2F | MDI box | `0x6C6` | Radio / RNS | Untested |  |
| `0x62C` | BAP_MDI_02 | 0x30 | UHV phone module (MDI) | `0x6C6` | Radio / RNS | Untested |  |
| `0x6DC` | BAP_RV | 0x0B | Rear view camera | `0x6CA` | Radio / RNS | Tested with cluster, Logged in car | ICAN: the radio asks on 0x6CA. KCAN: it asks on 0x6D9 through the gateway |
| `0x6D3` | BAP_TV_Tuner |  | TV tuner | `0x6C7` | Radio / RNS | Untested | peer not confirmed; may reuse the MDI layout |
| `0x6D8` | BAP_DSP | 0x2D | Amplifier (DSP) | `0x6C7` | Radio / RNS | Tested with cluster | only the radio's requests are logged |
| `0x6DB` | BAP_Clima | 0x01, 0x02 | Climatronic | `0x6CB` | Radio / RNS / MIB | Tested with cluster, Logged in car | KCAN, forwarded to ICAN. Carries two LSGs, only 0x01 is in my car logs |
| `0x6E8` | BAP_BC |  | Cluster (board computer) | `0x6EE` | RNS midline | Untested | LSG unknown |
| `0x678` | BAP_OPS_02 | 0x0A | Parking (two functions of LSG 0x0A) | `0x67A` | Radio / RNS | Logged in car | two functions of LSG 0x0A arrive here, the rest on 0x6DA |
| `0x66A` |  | 0x33 | eCall module | `0x6B7` | MIB2 PQ | Untested |  |
| `0x6FD` |  | 0x37 | ENI (online destinations / OCU) | `0x6B7` | RNS / MIB2 PQ | Untested |  |

### ASG channels (displays asking)

| CAN ID | Name | LSG | Sender | Peer ID | Peer | Status | Notes |
|---|---|---|---|---|---|---|---|
| `0x6D9` | BAP_Anzeige_RV | 0x0B | Radio / RNS |  | Rear view camera (on KCAN) | Untested | request ID for a camera on KCAN |
| `0x61D` | BAP_ASG_0E | 0x1A | Radio / RNS / MIB |  | Side Assist | Logged in car | ICAN to ECAN through the gateway |
| `0x676` | BAP_Anzeige | 0x05, 0x0A, 0x19, 0x1A, 0x1B, 0x1D, 0x21 | Cluster |  | driver-assist FSGs | Tested with cluster, Logged in car | forwarded to ACAN and ECAN |
| `0x67C` | BAP_ASG_07 | 0x2B, 0x31, 0x32 | Cluster |  | infotainment FSGs | Tested with cluster, Logged in car | forwarded to ICAN |
| `0x6C8` | BAP_ASG_01 | 0x07, 0x08, 0x09, 0x0C, 0x0D, 0x0E | Cluster |  | comfort FSGs | Tested with cluster, Logged in car | forwarded to KCAN and ECAN |
| `0x6C9` | BAP_ASG_02 | 0x03, 0x17, 0x22 | Cluster |  | aux heater and others | Tested with cluster, Logged in car | forwarded to ICAN |
| `0x67A` | BAP_Anzeige_02 | 0x0A, 0x1D, 0x21 | Radio / RNS |  | PDC; hybrid | Logged in car | forwarded from ICAN to ACAN, KCAN and ECAN. It's the radio asking, not a cluster |
| `0x6C5` | BAP_ASG_08 |  | Radio / RNS |  | Amplifier | Untested | the MIB2 PQ asks the amplifier on 0x6C7 instead |
| `0x6C6` | BAP_ASG_06 | 0x29, 0x2F, 0x30 | Radio / RNS |  | MDI; UHV | Untested |  |
| `0x6C7` | BAP_ASG_03 | 0x28, 0x2D | Radio / RNS |  | UHV; TV; amplifier | Tested with cluster, Logged in car |  |
| `0x6CA` | BAP_ASG_04 | 0x0B | Radio / RNS |  | Rear view camera | Tested with cluster, Logged in car | doesn't cross the gateway |
| `0x6CB` | BAP_ASG_05 | 0x01, 0x02 | Radio / RNS / MIB |  | Climatronic | Tested with cluster, Logged in car | ICAN to KCAN through the gateway |
| `0x6EE` | BAP_ASG_09 |  | RNS midline |  | Cluster (board computer) | Untested | ICAN to the cluster through the gateway |
| `0x6B7` |  | 0x33, 0x37 | RNS / MIB2 PQ |  | eCall; ENI | Untested |  |

### Others

| CAN ID | Name | LSG | Sender | Peer ID | Peer | Status | Notes |
|---|---|---|---|---|---|---|---|
| `0x6EB` | BAP_RDK_02 |  |  |  |  | Untested | never seen |
| `0x6D6` | BAP_Debug_Cmd |  | BAP tester |  | many modules | Untested | listed on many modules, no use found for it |
| `0x6D7` | BAP_Debug_Data |  | modules |  | BAP tester | Untested | listed on many modules, no use found for it |

## MQB (29-bit)

Most of these came from the community table (see [MQB notes](mqb.md)) and I have no MQB car to check them on.

| CAN ID | Name | LSG | Sender | Peer ID | Peer | Status | Notes |
|---|---|---|---|---|---|---|---|
| `0x17333310` | BAP_eCall_FSG_01 | 0x33 | Gateway (eCall) | `0x17333300` | MIB2 | Untested | StatusAll, 146 bytes |
| `0x17333300` | BAP_eCall_ASG_01 | 0x33 | MIB2 | `0x17333310` | Gateway (eCall) | Untested | GetAll, 1C C1 |
| `0x17330110` | Klima1 FSG | 0x01 | Climatronic | `0x17330100` | MIB2 | Untested |  |
| `0x17330100` | Klima1 ASG | 0x01 | MIB2 | `0x17330110` | Climatronic | Untested |  |
| `0x17330210` | Klima2 FSG | 0x02 | Climatronic (second) | `0x17330200` | MIB2 | Untested |  |
| `0x17330200` | Klima2 ASG | 0x02 | MIB2 | `0x17330210` | Climatronic (second) | Untested |  |
| `0x17330310` | Webasto FSG | 0x03 | Aux heater | `0x17330300` | MIB2 | Untested |  |
| `0x17330300` | Webasto ASG | 0x03 | MIB2 | `0x17330310` | Aux heater | Untested |  |
| `0x17330710` | RDK FSG | 0x07 | RDK (tyre pressure) | ? | ? | Untested | From the Audi A3 project on the 0x07 page |
| `0x17330810` | Innenlicht FSG | 0x08 | BCM (interior light) | `0x17330800` | MIB2 | Untested |  |
| `0x17330800` | Innenlicht ASG | 0x08 | MIB2 | `0x17330810` | BCM | Untested |  |
| `0x17330910` | Aussenlicht FSG | 0x09 | BCM (exterior light) | `0x17330900` | MIB2 | Untested |  |
| `0x17330900` | Aussenlicht ASG | 0x09 | MIB2 | `0x17330910` | BCM | Untested |  |
| `0x17330C10` | Wiper FSG | 0x0C | BCM (wipers) | `0x17330C00` | MIB2 | Untested |  |
| `0x17330C00` | Wiper ASG | 0x0C | MIB2 | `0x17330C10` | BCM | Untested |  |
| `0x17330D10` | Doorlocking FSG | 0x0D | BCM (central locking) | `0x17330D00` | MIB2 | Untested |  |
| `0x17330D00` | Doorlocking ASG | 0x0D | MIB2 | `0x17330D10` | BCM | Untested |  |
| `0x17330D01` | Doorlocking ASG (Kombi) | 0x0D | Kombi | `0x17330D10` | BCM | Untested |  |
| `0x17330D02` | Doorlocking ASG (FCM/OCU) | 0x0D | FCM / OCU | `0x17330D10` | BCM | Untested |  |
| `0x17330D04` | Doorlocking ASG (HUD) | 0x0D | HUD | `0x17330D10` | BCM | Untested |  |
| `0x17330E10` | Mirror FSG | 0x0E | BCM (mirrors) | `0x17330E00` | MIB2 | Untested |  |
| `0x17330E00` | Mirror ASG | 0x0E | MIB2 | `0x17330E10` | BCM | Untested |  |
| `0x17330A10` | OPS FSG | 0x0A | Parking | `0x17330A00` | MIB2 | Untested |  |
| `0x17330A00` | OPS ASG | 0x0A | MIB2 | `0x17330A10` | Parking | Untested |  |
| `0x17330B10` | RVC FSG | 0x0B | Rear view camera | `0x17330B00` | MIB2 | Untested |  |
| `0x17330B00` | RVC ASG | 0x0B | MIB2 | `0x17330B10` | Rear view camera | Untested |  |
| `0x17331010` | Sitz FSG | 0x10 | Seat module | `0x17331000` | MIB2 | Untested |  |
| `0x17331000` | Sitz ASG | 0x10 | MIB2 | `0x17331010` | Seat module | Untested |  |
| `0x17331110` | TimeDate FSG 01 | 0x11 | Cluster | `0x17331100` | MIB2 | Untested |  |
| `0x17331100` | TimeDate ASG 01 | 0x11 | MIB2 | `0x17331110` | Cluster | Untested |  |
| `0x17331310` | Einheiten FSG | 0x13 | Cluster (units) | `0x17331300` | MIB2 | Untested |  |
| `0x17331300` | Einheiten ASG | 0x13 | MIB2 | `0x17331310` | Cluster | Untested |  |
| `0x17331A10` | SWA FSG | 0x1A | Side Assist | `0x17331A00` | Cluster; MIB2 | Untested |  |
| `0x17331A00` | SWA ASG_01 | 0x1A | MIB2 | `0x17331A10` | Side Assist | Untested |  |
| `0x17331A01` | SWA ASG_02 | 0x1A | Cluster | `0x17331A10` | Side Assist | Untested |  |
| `0x17331F10` | RGS FSG 01 | 0x1F | Belt pretensioner | `0x17331F00` | MIB2 | Untested |  |
| `0x17331F00` | RGS ASG 01 | 0x1F | MIB2 | `0x17331F10` | Belt pretensioner | Untested |  |
| `0x17332110` | VZA FSG | 0x21 | Sign recognition | `0x17332100` | ? | Untested |  |
| `0x17332100` | VZA ASG | 0x21 | ? | `0x17332110` | Sign recognition | Untested |  |

Related: [PQ CAN Channels](pq-can-channels.md) · [PQ Network Map](pq-network-map.md) · [LSG Index](../lsg/README.md)
