# BAP Protocol Reverse Engineering

Documentation and analysis of Volkswagen's event-driven **Bedien-und Anzeigeprotokoll (BAP)**, reverse-engineered for PQ and MQB platforms. Includes protocol specifications, CAN traces, and behavioral observations.
Research here will help with the development of an open source library to use to interface with BAP devices. 
---

## Table of Contents
- [Protocol Overview](#protocol-overview)
- [Platform Implementations](#platform-implementations)
- [Message Architecture (PQ)](#message-architecture-(PQ))
- [CAN Traces](#can-traces)
- [Contributing](#contributing)
- [Safety & Legal](#safety--legal)
- [License](#license)

---

## Protocol Overview

BAP (Control and Display Protocol) manages ECU communication in Volkswagen Group vehicles, succeeding DDP (Diagnostic Data Protocol). Key features:

- **Event-driven architecture**: Reduces bus load vs constant polling
- **Standardized messaging**: Fixed header with variable payload
- **Multi-platform support**: PQ and MQB implementations
- **Error resilience**: Automatic retransmission and state synchronization
- **Multi-frame support** for messages >6 bytes

---

## Platform Implementations

### PQ Platform
**CAN 2.0A** implementation with 11-bit identifiers:
```plaintext
┌───────────────┬──────────────┬─────────────────┐
│ 11-bit CAN ID │   DLC (4b)   │   Payload (8B)  │
├───────────────┼──────────────┼─────────────────┤
│   0x123       │     8        │ 01 03 0A 00 05. │
└───────────────┴──────────────┴─────────────────┘
                                       │
                                       ▼
┌─────────────┬─────────────┬─────────────────┐
│ BAP Header  │ BAP Header  │ Data            │
│ Byte 1      │ Byte 2      │ (up to 6 bytes) │
├─────────────┼─────────────┼─────────────────┤
│ op_code/LSG │ LSG/FCT     │ Payload Data    │
└─────────────┴─────────────┴─────────────────┘
```

Example message:
```
ID: 0x123  DLC: 8  Data: 01 03 0A 00 05 01 00 00
    │           │       └─────────────────────┘
    │           │                └── BAP payload
    │           └── Length
    └── Standard 11-bit identifier
```

### MQB Platform

Extended 29-bit identifiers with embedded addressing:
Check out the MQB section for more info.

```
Extended CAN ID Structure (29-bit):
┌───────────┬────────────┬
│ Base ID   │ LSG ID     │  
│ (11 bits) │ (8 bits)   │ 
└───────────┴────────────┴
```

Example:
```
ID: 0x1733 08 00
      └──┘ └── LSG ID (0x08)
      └── Base identifier
    
    
```

## Message Architecture (PQ)

### Header Format

The BAP header uses a 2-byte format:

```
Bit Layout:
15 14 13 12│11 10 09 08 07 06│05 04 03 02 01 00
└─op_code──┘└────lsg_id─────┘└────fct_id───────┘
```

Fields:
- `op_code` (4 bits): Operation type identifier
- `lsg_id` (6 bits): Logical System Group identifier
- `fct_id` (6 bits): Function identifier within the LSG

### Common Operations

| op_code | Name | Example Payload | Purpose |
|---------|------|----------------|----------|
| 0x1 | BAP-Config | `03 01 0B 00 05` | Version info |
| 0x2 | FSG-Setup | `00` | Ready state |
| 0x3 | Heartbeat | `0A` | Keep-alive |

### Message Sequences

Seen initialization sequence:
```
Device → ECU: BAP-Config    [Version info]
ECU → Device: FSG-Setup     [Ready state]
Device → ECU: Heartbeat     [Interval]
```

Error recovery patterns:
```
On timeout: Reinitialize sequence
On data error: Request retransmission
On state mismatch: Request full update
```

## CAN Traces

### Directory Structure

```
MIB -> RVC
Coming soon 
```

### Documentation Template

For each control unit:

```markdown
### Control Unit Information
- Part Number: [e.g., 5K0-000-xxx]
- Software Version: [e.g., 0102]
- Type: [LSG/FSG]
- Known FCT/LSG IDs: [List with descriptions]

### Behavior Documentation
- Platform: [PQ/MQB]
- User Action: [Description]
- Expected Response: [Description]
- Trace File: [Filename]
```

## Contributing

We welcome contributions that expand our understanding of the BAP protocol. Focus areas include:

1. Message pattern documentation
2. Unknown FCT code identification
3. Platform-specific behaviors
4. Error handling sequences

See [CONTRIBUTING.md](CONTRIBUTING.md) for detailed guidelines.

## Safety & Legal

### ⚠️ Safety Notice

Vehicle systems research requires:
- Understanding of automotive safety systems
- Proper testing environment (Bench Test when possible)
- Do no interfer with the Powertrain CAN, BAP is avaible on non-critical buses
- Compliance with local regulations
- Awareness of warranty implications

### Research Used

Used for protocol understanding only (no code incorporated):

- [kisim](https://github.com/tmbinc/kisim/) (BSD-3): Protocol analysis concepts
- [revag-bap](https://github.com/norly/revag-bap/) (GPLv2): Early BAP documentation
- Karl Dietmann's BAP research (blog.dietmann.org, archived)

## License

This documentation is licensed under the MIT License. See [LICENSE](LICENSE) for details.

---
