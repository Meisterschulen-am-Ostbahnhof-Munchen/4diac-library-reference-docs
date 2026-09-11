# SRT_RPC_FROM_Remote_QXA_OPC


![SRT_RPC_FROM_Remote_QXA_OPC_network](./SRT_RPC_FROM_Remote_QXA_OPC_network.svg)

![SRT_RPC_FROM_Remote_QXA_OPC](./SRT_RPC_FROM_Remote_QXA_OPC.svg)

* * * * * * * * * *

## Introduction

SRT_RPC_FROM_Remote_QXA_OPC is a composite subapplication designed for device B (Station 12, IP 192.168.1.12) that receives Set, Reset, and Toggle commands via three independent OPC-UA server method calls. Unlike approaches that rely on value-change tricks or bridging, this subapp uses pure RPC triggers — each command is delivered through its own `SERVER_0` method invocation initiated by device A.

The subapp contains the actual flip-flop logic based on the `AX_T_FF_SR` adapter, which combines SR and toggle behavior. The resulting state drives a physical digital output (`DigitalOutput_Q1`) and is actively written back to device A via an `AX_CLIENT_1_0` client, ensuring that the central controller always has the current output state. The entire protocol is encapsulated within the `MyLib::sys` composite, following a "SUB style" design where the resource of the device remains free of protocol-specific logic.

## Interface Structure

The subapplication exposes only data inputs at its boundary; it has no event inputs, no event outputs, no data outputs, and no external adapters.

### **Event Inputs**

None.

### **Event Outputs**

None.

### **Data Inputs**

| Name | Type | Initial Value | Description |
|------|------|---------------|-------------|
| `Output` | `logiBUS::io::DQ::logiBUS_DO_S` | `logiBUS_DO::Invalid` | Identifies the target digital output (Output_Q1..Q8). |
| `ID_SET_METHOD` | `WSTRING` | — | Local method address (ACTION=CREATE_METHOD) for the Set method, invoked by device A via CALL_METHOD. |
| `ID_RESET_METHOD` | `WSTRING` | — | Local method address (ACTION=CREATE_METHOD) for the Reset method, invoked by device A via CALL_METHOD. |
| `ID_TOGGLE_METHOD` | `WSTRING` | — | Local method address (ACTION=CREATE_METHOD) for the Toggle method, invoked by device A via CALL_METHOD. |
| `ID_STATE_WRITE` | `WSTRING` | — | Remote target address (BOOL, ACTION=WRITE) for writing the flip-flop state back to device A. |

### **Data Outputs**

None.

### **Adapters**

None at the subapplication interface. All adapter connections are internal to the network.

## Functionality

The subapp implements a complete remote-controlled SR/T flip-flop with output feedback:

1. **Command reception**: Three `SERVER_0` instances (`TRIGGER_SET_SERVER`, `TRIGGER_RESET_SERVER`, `TRIGGER_TOGGLE_SERVER`) each listen for a specific OPC-UA method call. Device A invokes these methods remotely to send Set, Reset, or Toggle commands.

2. **Flip-flop logic**: The incoming event from each server is forwarded to the `AX_T_FF_SR` block:
   - Set event (`TRIGGER_SET_SERVER.IND`) drives the `S` input → sets the output `Q` to TRUE.
   - Reset event (`TRIGGER_RESET_SERVER.IND`) drives the `R` input → sets the output `Q` to FALSE.
   - Toggle event (`TRIGGER_TOGGLE_SERVER.IND`) drives the `CLK` input → inverts the current `Q` state.

3. **Immediate response**: Each server's `IND` event is directly wired back to its own `RSP` output. This ensures the OPC-UA server thread is released instantly, preventing long blocking periods (a known issue fixed in version 1.1).

4. **Signal distribution**: The flip-flop output `Q` is fed into `AX_SPLIT_2`, which fans it out to two consumers:
   - **OUT1** → `DigitalOutput_Q1` (`logiBUS_QXA`) — switches the physical digital output on the logiBUS device.
   - **OUT2** → `STATE_CLIENT` (`AX_CLIENT_1_0`) — sends the new state back to device A via an OPC-UA write operation at the address specified by `ID_STATE_WRITE`.

5. **Configuration inputs**: All method addresses and the output channel are configured via the subapp's data inputs, making the block reusable for different method IDs and output channels without modifying the internal structure.

