# BAP Protocol Reverse Engineering

Documentation and analysis of **Bedien-und Anzeigeprotokoll (BAP)**, reverse-engineered for PQ and MQB platforms. Includes protocol specifications, CAN traces, and behavioral observations. This research supports development of open-source libraries for interfacing with BAP-enabled ECUs.


## Protocol Overview

BAP (Control and Display Protocol) manages ECU communication in Volkswagen Group vehicles, succeeding DDP (Diagnostic Data Protocol). Key features:

- **Event-driven architecture**: Reduces bus load vs constant polling
- **Standardized messaging**: Fixed header with variable payload
- **Multi-platform support**: PQ and MQB implementations
- **Error resilience**: Automatic retransmission and state synchronization
- **Multi-frame support** for messages >6 bytes

## Platform Implementations

### PQ Platform
**CAN 2.0A** implementation with 11-bit identifiers:
```plaintext
┌───────────────┬──────────────┬─────────────────┐
│ 11-bit CAN ID │   DLC (4b)   │   Payload (8B)  │
├───────────────┼──────────────┼─────────────────┤
│   0x123       │     8        │ 01 03 0A 00 05. │
└───────────────┴──────────────┴─────────────────┘
```

### Message Architecture (PQ)

#### Multi-Frame Handling
PQ platforms use the first bit of the message to determine frame type:

1. **Short Frame (≤6 bytes)**
```plaintext
Byte 0 (First byte of message):
┌─┬───────────┐
│0│OP Code    │ ◄── First bit 0 indicates Short Frame
└─┴───────────┘

Complete Frame Structure:
┌────────────┬────────────┬──────────────────────┐
│First Byte  │Second Byte │   Payload (0-6 B)    │
│[0|OP Code] │[LSG|FCT]   │                      │
└────────────┴────────────┴──────────────────────┘
```

2. **Long Frame (>6 bytes)**
```plaintext
**Long Frame Structure**

Preamble Byte 0 (Bit Layout):
```plaintext
┌─┬─┬──────────┐
│1│S│  Index   │
└─┴─┴──────────┘
 │ │    │
 │ │    └── 6 bits: Frame index (starts at 0)
 │ └──── Frame type (0=Start frame, 1=Continuation)
 └────── Always 1 for Long Frame
```

Start Frame (S=0):
```plaintext
┌────────────┬────────────┬────────────┬─────────────┐
│Preamble B0 │Preamble B1 │BAP Header  │Payload      │
│[1|0|Index] │[Total Len] │[LSG|FCT]   │(up to 6 B)  │
└────────────┴────────────┴────────────┴─────────────┘

Continuation Frame (S=1):
┌────────────┬─────────────────────────────────┐
│Preamble    │           Payload               │
│[1|1|Index] │         (up to 7 B)            │
└────────────┴─────────────────────────────────┘
```

**Sequence Control**:
- Initial sequence marker: 0x80 (10000000 binary)
- Continuation Index starts at: 0xC0 (11000000 binary)

### MQB Platform
Extended 29-bit identifiers with embedded LSG addressing:
```plaintext
┌──────────────┬────────────┬──────────────┐
│ Base ID      │ LSG ID     │ Subsystem ID │
│ (16 bits)    │ (8 bits)   │ (5 bits)     │
└──────────────┴────────────┴──────────────┘
```

#### LSG Mappings (Found in traces)
- 0x01: Climate Control
- 0x03: Webasto
- 0x0A: Parking Sensors
- 0x11: Time/Date Module

## Research Status

### Confirmed Findings (PQ Platform)
1. **Frame Sequencing**
   - Timeout behavior documented
   - Error recovery mechanisms tested

### Under Investigation (MQB Platform)
1. **Multi-frame Protocol**
   - Frame sequence patterns
   - Error recovery mechanisms
   - Length encoding variations

2. **Advanced Features**
   - Extended addressing schemes
   - Cross-platform compatibility
   - Diagnostic integration

## Contributing

### Priority Research Areas
1. MQB Time/Date Module (0x17331110) testing
2. Multi-frame message handling differences


### Submission Guidelines
1. Maintain clear platform separation (PQ/MQB)
2. Include CAN traces for validation
3. Document test conditions and hardware setup
4. Follow existing format for consistency

## Safety & Legal

### Critical Requirements
- Use bench testing setups 

## License
This project is licensed under the MIT License - see the LICENSE file for details.

## Research Acknowledgments
- Karl Dietmann's BAP research (2009-2012) 
- kisim project (BSD-3) https://github.com/tmbinc/kisim
- revag-bap contributors (GPLv2) https://github.com/norly/revag-bap/
