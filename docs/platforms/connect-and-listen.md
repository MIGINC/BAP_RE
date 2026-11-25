# Connect and Listen Safely

To read BAP from a car, for a tyre pressure display for example, you only need to listen.

## Bus and transceiver

| Platform | Bus | Bit rate | Physical layer |
|---|---|---|---|
| PQ | **ICAN** (infotainment), **KCAN** (comfort) | 100 kbit/s | fault-tolerant (FT, ISO 11898-3) |
| PQ | ACAN, ECAN, cluster bus | 500 kbit/s | high speed (HS, ISO 11898-2) |
| MQB | all | 500 kbit/s | high speed (HS, ISO 11898-2) |
| MQB Evo | not covered, has CAN FD | | |

- The [PQ Network Map](pq-network-map.md) shows which module is on which bus.
- PQ BAP IDs are 11-bit (`0x6xx`). MQB infotainment BAP IDs are 29-bit (`0x1733xxxx`).
- HS transceivers on ICAN and KCAN are **not to the spec**, though they showed no issues in my testing. Use an FT one (TJA1055 type) for anything that stays in the car.
- Check that the transceiver's logic side suits your board. An ESP32 is 3.3 V.
- Don't connect a classic CAN controller to a CAN FD bus.

**Remember: no 120 Ω termination on your board in a car.**

## Steps

1. Set the controller to **listen-only mode**. It then can't send anything, not even an ACK.
2. Test on a bench against a [trace](../../Traces/README.md).
3. Park, and switch the ignition off.
4. Connect CAN high, CAN low and ground at the module or the gateway. Keep the branch short.
5. Switch the ignition on. Check for new warning lights and receive errors.
6. Wait for the module's BAP_Config and FunctionList before you trust a value.
7. Compare with what the car shows, or with a gauge.

Don't expect BAP on the OBD socket. The gateway doesn't forward BAP traffic there.

## On a bench

- With one module, use normal mode. In listen-only nothing ACKs, and the module keeps repeating its frame.
- Terminate an HS bus with 120 Ω at each end. Check whether the cluster has its own first.
- Unplug the module you're replacing.
- A bare cluster is on the cluster bus, 500 kbit/s HS, even for IDs that are on ICAN in the car. The gateway forwards those.
- What wakes a cluster isn't covered here. Coding depends on the LSG, see [LSG Index](../lsg/README.md#extra-conditions).

## Rules

- **Never send on an ID that a module already uses**, the cluster's request IDs included.
- Emulate modules on a bench, not in a car.
- Don't watch logs while driving.
- Your display doesn't replace the car's own warning lights.
- Use a fused, ignition-switched supply.

Related: [PQ Network Map](pq-network-map.md) · [CAN ID Map](can-id-map.md) · [0x07 RDK](../lsg/0x07-rdk.md)
