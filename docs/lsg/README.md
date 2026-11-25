# LSG Index

Every LSG I have something on. IDs are PQ, FSG / ASG. The DF links go to the [function catalogue](../catalogue/README.md). "Menu" is what appears on a Passat B7 cluster when an emulated FSG for that LSG is running.

| LSG | Name | What | PQ IDs | DF | Menu on the cluster | Status |
|---|---|---|---|---|---|---|
| 0x01 | Klima1 | climate, front | `0x6DB` / `0x6CB` | [3.0](../catalogue/lsg01-klima1-df3.0.md), [3.3](../catalogue/lsg01-klima1-df3.3.md) |  | Tested with cluster, Logged in car |
| 0x02 | Klima2 | climate, rear | `0x6DB` / `0x6CB` | [3.0](../catalogue/lsg02-klima2-df3.0.md), [3.3](../catalogue/lsg02-klima2-df3.3.md) |  | Simulated (application layer) |
| 0x03 | ParkHeater | aux heater (Standheizung) | `0x66E` / `0x6C9` | [3.0](../catalogue/lsg03-parkheater-df3.0.md) | *Auxiliary heater* page | Tested with cluster |
| 0x05 | ACC | adaptive cruise | `0x674` / `0x676` | [3.0](../catalogue/lsg05-acc-df3.0.md), [3.4](../catalogue/lsg05-acc-df3.4.md) | Settings row | Tested with cluster, Logged in car |
| 0x07 | RDK | tyre pressure | `0x6DD` / `0x6C8` | [3.0](../catalogue/lsg07-rdk-df3.0.md), [4.0](../catalogue/lsg07-rdk-df4.0.md), [4.3](../catalogue/lsg07-rdk-df4.3.md) | tyre pages | Tested with cluster, Logged in car |
| 0x08 | Innenlicht | interior light | `0x6DF` / `0x6C8` | [3.0](../catalogue/lsg08-innenlicht-df3.0.md), [5.1](../catalogue/lsg08-innenlicht-df5.1.md) | Settings row (Light & Vision) | Tested with cluster, Logged in car |
| 0x09 | Aussenlicht | exterior light | `0x6DF` / `0x6C8` | [4.3](../catalogue/lsg09-aussenlicht-df4.3.md), [5.4](../catalogue/lsg09-aussenlicht-df5.4.md) | Settings row (Light & Vision) | Tested with cluster, Logged in car |
| 0x0A | OPS | parking sensors | `0x6DA` / `0x676`<br>`0x678` / `0x67A` | [3.1](../catalogue/lsg0A-ops-df3.1.md), [3.3](../catalogue/lsg0A-ops-df3.3.md), [4.1](../catalogue/lsg0A-ops-df4.1.md), [4.4](../catalogue/lsg0A-ops-df4.4.md) | Settings row, DF 4.1 only | Tested with cluster, Logged in car |
| 0x0B | RVC | rear view camera | `0x6DC` / `0x6CA` | [3.1](../catalogue/lsg0B-rvc-df3.1.md), [5.1](../catalogue/lsg0B-rvc-df5.1.md), [5.3](../catalogue/lsg0B-rvc-df5.3.md), [5.5](../catalogue/lsg0B-rvc-df5.5.md) |  | Tested with cluster, Logged in car |
| 0x0C | Wiper | wipers | `0x6DF` / `0x6C8` | [3.0](../catalogue/lsg0C-wiper-df3.0.md), [3.1](../catalogue/lsg0C-wiper-df3.1.md) |  | Tested with cluster |
| 0x0D | Doorlocking | central locking | `0x6DE` / `0x6C8` | [3.3](../catalogue/lsg0D-doorlocking-df3.3.md), [4.5](../catalogue/lsg0D-doorlocking-df4.5.md) | Settings row | Tested with cluster, Logged in car |
| 0x0E | Mirror | mirrors | `0x6DE` / `0x6C8` | [3.0](../catalogue/lsg0E-mirror-df3.0.md), [3.1](../catalogue/lsg0E-mirror-df3.1.md) | Settings row | Tested with cluster, Logged in car |
| 0x10 | Sitz1 | driver seat |  | [4.0](../catalogue/lsg10-sitz1-df4.0.md), [4.5](../catalogue/lsg10-sitz1-df4.5.md) |  | Simulated (application layer) |
| 0x17 | Charisma | drive profile | `0x677` / `0x6C9` | [3.1](../catalogue/lsg17-charisma-df3.1.md) |  | Tested with cluster |
| 0x19 | LDW | Lane Assist | `0x673` / `0x676` | [4.0](../catalogue/lsg19-ldw-df4.0.md), [4.1](../catalogue/lsg19-ldw-df4.1.md) | Settings row + *Assist systems* page | Tested with cluster, Logged in car |
| 0x1A | SWA | Side Assist / blind spot | `0x672` / `0x676` | [3.0](../catalogue/lsg1A-swa-df3.0.md), [3.1](../catalogue/lsg1A-swa-df3.1.md), [3.4](../catalogue/lsg1A-swa-df3.4.md) | Settings row + *Assist systems* page | Tested with cluster, Logged in car |
| 0x1B | AWV | Front Assist | `0x674` / `0x676` | [3.0](../catalogue/lsg1B-awv-df3.0.md), [3.2](../catalogue/lsg1B-awv-df3.2.md) | Settings row + *Assist systems* page | Tested with cluster, Logged in car |
| 0x1D | LSG_1D | hybrid? | `0x679` / `0x676` | [3.2](../catalogue/lsg1D-lsg_1d-df3.2.md), [3.3](../catalogue/lsg1D-lsg_1d-df3.3.md) |  | Tested with cluster |
| 0x20 | Sitz2 | passenger seat |  | [4.0](../catalogue/lsg20-sitz2-df4.0.md), [4.5](../catalogue/lsg20-sitz2-df4.5.md) |  | Untested |
| 0x21 | VZA | traffic signs | `0x69B` / `0x676` | [3.0](../catalogue/lsg21-vza-df3.0.md), [3.3](../catalogue/lsg21-vza-df3.3.md) | *Sign Assist* page | Tested with cluster, Logged in car |
| 0x22 | MKE | driver alert (fatigue) | `0x677` / `0x6C9` | [3.0](../catalogue/lsg22-mke-df3.0.md) | *Assist systems* page | Tested with cluster |
| 0x28 | Telefon | telephone | `0x6D2` / `0x6C7` |  |  | Untested |
| 0x29 | Telefon2 | phonebook | `0x6D5` / `0x6C6`<br>`0x63B` / `0x6C6` |  |  | Untested |
| 0x2B | UHV_SecondaryDisplay | phone text on the cluster | `0x66F` / `0x67C` | [3.1](../catalogue/lsg2B-uhv_secondarydisplay-df3.1.md) | phone page | Tested with cluster, Logged in car |
| 0x2D | Amplifier | amplifier | `0x6D8` / `0x6C7` | [3.0](../catalogue/lsg2D-amplifier-df3.0.md), [3.1](../catalogue/lsg2D-amplifier-df3.1.md) |  | Tested with cluster |
| 0x2F | ? | MDI box | `0x63C` / `0x6C6` |  |  | Untested |
| 0x30 | ? | MDI in the phone module | `0x62C` / `0x6C6` |  |  | Untested |
| 0x31 | Audio | audio page on the cluster | `0x66C` / `0x67C` | [3.1](../catalogue/lsg31-audio-df3.1.md) | *Audio* page | Tested with cluster, Logged in car |
| 0x32 | Navigation | compass / navigation | `0x67D` / `0x67C` | [3.0](../catalogue/lsg32-navigation-df3.0.md) | *Navigation* page + Settings row | Tested with cluster, Logged in car |
| 0x33 | eCall | emergency call | `0x66A` / `0x6B7` | [3.5](../catalogue/lsg33-ecall-df3.5.md) |  | Simulated (application layer) |
| 0x37 | ENI | online destinations | `0x6FD` / `0x6B7` | [3.3](../catalogue/lsg37-eni-df3.3.md) |  | Untested |

