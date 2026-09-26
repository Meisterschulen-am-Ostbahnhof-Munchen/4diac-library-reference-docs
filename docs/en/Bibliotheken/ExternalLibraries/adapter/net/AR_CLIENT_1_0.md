# AR_CLIENT_1_0

* * * * * * * * * *

## Introduction

The **AR_CLIENT_1_0** function block is a composite function block that encapsulates the network-based `CLIENT_1_0` function block from the IEC 61499 standard library and maps its interface to a unidirectional **AR adapter** (`REAL` data type). A **REAL** value present at the adapter socket `IN` is buffered via an internal `E_D_FF_ANY` flip-flop and then sent via `CLIENT_1_0` as an OPC UA **Write** or `CALL_METHOD` invocation to the remote node configured under `ID`.

Unlike **AR_PUBLISH_1** (local publish/subscribe), `CLIENT_1_0` actively writes to a **remote** server.

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

| Adapter | Type                                | Direction     | Description            |
| ------- | ---------------------------------- | ------------- | ---------------------- |
| IN      | adapter::types::unidirectional::AR | Socket (Input)| REAL value to be sent  |

## Functionality

1. The `INIT` event initializes the internal `CLIENT_1_0` block with `QI` and `ID`, establishing the connection to the remote server and acknowledging with `INITO` upon success.
2. When the AR socket `IN` receives an event on `IN.E1`, the REAL value at `IN.D1` is latched into the internal `E_D_FF_ANY` flip-flop.
3. The flip-flop holds the value stable and generates event `EO`.
4. `EO` triggers `F_MOVE` (`REAL` data type) to supply `CLIENT_1_0.SD_1` and fire the send event `REQ` on `CLIENT_1_0`.
5. After successful transmission, `CLIENT_1_0` confirms with `CNF`.

## Technical Features

- **Buffering with E_D_FF_ANY**: The REAL value to be sent is buffered using an internal `E_D_FF_ANY`, preventing value instability during transmission.
- **Remote Write / Call Method**: `CLIENT_1_0` directly addresses a remote OPC UA server to transfer analog sensor readings or setpoints.
- **Encapsulation**: The raw `CLIENT_1_0` event/data interface is kept internal; only the clean AR adapter interface is exposed.

## State Overview

1. **Uninitialized**: The block waits for the `INIT` event.
2. **Initialized**: Connection to the remote server is established; ready to send.
3. **Send Active**: An event on the AR socket buffers the REAL value and triggers the remote write.

## Application Scenarios

- **Remote Sensor Data Transmission**: Sending analog values (e.g. pressure, temperature, position) to a remote controller or HMI via OPC UA.
- **Modular Control Architectures**: Integrating distributed REAL values into libraries built consistently on AR adapters.

## Comparison with Similar Blocks

- **AR_CLIENT_1_0_UNGATED**: Omits the internal `E_D_FF_ANY` flip-flop (change filter) and triggers the remote write or method call unconditionally on every `IN.E1` event — ideal for repeated VT/HMI commits of the same value.
- **AX_CLIENT_1_0**: Identical structure, but handles BOOL values via an AX adapter.
- **ATM_CLIENT_1_0**: Identical structure, but handles TIME values via an ATM adapter.
- **AR_PUBLISH_1**: Encapsulates `PUBLISH_1` instead of `CLIENT_1_0` for local publish/subscribe.

## Conclusion

**AR_CLIENT_1_0** bridges connection-oriented OPC UA remote writing with adapter-based REAL data handling.
