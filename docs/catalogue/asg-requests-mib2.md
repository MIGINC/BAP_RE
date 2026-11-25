# ASG requests (MIB2)

The rest of the catalogue is what the FSG sends. This page is the other direction: what one ASG, a MIB2, writes when a setting changes. It isn't a list of everything an ASG supports, and another ASG (the cluster) may write something else. Raw samples are in [data/asg_requests_mib2.json](../../data/asg_requests_mib2.json).

- **Simulated (application layer)**, except 0x0A and 0x0B, where the request went out on CAN (tested with cluster). This says what it *can* send, not that a menu uses it.
- `b0` is the first payload byte, `b0.4` is bit 4 of it. *name* = the value as it is, `1 → 01 00` = a value and the bytes it gave. Names are descriptions.
- **Idle** is the payload with every argument 0 or off. Bytes that never move in a row aren't shown as fields, they're usually 00 from the MIB2.
- Some functions are read-modify-write: the MIB2 sends back the last value with one bit changed (mirrors 0x0E/0x10, locking 0x0D/0x11, seat 0x10/0x17, parking 0x0A/0x10 and 0x11, units 0x13/0x12 and 0x15). Idle then shows bits left over from the call before.
- **Factory default** is 0x0D `01`, or `11` where 0x0D is also the on/off switch (bit 4). AWV sent `03`, and ACC uses 0x1B.
- DF `?` means I only have the function numbers, with no real module's version behind them. An LSG or function that's missing is no evidence that the MIB2 doesn't write it.
- A function can be here and not on the catalogue page for that DF: the MIB2 writes it, but it isn't on that page (0x08/0x2C, 0x0C/0x11, 0x10/0x0D, most of 0x17, 0x1D/0x18, 0x21/0x14 and 0x16). And 0x1D/0x14 is read-only on its page.
- Array requests are left out, see [MIB2 Results](../protocol/mib2.md#asg_id-and-taid).

## 0x01 Klima1, DF [3.3](lsg01-klima1-df3.3.md)

| FCT | Function | Op | Bytes | Idle | Fields |
|---|---|---|---|---|---|
| 0x0D | Aircon set factory default master | SetGet | 1 | `01` |  |
| 0x0D | Aircon system on off row | SetGet | 1 | `01` | b0.4 state |
| 0x11 | Aircon air circulation auto | SetGet | 1 | `00` | b0.1 air circulation auto |
| 0x13 | Aircon solar | SetGet | 1 | `00` | b0.0 solar |
| 0x15 | Aircon synchronisation | SetGet | 2 | `00 00` | b0 = master<br>b1.0 slave zone left row 1<br>b1.1 slave zone right row 1<br>b1.2 slave zone left row 2<br>b1.3 slave zone right row 2<br>b1.4 slave zone left row 3<br>b1.5 slave zone right row 3 |
| 0x1A | Aircon popup time | SetGet | 1 | `00` | b0 = popup time |
| 0x1B | Aircon temp zone | SetGet | 2 | `00 00` | b0 = temp value<br>temp unit: 1 → `00 01` |
| 0x1C | Aircon air volume | SetGet | 1 | `00` | b0 = air volume |
| 0x1D | Aircon air volume | SetGet | 1 | `00` | b0 = air volume regulated |
| 0x1E | Aircon air volume | SetGet | 4 | `00 00 00 00` | air volume auto: 1 → `00 00 00 02`, 2 → `00 00 00 02`, 10 → `00 00 00 02` |
| 0x1E | Aircon air distribution | SetGet | 4 | `00 00 00 00` | b0 = up<br>b1 = body<br>b2 = footwell<br>b3.0 automode |
| 0x1F | Aircon seat heater | SetGet | 1 | `00` | b0 = heater level |
| 0x20 | Aircon seat ventilation | SetGet | 1 | `00` | b0 = ventilation level |
| 0x27 | Aircon rear control | SetGet | 1 | `00` | b0.0 rear control |
| 0x29 | Aircon residual heat | SetGet | 1 | `00` | b0.0 heating |
| 0x2A | Aircon heater | SetGet | 2 | `00 00` | b0.0 heater |
| 0x2D | Aircon front window heater | SetGet | 2 | `00 00` | b0.0 front window heater |
| 0x2E | Aircon defrost | SetGet | 1 | `00` | b0.0 defrost |
| 0x2F | Aircon suppress visualisation | SetGet | 1 | `00` | b0.0 suppress visualisation |
| 0x30 | Aircon blower compensation | SetGet | 1 | `00` | b0.0 sds |
| 0x31 | Aircon AC | SetGet | 1 | `00` | b0.0 ac |

## 0x05 ACC, DF [3.4](lsg05-acc-df3.4.md)

| FCT | Function | Op | Bytes | Idle | Fields |
|---|---|---|---|---|---|
| 0x10 | ACC gong state | SetGet | 1 | `00` | b0.0 enable |
| 0x11 | ACC gong volume | SetGet | 1 | `00` | b0 = volume |
| 0x12 | ACC default mode | SetGet | 1 | `00` | ACC default mode: 1 → `01` |
| 0x13 | ACC time gap | SetGet | 2 | `00 00` | b0 = ACC time gap |
| 0x14 | ACC driving program | SetGet | 2 | `00 00` | b0 = program |
| 0x15 | ACC curve assist | SetGet | 2 | `00 00` | b0.0 state |
| 0x16 | ACC speed limit adoption | SetGet | 2 | `00 00` | b0.0 state |
| 0x17 | ACC speed limit offset | SetGet | 3 | `00 00 00` | b0 = offset |
| 0x18 | ACC traffic jam assist | SetGet | 2 | `00 00` | b0.0 state |
| 0x19 | ACC distance warning | SetGet | 3 | `00 00 00` | b0.0 system state<br>b1 = time gap |
| 0x1B | ACC set factory default | SetGet | 1 | `01` |  |

## 0x06 Suspension, DF ?

| FCT | Function | Op | Bytes | Idle | Fields |
|---|---|---|---|---|---|
| 0x10 | Suspension control active profile | SetGet | 3 | `00 00 00` | b0 = profile |
| 0x11 | Suspension control trailer mode | SetGet | 2 | `00 00` | b0.0 active |
| 0x12 | Suspension control car jack mode | SetGet | 2 | `00 00` | b0.0 active |
| 0x13 | Suspension control loading mode | SetGet | 2 | `00 00` | b0.0 active |
| 0x15 | Suspension control lift mode | SetGet | 2 | `00 00` | b0.0 active |

## 0x07 RDK, DF [4.3](lsg07-rdk-df4.3.md)

| FCT | Function | Op | Bytes | Idle | Fields |
|---|---|---|---|---|---|
| 0x0D | RDK system on off | SetGet | 1 | `00` | b0.4 enable |
| 0x16 | RDK tire changed | StartResult | 0 |  |  |
| 0x17 | RDK tire setup selected tire | StartResult | 2 | `00 00` | b0 = position |
| 0x17 | RDK speed limit | StartResult | 2 | `FF 00` | b1 = speed limit |
| 0x17 | RDK pressure level | StartResult | 2 | `FF 00` | b1 = level |

## 0x08 Innenlicht, DF [5.1](lsg08-innenlicht-df5.1.md)

| FCT | Function | Op | Bytes | Idle | Fields |
|---|---|---|---|---|---|
| 0x0D | Int light set factory default | SetGet | 1 | `01` |  |
| 0x10 | Int light colour | SetGet | 2 | `01 00` | colour: 1 → `02 00`, 2 → `03 00`, 3 → `04 00`, 4 → `05 00`, 5 → `06 00` |
| 0x11 | Int light state | SetGet | 2 | `00 00` | b0 = state |
| 0x12 | Int light environment | SetGet | 2 | `00 00` | b0.0 environment |
| 0x13 | Int light temperature | SetGet | 2 | `00 00` | b0.0 enable |
| 0x14 | Int light speed | SetGet | 2 | `00 00` | b0.0 speed |
| 0x15 | Int light brightness | SetGet | 3 | `00 00 00` | b1 = brightness<br>b0.0 state |
| 0x1F | Int light active profile | SetGet | 2 | `00 00` | b0 = profile |
| 0x20 | Int light ambient light color | SetGet | 4 | `00 00 00 00` | b0 = base color red<br>b1 = base color green<br>b2 = base color blue |
| 0x21 | Int light contour light color | SetGet | 4 | `00 00 00 00` | b0 = base color red<br>b1 = base color green<br>b2 = base color blue |
| 0x2B | Int light follow up time | SetGet | 2 | `00 00` | b0 = time |
| 0x2C | Int light door contact | SetGet | 2 | `18 00` | b0.0 int light door contact |

## 0x09 Aussenlicht, DF [5.4](lsg09-aussenlicht-df5.4.md)

| FCT | Function | Op | Bytes | Idle | Fields |
|---|---|---|---|---|---|
| 0x0D | Ext light set factory default | SetGet | 1 | `01` |  |
| 0x10 | Ext light day light | SetGet | 2 | `00 00` | b0.0 enable |
| 0x11 | Ext light tourist | SetGet | 2 | `00 00` | b0.0 enable |
| 0x12 | Ext light coming home | SetGet | 2 | `00 00` | b0 = time<br>b1.0 state |
| 0x13 | Ext light leaving home | SetGet | 2 | `00 00` | b0 = time<br>b1.0 state |
| 0x14 | Ext light switch on sensitivity | SetGet | 2 | `00 00` | b0 = config |
| 0x16 | Ext light motorway blinking | SetGet | 3 | `00 00 00` | b1 = time<br>b0.0 state |
| 0x18 | Ext light adaptive | SetGet | 2 | `00 00` | b0.0 enable |
| 0x19 | Ext light head light system | SetGet | 2 | `00 00` | b0.0 enable |
| 0x1A | Ext light gliding light system | SetGet | 2 | `00 00` | b0.0 enable |
| 0x1B | Ext light masked high beam | SetGet | 2 | `00 00` | b0.0 enable |
| 0x1D | Ext light automatic light | SetGet | 2 | `00 00` | b0.0 ext light auto rain<br>b0.1 ext light auto speed |
| 0x1E | Ext light laser light | SetGet | 2 | `00 00` | b0.0 enable |
| 0x1F | Ext light headlight range | SetGet | 2 | `00 00` | b0 = lowering |

## 0x0A OPS, DF [3.3](lsg0A-ops-df3.3.md)

| FCT | Function | Op | Bytes | Idle | Fields |
|---|---|---|---|---|---|
| 0x10 | PDC frequence front | SetGet | 2 |  | frequence: 1 → `01 00`, 2 → `02 00` |
| 0x10 | PDC volume front | SetGet | 2 |  | volume: 1 → `02 01`, 2 → `02 02` |
| 0x11 | PDC frequence rear | SetGet | 2 |  | frequence: 1 → `01 00`, 2 → `02 00` |
| 0x11 | PDC volume rear | SetGet | 2 |  | volume: 1 → `02 01`, 2 → `02 02` |
| 0x15 | PDC mute | SetGet | 1 | `00` | b0.0 mute |
| 0x17 | PDC default parking mode | SetGet | 1 | `00` | defaultparking mode: 1 → `01`, 2 → `01`, 3 → `01`, 15 → `01` |
| 0x19 | PDC system on off | SetGet | 1 | `00` | b0.4 state |
| 0x19 | Pdc set factory default | SetGet | 1 | `11` |  |
| 0x1A | PDC auto activation | SetGet | 2 | `00 00` | b0.0 auto activation |

## 0x0B RVC, DF [5.1](lsg0B-rvc-df5.1.md)

| FCT | Function | Op | Bytes | Idle | Fields |
|---|---|---|---|---|---|
| 0x0D | VPS system on off | SetGet | 1 | `00` | b0.4 state |
| 0x0D | VPS set factory default | SetGet | 1 | `11` |  |
| 0x10 | VPS follow up time | SetGet | 1 | `00` | b0 = time |
| 0x11 | VPS default mode RV | SetGet | 2 | `00 00` | b0 = vps screen<br>b1 = vps mode |
| 0x12 | VPS default mode FV | SetGet | 2 | `00 00` | b0 = vps screen<br>b1 = vps mode |
| 0x13 | VPS default mode SV | SetGet | 2 | `00 00` | b0 = vps screen<br>b1 = vps mode |
| 0x14 | VPS default mode BV | SetGet | 2 | `00 00` | b0 = vps screen<br>b1 = vps mode |
| 0x15 | VPS default view | SetGet | 1 | `01` | b0 = view |
| 0x17 | VPS color | SetGet | 2 | `00 00` | b0 = color |
| 0x18 | VPS contrast | SetGet | 2 | `00 00` | b0 = contrast |
| 0x19 | VPS brightness | SetGet | 2 | `00 00` | b0 = brightness |
| 0x1B | VPS dynamic parking mode | SetGet | 2 | `00 00` | b0.0 parkbox<br>b0.1 parallel to road<br>b0.2 offroad<br>b0.3 right sideview<br>b0.4 left sideview<br>b0.5 crossing<br>b0.6 trailer assist |
| 0x1C | VPS OPS overlay | SetGet | 2 | `00 00` | b0.0 rearview<br>b0.1 frontview<br>b0.2 sideview<br>b0.3 birdview |

## 0x0C Wiper, DF [3.1](lsg0C-wiper-df3.1.md)

| FCT | Function | Op | Bytes | Idle | Fields |
|---|---|---|---|---|---|
| 0x0D | Wiper set factory default | SetGet | 1 | `01` |  |
| 0x10 | Wiper service position | SetGet | 1 | `00` | b0.4 active |
| 0x11 | Wiper winter position | SetGet | 1 | `00` | b0.0 active |
| 0x12 | Wiper rear wiping | SetGet | 1 | `00` | b0.0 enable |
| 0x13 | Wiper tears wiping | SetGet | 1 | `00` | b0.0 enable |
| 0x14 | Wiper rain sensor on off | SetGet | 1 | `00` | b0.0 enable |
| 0x15 | Wiper rain sensor config | SetGet | 1 | `00` | b0 = rain sensor config |
| 0x16 | Easy entry steering column | SetGet | 2 | `00 00` | b0.0 enable |

## 0x0D Doorlocking, DF [4.5](lsg0D-doorlocking-df4.5.md)

| FCT | Function | Op | Bytes | Idle | Fields |
|---|---|---|---|---|---|
| 0x0D | Door locking set factory default | SetGet | 1 | `01` |  |
| 0x10 | Door locking comfort open settings | SetGet | 2 | `00 00` | b0.0 driver window<br>b0.1 codriver window<br>b0.2 driver rear window<br>b0.3 codriver rear window<br>b0.4 sun roof<br>b0.5 rear blind |
| 0x11 | Door locking cl boot open | SetGet | 2 | `00 00` | b0.0 boot open |
| 0x11 | Door locking unlocking mode | SetGet | 2 |  | mode: 1 → `01 00`, 2 → `01 01`, 3 → `01 02` |
| 0x11 | Door locking cl boot lock | SetGet | 2 | `01 02` | b0.1 boot lock |
| 0x12 | Door locking rain closing | SetGet | 1 | `00` | b0.0 rain closing |
| 0x13 | Door locking theft warning settings | SetGet | 2 | `00 00` | b0.0 interior<br>b0.1 decline |
| 0x14 | Door locking confirmation | SetGet | 1 | `00` | b0.0 confirmation |
| 0x15 | Door locking rear blind | SetGet | 1 | `00` | b0.0 rear blind button<br>b0.1 rear blind sun protection<br>b0.2 rear blind reverse gear |
| 0x16 | Door locking turn ind repeat | SetGet | 1 | `00` | b0.0 state |
| 0x17 | Door locking auto lock | SetGet | 2 | `00 00` | b0 = auto lock |
| 0x18 | Door locking auto unlock | SetGet | 1 | `00` | b0.4 auto unlock |
| 0x19 | Door locking mirror protection | SetGet | 1 | `00` | b0.0 mirror protection |
| 0x1D | Door locking boot open | SetGet | 1 | `00` | b0.0 boot open |
| 0x1E | Door locking boot close | SetGet | 1 | `00` | b0.0 boot close |
| 0x1F | Door locking remote lock unlock | StartResult | 6 | `00 00 00 00 00 00` | data: "A" → `01 41 00 00 00 00` |
| 0x1F | Door locking remote lock unlock | Abort | 0 |  |  |
| 0x21 | Door locking remote lock unlock signature | Set | 1,2 | `00` | data: "A" → `01 41` |
| 0x22 | Door locking remote horn | SetGet | 3 | `01 00 00` | b1 = time |
| 0x23 | Door locking remote blinking | SetGet | 3 | `01 00 00` | b1 = time |
| 0x25 | Door locking active user | SetGet | 1 | `00` | b0 = user reference |
| 0x26 | Door locking user profile on off | SetGet | 2 | `00 00` | b0.0 handling<br>b0.1 auto key assignment<br>b0.2 temporary key assignment |
| 0x27 | Door locking user profile control | StartResult | 4 | `21 00 00 00` | control: 2 → `22 00 00 00`, 3 → `23 00 00 00`, 4 → `24 00 00 00`, 5 → `25 00 00 00`, 255 → `FF 00 00 00`<br>b1 = target profile |
| 0x27 | Door locking user profile control | Abort | 0 |  |  |
| 0x29 | Door locking window auto close | SetGet | 1 | `00` | b0.0 control |
| 0x2A | Doorlocking blinds control | SetGet | 2 | `00 00` | control: 1 → `10 00`, 2 → `20 00`, 3 → `30 00` |

## 0x0E Mirror, DF [3.0](lsg0E-mirror-df3.0.md)

| FCT | Function | Op | Bytes | Idle | Fields |
|---|---|---|---|---|---|
| 0x0D | Mirror set factory default | SetGet | 1 | `01` |  |
| 0x10 | Mirror lowering | SetGet | 1 | `00` | b0.0 enable |
| 0x10 | Mirror sync adjust | SetGet | 1 | `01` | b0.1 enable |
| 0x10 | Mirror folding | SetGet | 1 | `03` | b0.2 enable |
| 0x10 | Mirror dimming | SetGet | 1 | `07` | b0.3 enable |
| 0x10 | Mirror heating | SetGet | 1 | `0F` | b0.4 enable |

## 0x10 Sitz1, DF [4.5](lsg10-sitz1-df4.5.md)

| FCT | Function | Op | Bytes | Idle | Fields |
|---|---|---|---|---|---|
| 0x0D | Seat set factory default | SetGet | 1 | `01` |  |
| 0x10 | Seat radio key automatic | SetGet | 2 | `00 00` | b0.0 state |
| 0x17 | Seat easy entry front left | SetGet | 1 | `00` | b0.0 state |
| 0x17 | Seat easy entry front right | SetGet | 1 | `01` | b0.1 state |

## 0x11 Uhrzeit, DF ?

| FCT | Function | Op | Bytes | Idle | Fields |
|---|---|---|---|---|---|
| 0x10 | Clock date | SetGet | 3 | `00 00 00` | b0 = year<br>b1 = month<br>b2 = day |
| 0x11 | Clock time | SetGet | 3 | `00 00 00` | b0 = hours<br>b1 = minutes<br>b2 = seconds |
| 0x11 | Clock time zone offset | SetGet | 3 | `00 00 00` | offset: 1.0 → `01 00 00`, 20.5 → `14 1E 00` |
| 0x13 | Clock day light saving | SetGet | 1 | `00` | b0.0 state |
| 0x18 | Clock GPS sync data | SetGet | 6 | `00 00 00 00 00 00` | b3 = hours<br>b4 = minutes<br>b5 = seconds<br>b0 = year<br>b1 = month<br>b2 = day |

## 0x13 Einheiten, DF ?

| FCT | Function | Op | Bytes | Idle | Fields |
|---|---|---|---|---|---|
| 0x0D | Um set factory default | SetGet | 1 | `01` |  |
| 0x11 | Temperature unit | SetGet | 1 | `00` | temperature unit: 1 → `01` |
| 0x12 | Distance unit | SetGet | 1 | `00` | distance unit: 1 → `10`, 15 → `F0` |
| 0x12 | Speed unit | SetGet | 1 | `F0` | speed unit: 1 → `F1` |
| 0x13 | Pressure unit | SetGet | 1 | `00` | b0 = pressure unit |
| 0x14 | Volume unit | SetGet | 1 |  | volume unit: 2 → `00`, 3 → `01`, 4 → `02` |
| 0x15 | Clock format | SetGet | 1 | `00` | clock format: 1 → `01` |
| 0x15 | Date format | SetGet | 1 | `01` | date format: 1 → `11`, 2 → `21` |
| 0x16 | Consumption petrol unit | SetGet | 1 | `02` | consumption petrol unit: 1 → `03`, 3 → `00`, 4 → `01`, 6 → `00`, 7 → `01` |
| 0x17 | Consumption gas unit | SetGet | 1 | `02` | consumption gas unit: 1 → `03`, 3 → `00`, 4 → `01`, 6 → `00`, 7 → `01` |
| 0x18 | Consumption electric unit | SetGet | 1 | `02` | consumption electric unit: 1 → `03`, 3 → `00`, 4 → `01`, 6 → `00`, 7 → `01` |
| 0x1A | Skin | SetGet | 2 | `00 00` | b0 = skin |
| 0x1B | Weight unit | SetGet | 2 | `00 00` | unit: 1 → `01 00` |

## 0x17 Charisma, DF [3.1](lsg17-charisma-df3.1.md)

| FCT | Function | Op | Bytes | Idle | Fields |
|---|---|---|---|---|---|
| 0x0D | Charisma set factory default | SetGet | 1 | `01` |  |
| 0x11 | Charisma active profile | SetGet | 1 | `01` | b0 = profile |
| 0x13 | Charisma trailer setting | SetGet | 3 | `00 00 00` | b1.0 trailer setting |
| 0x15 | Charisma prog button | SetGet | 1 | `00` | b0.0 button 1<br>b0.1 button 2<br>b0.2 button 3<br>b0.3 button 4<br>b0.4 button 5 |
| 0x16 | Charisma active operation mode | SetGet | 1 | `01` | b0 = operation mode |

## 0x19 LDW, DF [4.0](lsg19-ldw-df4.0.md)

| FCT | Function | Op | Bytes | Idle | Fields |
|---|---|---|---|---|---|
| 0x0D | LDW HCA set factory default | SetGet | 1 | `01` |  |
| 0x0D | LDW HCA system on off | SetGet | 1 | `00` | b0.4 state |
| 0x10 | LDW steering wheel vibration | SetGet | 2 | `00 00` | b0 = vibration |
| 0x11 | LDW warning time | SetGet | 2 | `00 00` | b0 = warning |
| 0x11 | HCA tolerance level | SetGet | 2 | `00 00` | b0 = HCA tolerance level |
| 0x12 | HCA intervention style | SetGet | 2 | `00 00` | b0 = HCA intervention style |

## 0x1A SWA, DF [3.1](lsg1A-swa-df3.1.md)

| FCT | Function | Op | Bytes | Idle | Fields |
|---|---|---|---|---|---|
| 0x10 | SWA frequency | SetGet | 1 | `00` | SWA frequency: 1 → `01` |
| 0x11 | SWA brightness | SetGet | 2 | `00 00` | b0 = brightness |
| 0x12 | SWA warning time | SetGet | 2 | `00 00` | b0 = time |
| 0x13 | SWA system | SetGet | 1 | `01` | system: 1 → `00` |
| 0x14 | SWA gong state | SetGet | 1 | `00` | b0.0 SWA gong state |
| 0x15 | SWA gong volume | SetGet | 1 | `00` | b0 = SWA gong volume |
| 0x17 | SWA RCTA | SetGet | 2 | `00 00` | b0.0 state |
| 0x19 | SWA exit assist | SetGet | 2 | `00 00` | b0.0 state |

## 0x1B AWV, DF 4.3

| FCT | Function | Op | Bytes | Idle | Fields |
|---|---|---|---|---|---|
| 0x0D | AWV system | SetGet | 1 | `00` | state: 1 → `01` |
| 0x0D | AWV set factory default | SetGet | 1 | `03` |  |
| 0x10 | AWV gong | SetGet | 1 | `00` | b0.0 AWV gong |
| 0x11 | AWV gong volume | SetGet | 1 | `00` | b0 = AWV gong volume |
| 0x12 | AWV warning | SetGet | 2 | `00 00` | b0.0 AWV warning |
| 0x13 | AWV brake jerk | SetGet | 1 | `00` | b0.0 AWV brake jerk |
| 0x14 | AWV emergency brake | SetGet | 1 | `00` | b0.0 target brake<br>b0.1 auto brake |
| 0x15 | AWV distance warning | SetGet | 1 | `00` | b0.0 AWV distance warning |
| 0x16 | AWV warning timegap | SetGet | 2 | `00 00` | b0 = AWV warning timegap |

## 0x1D LSG_1D, DF [3.3](lsg1D-lsg_1d-df3.3.md)

| FCT | Function | Op | Bytes | Idle | Fields |
|---|---|---|---|---|---|
| 0x14 | Hybrid target range | SetGet | 3 | `00 00 00` | range: 1 → `00 00 00`, 2 → `00 00 00`, 10 → `00 02 00`<br>b2 = unit |
| 0x18 | Hybrid active pedal | SetGet | 2 | `00 00` | b0.0 state |

## 0x1F RGS, DF ?

| FCT | Function | Op | Bytes | Idle | Fields |
|---|---|---|---|---|---|
| 0x0D | Rgs set factory default | SetGet | 1 | `01` |  |
| 0x10 | RGS belt pretensioner data front | SetGet | 2 | `00 00` | b0.0 left<br>b0.1 right |
| 0x11 | RGS belt pretensioner data rear | SetGet | 2 | `00 00` | b0.0 left<br>b0.1 right |
| 0x12 | RGS pre crash system | SetGet | 2 | `00 00` | b0.0 state |
| 0x13 | RGS pre sense system | SetGet | 2 | `00 00` | b0.0 state |
| 0x14 | RGS pre sense warning | SetGet | 2 | `00 00` | state: 1 → `01 00`, 2 → `03 00` |
| 0x16 | RGS local hazard information | Set | 6 | `00 00 00 00 00 00` | b0 = event ID<br>b1 = event type<br>b2 = event quality<br>distance value: 1.0 → `00 00 00 E8 03 00`, 20.5 → `00 00 00 00 00 00` |

## 0x21 VZA, DF [3.3](lsg21-vza-df3.3.md)

| FCT | Function | Op | Bytes | Idle | Fields |
|---|---|---|---|---|---|
| 0x0D | TSD system on off | SetGet | 1 | `00` | b0.4 activity |
| 0x0D | Tsd set factory default | SetGet | 1 | `11` |  |
| 0x11 | TSD road sign filter | SetGet | 2 | `00 00` | b0.0 trailer limits |
| 0x14 | TSD speed warning threshold | SetGet | 4 | `00 00 00 00` | b0.0 state<br>speed value: 1.0 → `00 01 00 00`, 20.5 → `00 14 00 00`<br>speed unit: 1 → `00 00 01 00` |
| 0x16 | TSD speed warning acoustics | SetGet | 2 | `18 00` | b0.0 state |

## 0x22 MKE, DF [3.0](lsg22-mke-df3.0.md)

| FCT | Function | Op | Bytes | Idle | Fields |
|---|---|---|---|---|---|
| 0x0D | MKE system on off | SetGet | 1 | `00` | b0.4 activity |
| 0x0D | MKE set factory default | SetGet | 1 | `11` |  |

## 0x23 BCmE, DF ?

| FCT | Function | Op | Bytes | Idle | Fields |
|---|---|---|---|---|---|
| 0x0D | Bcme set factory default | SetGet | 1 | `01` |  |
| 0x12 | BCmE live tip | Ack | 2 | `00 00` | b0 = live tip<br>b1.0 confirmation |

## 0x25 BatteryControl, DF ?

| FCT | Function | Op | Bytes | Idle | Fields |
|---|---|---|---|---|---|
| 0x0D | Battery control set factory default | SetGet | 1 | `01` |  |
| 0x13 | Battery control timer state | SetGet | 2 | `00 00` | b0.0 timer 1<br>b0.1 timer 2<br>b0.2 timer 3<br>b0.3 timer 4 |
| 0x18 | Battery control immediately | StartResult | 2 | `00 00` | b0 = ref profile ID<br>b1 = control |
| 0x1B | Battery control past error reason | SetGet | 2 | `00 00` | reset: 1 → `01 00` |
| 0x1D | Battery control remaining charge time | SetGet | 7 | `00 00 00 00 00 00 00` | b0 = time 1 mode<br>b1 = time 1 target SOC<br>time 2 mode: 1 → `10 00 00 00 00 00 00`, 2 → `20 00 00 00 00 00 00`, 15 → `F0 00 00 00 00 00 00`<br>b2 = time 2 target SOC |

## 0x26 Brake, DF ?

| FCT | Function | Op | Bytes | Idle | Fields |
|---|---|---|---|---|---|
| 0x10 | Brake electrical parking | SetGet | 2 | `00 00` | b0.0 enable |
| 0x11 | Brake auto hold | SetGet | 2 | `00 00` | b0 = auto hold setup |
| 0x12 | Brake esc mode | SetGet | 2 | `00 00` | b0 = esc mode setup |
| 0x13 | Brake hdc mode | SetGet | 2 | `00 00` | b0.0 state |

## 0x3C EfficiencyAssist, DF ?

| FCT | Function | Op | Bytes | Idle | Fields |
|---|---|---|---|---|---|
| 0x0D | EA system | SetGet | 1 | `00` | b0.4 state |
| 0x0D | EA set factory default | SetGet | 1 | `11` |  |
| 0x10 | EA pedal jerk | SetGet | 2 | `00 00` | b0.0 state |

Related: [Function catalogue](README.md) · [Opcodes](../protocol/opcodes.md)
