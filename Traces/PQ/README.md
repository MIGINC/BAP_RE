# PQ Traces

Passat B7, logged in car, `.crtd` (GVRET / SavvyCAN). Each trace has only the frames of one LSG, on the IDs shown. Time starts at 0 in each, except the older rear camera trace.

ICAN and KCAN are 100 kbit/s, ACAN and ECAN are 500 kbit/s. *Settings* traces are menu changes in the cluster: each write is opcode 2 (SetGet, or StartResult for the RDK's 0x17), and the module's answer follows. Cluster writes may be padded to DLC 8, see [Frames on the Wire](../../docs/protocol/frames.md#single-frame).

| Trace | DF | Bus | IDs | Shows |
|---|---|---|---|---|
| [0x01 Klima1, controls](LSG_0x01_Klima1_controls_Passat_B7.crtd) | 3.3 | KCAN | 0x6CB 0x6DB | GetAll, StatusAll and Status changes while the A/C controls were used, 60 s |
| [0x05 ACC, settings](LSG_0x05_ACC_settings_Passat_B7.crtd) | 3.4 | ECAN | 0x674 0x676 | writes to 0x13, 0x14 |
| [0x05 ACC, startup](LSG_0x05_ACC_startup_Passat_B7.crtd) | 3.4 | ECAN | 0x674 0x676 | Reset, GetAll, StatusAll, then the rotation, 30 s |
| [0x07 RDK, settings](LSG_0x07_RDK_settings_Passat_B7.crtd) | 4.0 | ECAN | 0x6C8 0x6DD | writes to 0x17 |
| [0x08 Innenlicht, settings](LSG_0x08_Innenlicht_settings_Passat_B7.crtd) | 3.0 | KCAN | 0x6C8 0x6DF | writes to 0x10 |
| [0x08 Innenlicht, startup](LSG_0x08_Innenlicht_startup_Passat_B7.crtd) | 3.0 | KCAN | 0x6C8 0x6DF | Reset, GetAll, StatusAll, then the rotation, 30 s |
| [0x09 Aussenlicht, settings](LSG_0x09_Aussenlicht_settings_Passat_B7.crtd) | 4.3 | KCAN | 0x6C8 0x6DF | writes to 0x10, 0x12, 0x13, 0x16 |
| [0x09 Aussenlicht, startup](LSG_0x09_Aussenlicht_startup_Passat_B7.crtd) | 4.3 | KCAN | 0x6C8 0x6DF | Reset, GetAll, StatusAll, then the rotation, 30 s |
| [0x0A OPS, ack](LSG_0x0A_OPS_ack_Passat_B7.crtd) | 4.1 | ACAN | 0x678 0x67A 0x6DA | StatusAck and Ack on 0x15, 28 s |
| [0x0A OPS, startup](LSG_0x0A_OPS_startup_Passat_B7.crtd) | 4.1 | ACAN | 0x676 0x678 0x67A 0x6DA | Reset, GetAll, StatusAll, then the rotation, 30 s |
| [0x0B RVC, startup](LSG_0x0B_RVC_startup_Passat_B7.crtd) | 5.1 | ICAN | 0x6CA 0x6DC | Reset, GetAll, StatusAll, then the rotation, 30 s |
| [0x0D Doorlocking, settings](LSG_0x0D_Doorlocking_settings_Passat_B7.crtd) | 3.3 | KCAN | 0x6C8 0x6DE | writes to 0x11, 0x13, 0x18, 0x1A, 0x1B, 0x1F |
| [0x0D Doorlocking, startup](LSG_0x0D_Doorlocking_startup_Passat_B7.crtd) | 3.3 | KCAN | 0x6C8 0x6DE | Reset, GetAll, StatusAll, then the rotation, 30 s |
| [0x0E Mirror, settings](LSG_0x0E_Mirror_settings_Passat_B7.crtd) | 3.0 | KCAN | 0x6C8 0x6DE | writes to 0x10 |
| [0x0E Mirror, startup](LSG_0x0E_Mirror_startup_Passat_B7.crtd) | 3.0 | KCAN | 0x6C8 0x6DE | Reset, GetAll, StatusAll, then the rotation, 30 s |
| [0x19 LDW, settings](LSG_0x19_LDW_settings_Passat_B7.crtd) | 4.0 | ECAN | 0x673 0x676 | writes to 0x0D, 0x10, 0x11, 0x12 |
| [0x19 LDW, startup](LSG_0x19_LDW_startup_Passat_B7.crtd) | 4.0 | ECAN | 0x673 0x676 | Reset, GetAll, StatusAll, then the rotation, 30 s |
| [0x1A SWA, settings](LSG_0x1A_SWA_settings_Passat_B7.crtd) | 3.4 | ECAN | 0x672 0x676 | writes to 0x13, 0x17 |
| [0x1A SWA, startup](LSG_0x1A_SWA_startup_Passat_B7.crtd) | 3.4 | ECAN | 0x61D 0x672 0x676 | Reset, GetAll, StatusAll, then the rotation, 30 s |
| [0x1B AWV, settings](LSG_0x1B_AWV_settings_Passat_B7.crtd) | 3.2 | ECAN | 0x674 0x676 | writes to 0x10, 0x13, 0x14 |
| [0x1B AWV, startup](LSG_0x1B_AWV_startup_Passat_B7.crtd) | 3.2 | ECAN | 0x674 0x676 | Reset, GetAll, StatusAll, then the rotation, 30 s |
| [0x21 VZA, settings](LSG_0x21_VZA_settings_Passat_B7.crtd) | 3.0 | ECAN | 0x676 0x67A 0x69B | writes to 0x0D, 0x11 |
| [0x21 VZA, startup](LSG_0x21_VZA_startup_Passat_B7.crtd) | 3.0 | ECAN | 0x676 0x67A 0x69B | Reset, GetAll, StatusAll, then the rotation, 30 s |
| [0x2B SecondaryDisplay, page](LSG_0x2B_SecondaryDisplay_page_Passat_B7.crtd) | 3.1 | ICAN | 0x66F 0x67C | opening the phone page, then scrolling a list. Excerpt: every frame with text is removed |
| [0x2B SecondaryDisplay, startup](LSG_0x2B_SecondaryDisplay_startup_Passat_B7.crtd) | 3.1 | ICAN | 0x66F 0x67C | connect, 0x10 to 0x13, first screens and 0x15 Sets, 30 s |
| [0x31 Audio, startup](LSG_0x31_Audio_startup_Passat_B7.crtd) | 3.1 | ICAN | 0x66C 0x67C | Reset, GetAll, StatusAll, then the rotation, 30 s |
| [0x32 Navigation, startup](LSG_0x32_Navigation_startup_Passat_B7.crtd) | 3.0 | ICAN | 0x67C 0x67D | Reset, GetAll, StatusAll, then the rotation, 30 s |

Two more have a page of their own: [RDK start-up](LSG_0x07_RDK_Passat_B7.md) and [the rear camera with dropped frames](LSG_0x0B_7N0907441B_RVC_Passat_B7.md).

Frames with call list text are left out of the 0x2B traces, because they carry contact names and numbers.
