# Contributing

The most useful thing you can send is a **trace with notes on what you did while recording it**. Decoding BAP is mostly lining up bytes against actions.

## Traces

1. Put the capture, filtered as described under [Guidelines](#guidelines), under `Traces/PQ/` or `Traces/MQB/`, named `LSG_0xNN_<part number>_<what>_<car>.<ext>`.
   Any common text format is fine (`.crtd`, `.trc`, `.asc`, `.csv`).
2. Add a `.md` next to it using [the template](Traces/TEMPLATE.md): the car, the module part number, the bus, the logger, and a
   **timestamped list of what you did** ("12.4 s opened the tyre menu", "31.0 s set load to full").
3. Capture at full rate if you can. Dropped frames break multi-frame messages, the RVC trace shows what that looks like.

Start-up is gold. If you can, start logging *before* switching the ignition on, so the GetAll/StatusAll exchange is in the trace.

## Wanted

- **Anything marked Untested** in the [LSG index](docs/lsg/README.md) or still `Fct_XX` in the [catalogue](docs/catalogue/README.md).
- **MQB traces** with BAP in them, anything at all.
- **Confirmations of the MQB ID table** in [the CAN ID map](docs/platforms/can-id-map.md).
- **Menu captures**, meaning logs taken while changing settings in the cluster or radio menus. Physically operating things (lights, windows) produces plain CAN, not BAP.

## The CAN ID map

`data/can_map_pq.csv` and `data/can_map_mqb.csv` hold the CAN ID → LSG → FSG/ASG table. If you've matched an ID to an LSG, a PR adding or correcting a row is ideal. Set `status` to `Logged in car` for your own captures, and point to a trace.

## Corrections

If something in the docs is wrong, open an issue or a PR with the evidence: a trace excerpt, or a reference. Say whether it was logged in a car or tested on a bench.

## Guidelines

- Keep PQ and MQB apart; they share framing but not CAN IDs.
- Filter traces to the one LSG before posting: its FSG ID and the ASG ID that asks it, and on shared IDs only that LSG. Raw logs carry your VIN and mileage, and phone or navigation LSGs can carry names and places.
- Don't post confidential documents, source code or firmware.
