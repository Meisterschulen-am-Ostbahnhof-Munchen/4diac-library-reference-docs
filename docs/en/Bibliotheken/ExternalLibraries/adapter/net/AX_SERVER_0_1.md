# AX_SERVER_0_1

* * * * * * * * * *

## Introduction

The **AX_SERVER_0_1** function block is a composite function block from the `adapter::net` package. It encapsulates the network server block `iec61499::net::SERVER_0_1` and exposes incoming OPC UA method calls (`CALL_METHOD`) for **BOOL** arguments as a unidirectional **AX adapter** (`OUT`). It serves as the receiving counterpart to **AX_CLIENT_1_0** (or `CLIENT_1_0`).

## Interface Structure

### **Event Inputs**

- **INIT** (EInit): Initialization event, connected to `QI` and `ID` (forwards to `SERVER_0_1.INIT`)

### **Event Outputs**

- **INITO** (EInit): Initialization confirmation (`SERVER_0_1.INITO`), connected to `QO` and `STATUS`

### **Data Inputs**

- **QI** (BOOL): Input qualifier, enables (TRUE) or disables (FALSE) the server
- **ID** (WSTRING): Server identifier (OPC UA server address and method specification)

### **Data Outputs**

- **QO** (BOOL): Output qualifier, server operational status
- **STATUS** (WSTRING): Status information as a Unicode string

### **Adapters**

| Adapter | Type                                | Direction    | Description                                     |
| ------- | ---------------------------------- | ------------ | ----------------------------------------------- |
| OUT     | adapter::types::unidirectional::AX | Plug (Output)| Received BOOL value on each method invocation   |

## Functionality

1. An `INIT` event at `AX_SERVER_0_1` is forwarded to the internal `SERVER_0_1.INIT` input. The server starts the OPC UA endpoint and acknowledges with `INITO` upon success.
2. Upon an incoming method call from a remote client (`CLIENT_1_0`/`AX_CLIENT_1_0`), `SERVER_0_1` emits indication event `IND` and presents the received BOOL value at `RD_1`.
3. To prevent response-latency lockups (RSP latency bug), `SERVER_0_1.IND` is directly looped back to `SERVER_0_1.RSP` within the internal network, causing the server to acknowledge the method immediately.
4. Simultaneously, `IND` passes the received value through `F_MOVE` (`BOOL` data type) to AX plug `OUT.D1` and fires output event `OUT.E1`.

## Technical Features

- **Direct RSP Loopback**: To eliminate 4-second server lockups caused by delayed responses, `SERVER_0_1.IND` is internally tied back to `SERVER_0_1.RSP`.
- **Seamless Adapter Integration**: The raw `RD_1` output of `SERVER_0_1` is mapped via `F_MOVE` (BOOL) directly onto the AX adapter structure (`OUT.D1` / `OUT.E1`).
- **Receiving Counterpart to `AX_CLIENT_1_0`**: In IEC 61499, a `CLIENT_1_0` block ("Connect to a SERVER_0_1 block") pairs with a `SERVER_0_1` block.

## State Overview

1. **Uninitialized**: The server is inactive and waiting for the `INIT` event.
2. **Listening**: The OPC UA server is active and waiting for method calls from remote clients.
3. **Indication**: A method call arrives; the BOOL value is passed to the `OUT` adapter and the response is sent immediately.

## Application Scenarios

- **Receiving Remote Control Commands**: Receiving discrete switching commands (e.g. start/stop, enable signals) from a remote module via OPC UA `CALL_METHOD`.
- **Modular Server Architectures**: Providing OPC UA method endpoints for Boolean signals in adapter-based applications.

## Comparison with Similar Blocks

- **AR_SERVER_0_1**: Receives REAL values via an AR adapter.
- **ATM_SERVER_0_1**: Receives TIME values via an ATM adapter.
- **AX_SUBSCRIBE_1**: Encapsulates `SUBSCRIBE_1` for local publish/subscribe rather than method invocations.

## Conclusion

**AX_SERVER_0_1** delivers received OPC UA method invocations for BOOL values over a clean AX adapter while preventing server response locks.
