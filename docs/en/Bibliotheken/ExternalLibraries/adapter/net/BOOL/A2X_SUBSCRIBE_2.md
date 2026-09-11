# A2X_SUBSCRIBE_2

![A2X_SUBSCRIBE_2](./A2X_SUBSCRIBE_2.svg)

* * * * * * * * * *

## Introduction

The **A2X_SUBSCRIBE_2** function block is a composite network subscriber designed to receive two Boolean values from a remote **PUBLISH_2** block via an Ethernet-based publish/subscribe mechanism. It internally buffers the two received BOOL values using dedicated D flip-flops (E_D_FF) and delivers them — together with corresponding event signals — through a unidirectional A2X adapter plug for further processing in the application logic.

This block is particularly useful in distributed IEC 61499 systems where two independent Boolean state variables (e.g., UP and DOWN commands) must be reliably received, synchronized, and made available to a local consumer in a structured and event-driven manner.

## Interface Structure

### **Event Inputs**

| Event Name | Type | Associated Data | Description |
|-------------|------|-----------------|-------------|
| `INIT` | EInit | `QI`, `ID` | Initialization event that configures the underlying subscription with the quality indicator and the communication identifier. |
| `RSP` | Event | `QI` | Response event used to acknowledge or re-trigger the subscription process after a previous request. |

### **Event Outputs**

| Event Name | Type | Associated Data | Description |
|-------------|------|-----------------|-------------|
| `INITO` | EInit | `QO`, `STATUS` | Initialization confirmation, indicating whether the subscription was successfully established. |
| `IND` | Event | `QO`, `STATUS` | Indicates that new data has been received from the publisher and is available at the adapter outputs. |

### **Data Inputs**

| Data Name | Type | Description |
|-----------|------|-------------|
| `QI` | BOOL | Quality indicator enabling (`TRUE`) or disabling (`FALSE`) the subscription operation. |
| `ID` | WSTRING | Communication identifier (e.g., multicast address or group ID) used to bind the subscriber to the correct publisher. |

### **Data Outputs**

| Data Name | Type | Description |
|-----------|------|-------------|
| `QO` | BOOL | Quality output confirming that the subscription is active and operational. |
| `STATUS` | WSTRING | Status information string providing runtime diagnostics or error messages. |

### **Adapters**

| Adapter Name | Direction | Type | Description |
|---------------|-----------|------|-------------|
| `OUT` | Plug | `adapter::types::unidirectional::A2X` | Buffered output adapter delivering the two received Boolean values (`UP`, `DOWN`) together with their associated event indicators (`E_UP`, `E_DOWN`). |

## Functionality

The **A2X_SUBSCRIBE_2** block operates as follows:

1. **Initialization**: Upon receiving the `INIT` event, the internal `SUBSCRIBE_2` network subscriber is configured using the `QI` enable flag and the `ID` communication identifier. The `INITO` event confirms successful initialization with updated `QO` and `STATUS` values.

2. **Data Reception**: When the underlying `SUBSCRIBE_2` network service receives data from a matching publisher, it raises the `IND` event. This event triggers three parallel actions:
   - The newly received `RD_1` value is latched into the `E_D_FF_UP` D flip-flop.
   - The newly received `RD_2` value is latched into the `E_D_FF_DOWN` D flip-flop.
   - The `IND` event is propagated to the block's external event output to signal that fresh data is available.

3. **Buffered Output**: The latched values from both flip-flops are continuously driven onto the adapter outputs (`OUT.UP` and `OUT.DOWN`). Correspondingly, the flip-flop event outputs (`EO`) are connected to `OUT.E_UP` and `OUT.E_DOWN`, providing event-driven notifications for each data change.

4. **Response Handling**: The `RSP` event input allows the application to explicitly trigger a new subscription request or acknowledgement cycle through the internal subscriber, with the `QI` value supplied for re-evaluation.

## Technical Features

