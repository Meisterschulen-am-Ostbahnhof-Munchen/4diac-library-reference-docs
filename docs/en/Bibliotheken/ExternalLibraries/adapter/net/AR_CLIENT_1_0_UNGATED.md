# AR_CLIENT_1_0_UNGATED

* * * * * * * * * *

## Introduction

The **AR_CLIENT_1_0_UNGATED** function block is a composite function block from the `adapter::net` package. It encapsulates the network-based `CLIENT_1_0` function block from the IEC 61499 standard library and maps its interface to a unidirectional **AR adapter** (`IN`, `REAL` data type).

Unlike **AR_CLIENT_1_0**, this block **does not** contain an internal `E_D_FF_ANY` value filter (Send-on-Change). Every incoming event on `IN.E1` triggers a send request to `CLIENT_1_0.REQ` **unconditionally and without filtering**.

## Interface Structure

### **Event Inputs**

- **INIT** (EInit): Initialization event, connected to `QI` and `ID`

### **Event Outputs**

- **INITO** (EInit): Initialization confirmation, connected to `QO` and `STATUS`
- **CNF** (Event): Confirmation that data was sent, connected to `QO` and `STATUS`

### **Data Inputs**

- **QI** (BOOL): Input qualifier, opens (TRUE) or closes (FALSE) the connection to the server
- **ID** (WSTRING): Connection identifier (OPC UA address of the target node, e.g., `opc_ua[CALL_METHOD;opc.tcp://192.168.1.12:4840#;...]`)

### **Data Outputs**

- **QO** (BOOL): Output qualifier, connection status
- **STATUS** (WSTRING): Status information as a Unicode string

### **Adapters**

| Adapter | Type                                | Direction     | Description                                                     |
| ------- | ---------------------------------- | ------------- | --------------------------------------------------------------- |
| IN      | adapter::types::unidirectional::AR | Socket (Input)| REAL value to be sent (transmitted on every `IN.E1` trigger)   |

## Functionality

1. The `INIT` event initializes the internal `CLIENT_1_0` block with `QI` and `ID`, acknowledging with `INITO` upon success.
2. When the AR socket `IN` receives an event on `IN.E1`, the event is forwarded **directly** to `F_MOVE.REQ`.
3. `F_MOVE` (`REAL`) receives the REAL value present at `IN.D1`, passes it to `CLIENT_1_0.SD_1`, and triggers `CLIENT_1_0.REQ`.
4. The `CLIENT_1_0` block executes the OPC UA remote write or method call and confirms with `CNF`.

## Technical Features

- **Unfiltered Event Triggering (Ungated)**: Because no `E_D_FF_ANY` flip-flop is included, no change filter is applied. Even if value `IN.D1` is identical to the previously transmitted value, a new `IN.E1` event forces execution.
- **Tailored for OPC UA CALL_METHOD / Remote Input**: Essential for user-interface controls (e.g. VT numeric inputs or Web HMIs) where re-committing the same numeric value must reliably invoke the remote RPC method every time.
- **Decoupled from Change-Filter Logic**: While `AR_CLIENT_1_0` is optimized for continuous send-on-change use cases, `AR_CLIENT_1_0_UNGATED` serves event-driven method execution.

## State Overview

1. **Uninitialized**: The block waits for the `INIT` event.
2. **Initialized**: Connection to the server is established; ready to transmit.
3. **Send Active**: Each `IN.E1` event immediately triggers transmission via `CLIENT_1_0`.

## Application Scenarios

- **Remote Method Invocations via VT/HMI**: E.g. `NumericValue_TO_CLIENT_1_0_OPC.SUB` — when an operator confirms a numeric entry on the VT, the OPC UA method must be invoked even if the entered number is unchanged.
- **Event-Driven Command Transmission**: Transferring REAL parameters where the timing of the trigger event conveys independent meaning.

## Comparison with Similar Blocks

- **AR_CLIENT_1_0**: Contains an internal `E_D_FF_ANY` flip-flop that filters out unchanged values (Send-on-Change). `AR_CLIENT_1_0_UNGATED` deliberately omits this filter.
- **AX_CLIENT_1_0**: Handles BOOL values with a change filter.
- **ATM_CLIENT_1_0**: Handles TIME values with a change filter.

## Conclusion

**AR_CLIENT_1_0_UNGATED** ensures that every trigger event on the AR adapter interface results in an OPC UA transmission — ideal for remote method calls in HMI and VT applications.
