# ATM_CLIENT_1_0

* * * * * * * * * *

## Introduction

The **ATM_CLIENT_1_0** function block is a composite function block that encapsulates the network-based `CLIENT_1_0` function block from the IEC 61499 standard library and maps its interface to a unidirectional **ATM adapter** (`TIME` data type). A **TIME** duration value present at adapter socket `IN` is buffered via an internal `E_D_FF_ANY` flip-flop and then sent via `CLIENT_1_0` as an OPC UA **Write** or `CALL_METHOD` invocation to the remote node configured under `ID`.

## Interface Structure

### **Event Inputs**

- **INIT** (EInit): Initialization event, connected to `QI` and `ID`

### **Event Outputs**

- **INITO** (EInit): Initialization confirmation, connected to `QO` and `STATUS`
- **CNF** (Event): Confirmation that data was sent, connected to `QO` and `STATUS`

### **Data Inputs**

- **QI** (BOOL): Input qualifier, opens (TRUE) or closes (FALSE) the connection to the server
- **ID** (WSTRING): Connection identifier (OPC UA address of the target node, e.g., `opc_ua[WRITE;opc.tcp://192.168.1.12:4840#;...]`)

### **Data Outputs**

- **QO** (BOOL): Output qualifier, connection status
- **STATUS** (WSTRING): Status information as a Unicode string

### **Adapters**

| Adapter | Type                                 | Direction     | Description            |
| ------- | ----------------------------------- | ------------- | ---------------------- |
| IN      | adapter::types::unidirectional::ATM | Socket (Input)| TIME value to be sent  |

## Functionality

1. The `INIT` event initializes the internal `CLIENT_1_0` block with `QI` and `ID`, establishing connection to the remote server and acknowledging with `INITO`.
2. When the ATM socket `IN` receives an event on `IN.E1`, the TIME value at `IN.D1` is latched into the internal `E_D_FF_ANY` flip-flop.
3. The flip-flop holds the value stable and generates event `EO`.
4. `EO` triggers `F_MOVE` (`TIME` data type) to supply `CLIENT_1_0.SD_1` and fire the send event `REQ` on `CLIENT_1_0`.
5. After successful transmission, `CLIENT_1_0` confirms with `CNF`.

## Technical Features

- **Buffering with E_D_FF_ANY**: The TIME value to be sent is buffered using an internal `E_D_FF_ANY`, preventing value instability during transmission.
- **Remote Write / Call Method**: `CLIENT_1_0` directly addresses a remote OPC UA server to transfer time duration parameters.
- **Encapsulation**: The raw `CLIENT_1_0` event/data interface is kept internal; only the clean ATM adapter interface is exposed.

## State Overview

1. **Uninitialized**: The block waits for the `INIT` event.
2. **Initialized**: Connection to the remote server is established; ready to send.
3. **Send Active**: An event on the ATM socket buffers the TIME value and triggers the remote write.

## Application Scenarios

- **Remote Time Parameter Transmission**: Sending time duration values (e.g., valve pulse durations, flushing times) to a remote controller via OPC UA.
- **Modular Control Architectures**: Integrating distributed TIME values into libraries built on ATM adapters.

## Comparison with Similar Blocks

- **AX_CLIENT_1_0**: Handles BOOL values via an AX adapter.
- **AR_CLIENT_1_0**: Handles REAL values via an AR adapter.
- **ATM_PUBLISH_1**: Encapsulates `PUBLISH_1` for local publish/subscribe.

## Conclusion

**ATM_CLIENT_1_0** allows direct writing of TIME values to a remote OPC UA server over a clean ATM adapter interface.
