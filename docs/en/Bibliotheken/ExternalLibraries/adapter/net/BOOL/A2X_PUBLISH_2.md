# A2X_PUBLISH_2

![A2X_PUBLISH_2](./A2X_PUBLISH_2.svg)

* * * * * * * * * *
## Introduction

The **A2X_PUBLISH_2** function block is a network publishing component that takes the two Boolean values (`UP` and `DOWN`) from an A2X adapter socket and publishes them via a standard `PUBLISH_2` block to one or more `SUBSCRIBE_2` counterparts. Each published value is buffered internally with an edge-triggered `E_D_FF` flip-flop, ensuring that only state changes trigger a network transmission, reducing unnecessary network traffic. The block is designed for scenarios where a single publisher must distribute a pair of Boolean signals (e.g., up/down commands) to multiple subscribers over Ethernet.

## Interface Structure

### **Event Inputs**

| Event | Type | With Variables | Comment |
|-------|------|----------------|---------|
| `INIT` | `EInit` | `QI`, `ID` | Initialization of the publish connection. |

### **Event Outputs**

| Event | Type | With Variables | Comment |
|-------|------|----------------|---------|
| `INITO` | `EInit` | `QO`, `STATUS` | Initialization confirmation. |
| `CNF` | `Event` | `QO`, `STATUS` | Confirmation that data was sent. |

### **Data Inputs**

| Variable | Type | Comment |
|----------|------|---------|
| `QI` | `BOOL` | Quality / enable input. When `TRUE`, the block is active. |
| `ID` | `WSTRING` | Remote endpoint identifier (e.g., IP address and port) for the publish channel. |

### **Data Outputs**

| Variable | Type | Comment |
|----------|------|---------|
| `QO` | `BOOL` | Quality / confirmed output. Reflects the active state after initialization. |
| `STATUS` | `WSTRING` | Status information of the last operation (e.g., error or success message). |

### **Adapters**

| Direction | Name | Type | Comment |
|-----------|------|------|---------|
| Socket | `IN` | `adapter::types::unidirectional::A2X` | Provides the two BOOL values `UP` and `DOWN` to be published. |

## Functionality

The `A2X_PUBLISH_2` block combines an A2X adapter with an internal `PUBLISH_2` network block and two edge-triggered D-flip-flops (`E_D_FF_UP` and `E_D_FF_DOWN`).

1. **Data capture**: The Boolean values `IN.UP` and `IN.DOWN` are continuously wired to the `D` inputs of two flip-flops.
2. **Edge detection**: When an event `IN.E_UP` occurs, the flip-flop `E_D_FF_UP` latches the current `UP` value onto its output `Q`. The same applies to `E_D_FF_DOWN` when `IN.E_DOWN` occurs.
3. **Trigger publishing**: On every rising edge at either flip-flop (the `EO` output), the internal `PUBLISH_2` block receives a `REQ` event, capturing the latched values into its `SD_1` and `SD_2` data inputs.
4. **Network transmission**: The `PUBLISH_2` block sends the two values to the configured endpoint (`ID`). When successful, it emits the `CNF` event and updates `QO` and `STATUS`.
5. **Initialization**: The `INIT` event initializes the underlying `PUBLISH_2` connection. The result is reported via `INITO`, `QO`, and `STATUS`.

The internal buffering ensures that repeated identical values do not trigger unnecessary transmissions. Only actual changes of the `UP` or `DOWN` signals cause the publish operation.

## Technical Features

- **Edge-triggered buffering**: Both input signals are latched in `E_D_FF` elements, ensuring that the last state is retained until a new edge appears.
- **Single publish channel**: The two buffered values are transmitted together in one `PUBLISH_2` operation, minimizing network overhead.
- **Standard 61499-compliant**: Built from standard `iec61499` function blocks (`E_D_FF`, `PUBLISH_2`).
- **Unidirectional adapter**: Designed for one-way communication from the publisher to subscribers.
- **Status feedback**: `QO` and `STATUS` provide clear reporting of initialization and transmission results.

## State Overview

The block behaves as follows with respect to its internal flip-flops:

| State | Condition | Behavior |
|-------|-----------|----------|
| Initial | After `INIT`, no event received | Flip-flops hold their default (`FALSE`). No publish trigger occurs. |
| Idle | No edge on `E_UP` or `E_DOWN` | Flip-flop outputs unchanged; no publication triggered. |
| Up-edge | `IN.E_UP` rises | `UP` value latched; `E_D_FF_UP.EO` triggers `PUBLISH_2.REQ`. |
| Down-edge | `IN.E_DOWN` rises | `DOWN` value latched; `E_D_FF_DOWN.EO` triggers `PUBLISH_2.REQ`. |
| Publishing | `REQ` emitted | `PUBLISH_2` sends `SD_1` and `SD_2` to the network. |
| Confirmed | `CNF` emitted | Outputs `QO` and `STATUS` are updated; the block returns to idle. |

The flip-flops retain their last value indefinitely until the next relevant edge, meaning the last published state is always remembered.

## Application Scenarios

- **Machine control**: Publishing two limit-switch states (up/down) from one controller to several HMIs or PLCs.
- **Remote I/O**: Distributing two Boolean sensor values to all subscribers in a multicast group.
- **Building automation**: Broadcasting window open/close or door up/down commands to multiple control panels.
- **Rapid status distribution**: Any application where a single source must notify several consumers of a Boolean pair, with network efficiency achieved by transmitting only on state changes.

## Comparison with Similar Blocks

| Block | Key Difference |
|-------|----------------|
| `A2X_PUBLISH_2` | Buffers adapter BOOLs in `E_D_FF` and publishes only on change. |
| Plain `PUBLISH_2` | Publishes directly from data inputs with no internal edge detection; every `REQ` sends data. |
| `A2X_SUBSCRIBE_2` | Receives instead of sends, no buffering of edge events. |
| `A2X_PUBLISH_2` with manual buffering | Requires external `E_D_FF` instances; this block encapsulates them for convenience. |

The main advantage over a direct `PUBLISH_2` usage is the automatic edge-triggered buffering, which reduces network load and simplifies application logic.

## Conclusion

The **A2X_PUBLISH_2** function block provides a compact, efficient solution for publishing a pair of Boolean signals from an A2X adapter to multiple subscribers. By integrating edge-triggered buffers with a standard publish block, it ensures that only meaningful state changes are transmitted, reducing network traffic and simplifying the application design. It is well suited for distributed automation systems where reliable and efficient status dissemination is required.