- **Composite implementation**: Built as a network of three internal function blocks (`SUBSCRIBE_2`, `E_D_FF_UP`, `E_D_FF_DOWN`), demonstrating reusable modular design.
- **Ethernet-based communication**: Uses the standard IEC 61499 network `SUBSCRIBE_2` service type for reliable remote data exchange.
- **Data buffering**: Each incoming Boolean value is latched into an edge-triggered D flip-flop, ensuring stable output values between publication cycles.
- **Event-driven architecture**: Both the external `IND` event and the internal adapter events (`E_UP`, `E_DOWN`) are generated based on the reception trigger, allowing downstream logic to react immediately.
- **Standardized status reporting**: `QO` and `STATUS` provide consistent quality and diagnostic information for monitoring and error handling.
- **Unidirectional data flow**: The A2X adapter plug strictly follows a publisher-to-subscriber direction, simplifying data flow analysis.

## State Overview

The block does not maintain an explicit internal state machine in the composite network, but its behaviour can be described in terms of operational phases:

| Phase | Description | Trigger |
|-------|-------------|---------|
| **Idle** | Subscription is inactive; `QO = FALSE`. | Before `INIT` or after failed initialization |
| **Initializing** | `INIT` received; internal subscriber is being configured. | `INIT` event edge |
| **Active / Subscribed** | Subscription established; `QO = TRUE`; waiting for data. | Successful `INITO` with valid status |
| **Data Received** | New publication arrived; flip-flop latches are updated; `IND` raised. | Internal `SUBSCRIBE_2.IND` |
| **Re-Trigger** | Application requested a new subscription cycle via `RSP`. | `RSP` event edge |

## Application Scenarios

Typical use cases for **A2X_SUBSCRIBE_2** include:

- **Distributed control systems**: Receiving two independent Boolean control commands (e.g., UP/DOWN, START/STOP) from a central controller over Ethernet and forwarding them to local actuator logic.
- **Remote status monitoring**: Acquiring two-status-bit telemetry (e.g., valve open/closed, sensor alarm/normal) from a remote field device and presenting them to a local HMI or processing unit.
- **Command buffering in safety-related applications**: Latching incoming commands to guarantee stable output levels even if network interruptions occur between publication cycles.
- **Multi-node coordination**: Acting as a reception front-end in a star or multicast network where one publisher serves multiple subscribers.

## Comparison with Similar Blocks

| Feature | A2X_SUBSCRIBE_2 | Standard SUBSCRIBE_2 | SUBSCRIBE_1 |
|---------|-----------------|----------------------|-------------|
| **Data capacity** | 2 BOOL values | 2 BOOL values (`RD_1`, `RD_2`) | 1 value (any type) |
| **Output format** | Buffered via D flip-flops, delivered through A2X adapter | Raw output variables | Raw output variable |
| **Event granularity** | Separate events per buffered channel (`E_UP`, `E_DOWN`) plus common `IND` | Single `IND` event | Single `IND` event |
| **Integration convenience** | Direct plug connection to A2X adapter sockets | Requires manual connection of data/events | Requires manual connection |
| **Data stability** | Continues to hold last values after reception | Outputs follow last received values (no explicit latching) | Outputs follow last received values |
| **Use case fit** | Application-oriented ready-to-connect subscriber | Low-level network service | Generic single-value network reception |

Compared to the raw `SUBSCRIBE_2` network FB, **A2X_SUBSCRIBE_2** adds buffering, structured event distribution, and an adapter-based interface that simplifies integration into higher-level application designs.

## Conclusion

The **A2X_SUBSCRIBE_2** function block provides a clean, ready-to-use solution for receiving two Boolean values over an Ethernet-based publish/subscribe network and delivering them to the application layer through a standardized A2X adapter interface. By combining network subscription with D flip-flop buffering and dedicated per-channel events, it significantly reduces the design effort for distributed control applications while enhancing data stability and clarity. Its modular composite implementation follows best practices of IEC 61499, making it a valuable building block in industrial automation and decentralized control scenarios.
