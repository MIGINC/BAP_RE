# Function catalogue

One page per LSG and LSG version (DF): function IDs, names, classes, data types and sizes, plus the payload layout where I have one. The same data is in [data/catalogue](../../data/catalogue) as JSON, if you want to feed it to a decoder.

- The DF is bytes 4 and 5 of BAP_Config. Match on that, not on the car: a PQ car can carry a module with an MQB-era DF.
- `Fct_XX` means the function exists but has no name I trust yet.
- **TX** is what the FSG sends, **RX** is what it accepts, in bytes.
- A function being listed doesn't mean a given module implements it. The module's own FunctionList (0x03) decides that.
- **Car** on a page marks the functions a module with exactly that DF sent in a car capture. Pages without the column have no car capture for that DF. The JSON has it as `logged_in_car`.
- **Sim** on a page marks the functions simulated at the application layer, see [status tags](../../README.md#status-tags). The JSON has it as `simulated`.
- The status is for the LSG as a whole. It doesn't mean every function on the page has been exercised, or every DF. If it says Untested, treat the page as a lead.

What an ASG writes back is on its own page: [ASG requests (MIB2)](asg-requests-mib2.md).

| LSG | Name | DF (functions) | Status |
|---|---|---|---|
| 0x01 | Klima1 | [3.0](lsg01-klima1-df3.0.md) (41), [3.3](lsg01-klima1-df3.3.md) (45) | Tested with cluster, Logged in car |
| 0x02 | Klima2 | [3.0](lsg02-klima2-df3.0.md) (16), [3.3](lsg02-klima2-df3.3.md) (21) | Simulated (application layer) |
| 0x03 | ParkHeater | [3.0](lsg03-parkheater-df3.0.md) (17) | Tested with cluster |
| 0x05 | ACC | [3.0](lsg05-acc-df3.0.md) (10), [3.4](lsg05-acc-df3.4.md) (17) | Tested with cluster, Logged in car |
| 0x07 | RDK | [3.0](lsg07-rdk-df3.0.md) (9), [4.0](lsg07-rdk-df4.0.md) (16), [4.3](lsg07-rdk-df4.3.md) (18) | Tested with cluster, Logged in car |
| 0x08 | Innenlicht | [3.0](lsg08-innenlicht-df3.0.md) (8), [5.1](lsg08-innenlicht-df5.1.md) (35) | Tested with cluster, Logged in car |
| 0x09 | Aussenlicht | [4.3](lsg09-aussenlicht-df4.3.md) (15), [5.4](lsg09-aussenlicht-df5.4.md) (23) | Tested with cluster, Logged in car |
| 0x0A | OPS | [3.1](lsg0A-ops-df3.1.md) (16), [3.3](lsg0A-ops-df3.3.md) (19), [4.1](lsg0A-ops-df4.1.md) (36), [4.4](lsg0A-ops-df4.4.md) (44) | Tested with cluster, Logged in car |
| 0x0B | RVC | [3.1](lsg0B-rvc-df3.1.md) (13), [5.1](lsg0B-rvc-df5.1.md) (21), [5.3](lsg0B-rvc-df5.3.md) (26), [5.5](lsg0B-rvc-df5.5.md) (33) | Tested with cluster, Logged in car |
| 0x0C | Wiper | [3.0](lsg0C-wiper-df3.0.md) (8), [3.1](lsg0C-wiper-df3.1.md) (12) | Tested with cluster |
| 0x0D | Doorlocking | [3.3](lsg0D-doorlocking-df3.3.md) (18), [4.5](lsg0D-doorlocking-df4.5.md) (34) | Tested with cluster, Logged in car |
| 0x0E | Mirror | [3.0](lsg0E-mirror-df3.0.md) (8), [3.1](lsg0E-mirror-df3.1.md) (8) | Tested with cluster, Logged in car |
| 0x10 | Sitz1 | [4.0](lsg10-sitz1-df4.0.md) (8), [4.5](lsg10-sitz1-df4.5.md) (9) | Simulated (application layer) |
| 0x17 | Charisma | [3.1](lsg17-charisma-df3.1.md) (5) | Tested with cluster |
| 0x19 | LDW | [4.0](lsg19-ldw-df4.0.md) (10), [4.1](lsg19-ldw-df4.1.md) (10) | Tested with cluster, Logged in car |
| 0x1A | SWA | [3.0](lsg1A-swa-df3.0.md) (11), [3.1](lsg1A-swa-df3.1.md) (15), [3.4](lsg1A-swa-df3.4.md) (19) | Tested with cluster, Logged in car |
| 0x1B | AWV | [3.0](lsg1B-awv-df3.0.md) (7), [3.2](lsg1B-awv-df3.2.md) (11) | Tested with cluster, Logged in car |
| 0x1D | LSG_1D | [3.2](lsg1D-lsg_1d-df3.2.md) (12), [3.3](lsg1D-lsg_1d-df3.3.md) (12) | Tested with cluster |
| 0x20 | Sitz2 | [4.0](lsg20-sitz2-df4.0.md) (7), [4.5](lsg20-sitz2-df4.5.md) (7) | Untested |
| 0x21 | VZA | [3.0](lsg21-vza-df3.0.md) (11), [3.3](lsg21-vza-df3.3.md) (11) | Tested with cluster, Logged in car |
| 0x22 | MKE | [3.0](lsg22-mke-df3.0.md) (8) | Tested with cluster |
| 0x2B | UHV_SecondaryDisplay | [3.1](lsg2B-uhv_secondarydisplay-df3.1.md) (13) | Tested with cluster, Logged in car |
| 0x2D | Amplifier | [3.0](lsg2D-amplifier-df3.0.md) (23), [3.1](lsg2D-amplifier-df3.1.md) (31) | Tested with cluster |
| 0x31 | Audio | [3.1](lsg31-audio-df3.1.md) (23) | Tested with cluster, Logged in car |
| 0x32 | Navigation | [3.0](lsg32-navigation-df3.0.md) (17) | Tested with cluster, Logged in car |
| 0x33 | eCall | [3.5](lsg33-ecall-df3.5.md) (20) | Simulated (application layer) |
| 0x37 | ENI | [3.3](lsg37-eni-df3.3.md) (16) | Untested |
