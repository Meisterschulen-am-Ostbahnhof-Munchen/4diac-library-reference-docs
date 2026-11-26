# AlPgnRxNew_TP

![AlPgnRxNew_TP](./AlPgnRxNew_TP.svg)

* * * * * * * * * *

## Introduction

Description from the comment of the type: Receive a PGN of any length (also via transport protocol) into a byte array

Package: `isobus::pgn::rx`

## Interface structure

### **Event inputs**

-   **INIT** (Type: `EInit`): Service Initialization
-   **install** (Type: `Event`): installs a Receive PGN (RX) (with data: `u32Pgn`, `NmSource`, `u8Priority`, `Data`)
-   **CHG** (Type: `Event`): Change source to NmSource.s16Handle (with data: `NmSource`)

### **Event outputs**

-   **INITO** (Type: `EInit`): Initialization Confirm
-   **installO** (Type: `Event`): install Done (with data: `PGN_handle`)
-   **IND** (Type: `Event`): Data was received (with data: `s32TimeStamp`, `u16DaSize`, `u8RxPriority`, `OVERFLOW`, `Data`)
-   **dataERR** (Type: `Event`) (with data: `dataERRC`)
-   **pgnERR** (Type: `Event`) (with data: `pgnERRC`)

### **Data inputs**

-   **u32Pgn** (Type: `UDINT`): PGN - Range: 0 - 0x3FFFF
-   **NmSource** (Type: `isobus::pgn::ISONETEVENT_T`): Define the Communication Partner
-   **u8Priority** (Type: `USINT`): Priority given to the stack (0..7), not used as receive filter; received priority see u8RxPriority (Default: `7`)

### **Data outputs**

-   **PGN_handle** (Type: `INT`): PGN-handle on success, HANDLE_UNVALID in case of an error
-   **dataERRC** (Type: `INT`): dataERR error Code
-   **pgnERRC** (Type: `INT`): pgnERR error Code
-   **s32TimeStamp** (Type: `DINT`): Time stamp of the message in ms (Default: `-1`)
-   **u16DaSize** (Type: `UINT`): Received data length in bytes (0..65535)
-   **u8RxPriority** (Type: `USINT`): Priority of the received PGN
-   **OVERFLOW** (Type: `BOOL`): TRUE if more bytes were received than Data holds (Data is cut)

### **Adapters (sockets)**

None.

### **Adapters (plugs)**

None.

## Extended Transport Protocol (ETP)

!!! warning "State: not yet tested"
    The following statements come from the firmware development (LOGIBUS commit `24534469`, forte commit `8d02d14f7`). Both commits were not pushed at the time of writing and I could not check them. Nothing has been tested on hardware.

- **Size:** The block accepts messages up to 65535 bytes (before 1785). `u16DaSize` is valid from 0 to 65535. `OVERFLOW` indicates that `Data` (for example a `PROVIDE_ARR_*`) was too small.
- **ETP only addressed:** ETP only exists to a specific destination, a broadcast is not possible.
- **LOGIBUS:** Until now the default TP filter of the CCI driver discarded every ETP. LOGIBUS now sets its own filter that accepts TP and ETP up to 128 KiB (`LOGIBUS_ETP_IN_SIZE_MAX`). The stack aborts larger ETPs with "System resources". The receive buffer comes from PSRAM and is released after the data has been distributed, also after an abort.
- **PGN range:** PGNs from `0x1F000` to `0x1FFFF` with more than 8 bytes go as NMEA Fast Packet (at most 223 bytes), not as ETP. For ETP use Proprietary A2 (`0x1EF00`) or Proprietary A (`0xEF00`).
- See also [`AlPgnTxNew_TP`](../tx/AlPgnTxNew_TP.md), [`DlPgnSend`](../tx/DlPgnSend.md) and the page NMEA 2000 Fast Packet.

Note: This page was generated from the type definition (`AlPgnRxNew_TP.fbt`) and only shows the interface. A detailed description of the behavior is still missing.