- **Tested with cluster**: tested against a real or emulated cluster or MIB2. It doesn't mean a menu appears, that's the Menu column. 0x01, 0x0B and 0x2D were tested with a MIB2, the rest with a cluster.
- **Logged in car**: FSG traffic for that LSG is in a car capture.
- **Simulated (application layer)**: values put into an emulated MIB2 above BAP, and decoded. Says what a value means, says nothing about framing, versions, timing or CAN IDs. Only for some functions and DFs, marked Sim on the catalogue page.
- **Untested**: none of these yet, treat it as a lead.

Only the IDs shown are covered by the status. Driver-assist modules answer more than one display, see the [CAN ID Map](../platforms/can-id-map.md).

"SWA" is *Spurwechselassistent*, the lane-*change* assistant (Side Assist). The first version of this repo had it as Lane Assist, which is 0x19.

## Extra conditions

Some LSGs need more than a talking FSG (the cluster side is tested with cluster):

- **ACC (0x05 / 0x1B)** needs a bit in the cluster's coding
- **VZA (0x21)** needs the camera to report its type on plain CAN 0x5BE
- **0x1D** needs its own coding bit
- **Phone display (0x2B)** needs no cluster coding, but in a car the gateway has to be coded for it

The gateway installation list on CAN (0x35F) doesn't gate any of them in the cluster.

## One device, several LSGs

The phone module alone hosts 0x28, 0x29, 0x2B and 0x30. The other way round, 0x2F and 0x30 are two different boxes using the same MDI function layout. So "supporting a car" can mean several near-identical LSGs that differ only in their ID and CAN pair.

## MQB

On MQB the LSG is in the 29-bit ID, see [MQB notes](../platforms/mqb.md). These are the other LSGs that show up there. I have names but no functions, and all are untested:

| LSG | Name | LSG | Name | LSG | Name |
|---|---|---|---|---|---|
| 0x04 | HUD | 0x23 | BCmE | 0x39 | RangeData |
| 0x0F | BC (board computer) | 0x25 | BatteryControl | 0x3A | PedAssist |
| 0x11 | Uhrzeit (time / date) | 0x26 | Brake | 0x3C | EfficiencyAssist |
| 0x12 | SIA (service interval) | 0x27 | StartStopReasons | 0x42 | KlimaMaster |
| 0x13 | Einheiten (units) | 0x2A | OnlineFunctions | 0x45 | DisplayConfig |
| 0x14 | Homelink | 0x34 | MFL | 0x46 | SDS |
| 0x1C | Hybrid | 0x35 | SpecialFunction | 0x50 | InfoSettings |
| 0x1F | RGS (belt pretensioner) | 0x38 | ExBox | 0x59 | Doors |

LSG IDs above 0x3F don't fit the 6-bit LSG field in the BAP header, so I don't know how 0x42, 0x45, 0x46, 0x50 and 0x59 are carried.

On PQ the cluster's clock and unit settings go out on plain CAN (0x60E), not BAP, so 0x11 and 0x13 look MQB-only.

Related: [CAN ID Map](../platforms/can-id-map.md) · [PQ CAN Channels](../platforms/pq-can-channels.md) · [Function catalogue](../catalogue/README.md)
