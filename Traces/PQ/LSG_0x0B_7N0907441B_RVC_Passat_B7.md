# LSG_0x0B_7N0907441B_RVC_Passat_B7

**Trace File**: `LSG_0x0B_7N0907441B_RVC_Passat_B7.crtd`  
**Part Number**: 7N0 907 441 B  
**LSG ID**: 0x0B (Rear View Camera / RVC)  
**Vehicle**: Passat B7  
**Platform**: PQ

---

## 1. Setup

- **Bench / Vehicle**: 2013 Passat B7 (Infotainment CAN).
- **Hardware**:  
  - Teensy++ Can Logger  


---

## 2. Procedure / User Actions

1. **Activated RVC** (manual toggling via PDC button).
2. **Cycled through camera modes**:
   - Standard rear-view
   - Trailer helper mode
   - Parallel parking mode
3. **Adjusted image sliders**:
   - Brightness: Min → Max
   - Contrast: Min → Max
   - Color: Min → Max

---

## 3. Observations & Decoded Frames

Below is a **high-level** summary of notable frames and what they represent. (See the raw `.crtd` file or the console output for exact timestamps and data.)

- **Heartbeat / Property_HeartbeatStatus**  
  - Appears periodically (e.g., every second).  
  - Confirms the module is in “normalOperation.”

- **BAP-Config** frames  
  - Repeatedly show `bap_version_major = 3, minor = 1`, and `lsg_class = 11` (0x0B).  
  - Indicates the RVC module’s software version details.

- **FSG-OperationState** (Property_Status)  
  - Data `[00]` = `{'state': 'normalOperation'}`.  
  - Confirms the RVC is active and not in an error mode.

- **FCT_ID=0x10, 0x11, 0x16, 0x17, 0x18, 0x19, 0x1A**  
  - These function IDs appear frequently with `Op=Property_Status` or `Op=Property_Get`.  
  - Some carry numeric values (e.g. `[64]` or `[38]`), which might correlate to camera modes or slider values. (under investigation).


---

## 4. Known / Unknown Decoding

- **Known**:  
  - Heartbeat (Op=Property_Status or _HeartbeatStatus) is recognized.  
  - BAP-Config repeating frames confirm the module’s BAP version and LSG ID.  
  - `FSG-Setup` and `FSG-OperationState` are part of the standard BAP init/operational states.  