## Technical Features

- **RPC-based trigger**: Commands are delivered as explicit OPC-UA method calls, eliminating the need for value-change detection tricks or intermediate bridge FBs.
- **Tailored server instances**: Separate `SERVER_0` for Set, Reset, and Toggle allows independent method addresses and clear segregation of control semantics.
- **Non-blocking response handling**: The `IND → RSP` direct connection prevents the open62541 server thread from being held, avoiding global `serviceMutex` contention (critical fix in version 1.1 that improved project-wide OPC-UA responsiveness).
- **Combined SR + T flip-flop**: The `AX_T_FF_SR` adapter provides both deterministic Set/Reset behavior and toggle functionality in one compact element.
- **Active state feedback**: The current flip-flop state is actively written back to the controlling device, ensuring synchronized state awareness without poll requests.
- **Protocol encapsulation**: All communication logic resides within the `MyLib::sys` composite, keeping the device resource clean and the subapp portable across resources and devices.
- **Structured configuration**: All network addresses and output identifiers are externalized as WSTRING/struct inputs, enabling easy adaptation to different OPC-UA topologies.

## State Overview

The core state machine is implemented by the `AX_T_FF_SR` block, which operates as follows:

| Input Event | Condition | Resulting State (Q) |
|-------------|-----------|---------------------|
| Set (`S`) | Always | `TRUE` |
| Reset (`R`) | Always | `FALSE` |
| Toggle (`CLK`) | Rising edge | Invert current Q: `TRUE → FALSE` or `FALSE → TRUE` |

- **Initial state**: `FALSE` (after startup or explicit Reset).
- **Stable states**: `TRUE` (output active) or `FALSE` (output inactive), with no undefined or metastable conditions.
- **Conflict handling**: If Set and Reset arrive simultaneously, priority behavior follows the `AX_T_FF_SR` adapter semantics (typically Reset dominates, depending on the underlying implementation).

The `Q` output is then split and routed to both the digital output and the remote feedback client, so the observed physical state always matches the state reported back to device A.

## Application Scenarios

- **Remote output control**: A central controller (device A) remotely sets, resets, or toggles digital output Q1 on a field device (device B) over OPC-UA.
- **Distributed automation networks**: Coordinating outputs across multiple stations (e.g., Station 12 in a larger plant) where each station hosts its own OPC-UA server and receives commands on demand.
- **State-synchronized systems**: Scenarios where the controller must know the exact output state after each command — the active write-back via `CLIENT_1_0` guarantees this without extra polling.
- **Integration in machine safety or indicator logic**: Toggle functionality is useful for alternating signals (blinking lamps, periodic valve switching), while Set/Reset provides deterministic latching.

## Comparison with Similar Blocks

| Feature | SRT_RPC_FROM_Remote_QXA_OPC | Typical local I/O FB | Bridge-based remote FB |
|---------|-----------------------------|----------------------|------------------------|
| Command transport | OPC-UA method calls (RPC) | Local event connections | Value-change signals |
| State feedback | Active write-back via client | Direct wiring | Requires extra feedback FB |
| Server thread blocking | Avoided (IND→RSP direct) | N/A | Can block if RSP unconnected |
| Protocol location | Inside subapp (SUB style) | In resource | In resource |
| Reusability | High — copy subapp between devices | Low — tied to resource | Medium |
| Toggle capability | Built-in (`AX_T_FF_SR`) | Usually only Set/Reset | Depends on logic |

Compared to a bridge-based solution that translates value changes into commands, this subapp provides a cleaner, semantically richer interface. Compared to placing the protocol in the device resource, the SUB style allows the same communication pattern to be reused across multiple devices without duplicating resource-level wiring.

## Conclusion

SRT_RPC_FROM_Remote_QXA_OPC is a well-structured subapplication that encapsulates remote OPC-UA-based control of a digital output in a compact, reusable composite. Its three dedicated server instances allow precise Set, Reset, and Toggle semantics through pure RPC calls, while the direct `IND→RSP` wiring ensures responsive OPC-UA behavior. The integrated `AX_T_FF_SR` flip-flop provides reliable state management, and the active feedback via `CLIENT_1_0` keeps the controlling device synchronized. By keeping all protocol logic inside the subapp, the design enhances modularity, reusability, and maintainability across distributed IEC 61499 applications.
