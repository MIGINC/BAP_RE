# PQ CAN Channels

On PQ, BAP uses 11-bit CAN IDs. Each FSG transmits on its own ID. The ASG side is more shared: the cluster, for example, sends all its infotainment BAP requests on one ID, and every infotainment FSG listens there and picks out its own LSG from the header.

The full, running list of every ID, its LSG(s) and who's on each end is on the **[CAN ID Map](can-id-map.md)**. This page covers how the channels are organised.

## The cluster's side

The Passat B7 cluster transmits BAP on five IDs, grouped by bus area rather than by module:

| Cluster TX | Name | Who listens (FSGs) |
|---|---|---|
| 0x676 | BAP_Anzeige | driver assist: ACC, Lane Assist, Side Assist, parking, VZA, hybrid |
| 0x67C | BAP_ASG_07 | infotainment: phone/SD, audio, compass/nav |
| 0x6C8 | BAP_ASG_01 | comfort: RDK, lights, comfort settings |
| 0x6C9 | BAP_ASG_02 | aux heater and others |
| 0x6E8 | BAP_BC | not matched to an LSG yet (possibly the cluster as FSG for the board computer) |

One trap: IDs can differ between cluster software versions, so if a module won't handshake, check what your cluster actually sends on. 0x67A is the radio asking, not a cluster.

## The radio's side

The RNS / radio is the ASG for the other half of the infotainment world: telephone, phonebook, MDI, rear camera, TV and amplifier. It has one request ID per group of modules (BAP_ASG_03, _04, _06, _08…) instead of the cluster's handful of shared ones.

The MIB2 PQ asks the amplifier (0x2D) on 0x6C7, Climatronic on 0x6CB, eCall and ENI on 0x6B7, Side Assist on 0x61D, and parking and VZA on 0x67A. It's also the FSG for the cluster's phone, audio and navigation pages, on 0x66F, 0x66C and 0x67D.

Two things to watch here. The phonebook splits its functions across two transmit IDs, so one LSG doesn't always mean one CAN ID. Parking does the same: two of LSG 0x0A's functions arrive on 0x678 (BAP_OPS_02) and the rest on 0x6DA. And late PQ cars with factory MIB2 PQ carry some MQB-generation modules (the rear camera, for example) on the same PQ IDs.

## What the gateway routes

The gateway forwards whole BAP frames by CAN ID. The [PQ Network Map](pq-network-map.md) draws it out:

| Bus | FSGs on it | Requests forwarded to it |
|---|---|---|
| ACAN (powertrain) | ACC 0x674, Lane Assist 0x673, parking 0x6DA, hybrid 0x679, 0x678 | 0x676 (cluster), 0x67A (radio) |
| ECAN (extended) | ACC, Lane Assist, Side Assist 0x672, VZA 0x69B, RDK 0x6DD | 0x676, 0x6C8, 0x67A, 0x61D |
| KCAN (comfort) | RDK 0x6DD, light 0x6DF, comfort 0x6DE, Climatronic 0x6DB, rear camera 0x6DC | 0x6C8 (cluster), 0x67A, 0x6CB, 0x6D9 (radio) |
| ICAN (infotainment) | aux heater 0x66E, phone SD 0x66F, audio 0x66C, nav 0x67D | 0x67C, 0x6C9, 0x6E8 (cluster) |

The radio has a request ID for each module it can only reach through the gateway: 0x6CB for Climatronic, 0x6D9 for a rear camera on KCAN, and 0x61D for Side Assist. Side Assist only reaches the radio on later gateways. The gateway also forwards parking, hybrid and VZA to ICAN so the radio can show them.

The radio's own modules (phone, phonebook, MDI, TV, amplifier) share ICAN with the radio, so the gateway never routes them.

## Debug IDs

`0x6D6` / `0x6D7` are named BAP_Debug_Cmd / BAP_Debug_Data, and lots of modules list them. I've never seen them do anything useful.

Related: [CAN ID Map](can-id-map.md) · [PQ Network Map](pq-network-map.md) · [LSG Index](../lsg/README.md) · [Frames on the Wire](../protocol/frames.md)
