# Traces

Captures with notes, cut down to the BAP traffic of one LSG: the module's frames and the display's requests to it, nothing else from the bus. The single-LSG traces from my Passat are listed on [PQ Traces](PQ/README.md). The others have a `.md` next to them. To add one, see [CONTRIBUTING](../CONTRIBUTING.md) and the [template](TEMPLATE.md).

| Trace | Platform | LSG | Notes |
|---|---|---|---|
| [LSG_0x07_RDK_Passat_B7](PQ/LSG_0x07_RDK_Passat_B7.md) | PQ | 0x07 tyre pressure | complete start-up, size list and rotation, with expected values |
| [LSG_0x0B_7N0907441B_RVC_Passat_B7](PQ/LSG_0x0B_7N0907441B_RVC_Passat_B7.md) | PQ | 0x0B rear camera | clean heartbeat rotation, two start-ups, radio mode writes; logger drops frames |
| [LSG_0x33_unknown_eCall_MQB](MQB/LSG_0x33_unknown_eCall_MQB.md) | MQB | 0x33 eCall | one GetAll / StatusAll, last frame missing; the only MQB capture so far |
