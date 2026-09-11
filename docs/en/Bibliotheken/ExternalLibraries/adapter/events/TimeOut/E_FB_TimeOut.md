# E_FB_TimeOut

![E_FB_TimeOut](./E_FB_TimeOut.svg)

* * * * * * * * * *
## Introduction

The `E_FB_TimeOut` function block is a composite implementation of a timeout service. It provides a simple, reusable delay/timeout mechanism that can be controlled and observed through adapter sockets. The block is designed to behave in a way similar to an `FB_TON` but with a cyclic interface for querying the timeout status and elapsed time.

The functionality is implemented by an internally embedded `E_FB_DELAY` block. The external interface consists exclusively of two adapter sockets: one for controlling the timeout operation and one for cyclic time-tick queries.

## Interface Structure

`E_FB_TimeOut` does not expose direct event or data inputs/outputs. All communication with the outside world happens through its two adapter sockets. This makes the block well suited for adapter-based, decoupled system architectures.

### **Event Inputs**

The following event inputs are available through the adapter sockets:

| Event | Via Adapter | Connected to | Description |
|---|---|---|---|
| `START` | `TimeOutSocket` (`ATimeOut`) | `DLY.START` | Starts the timeout delay. |
| `STOP` | `TimeOutSocket` (`ATimeOut`) | `DLY.STOP` | Stops or cancels the running timeout. |
| `REQ` | `TimeTickSocket` (`ATimeTick`) | `DLY.REQ` | Requests the current timeout status and time values. |

### **Event Outputs**

The following event outputs are available through the adapter sockets:

| Event | Via Adapter | Connected from | Description |
|---|---|---|---|
| `TimeOut` | `TimeOutSocket` (`ATimeOut`) | `DLY.EO` | Emitted when the configured timeout duration has elapsed. |
| `CNF` | `TimeTickSocket` (`ATimeTick`) | `DLY.CNF` | Confirms a `REQ` query and provides updated status data. |
| `STARTO_IN` | `TimeTickSocket` (`ATimeTick`) | `DLY.STARTO` | Indicates that the timeout has been started. |
| `STOPO_IN` | `TimeTickSocket` (`ATimeTick`) | `DLY.STOPO` | Indicates that the timeout has been stopped. |

### **Data Inputs**

The following data input is available through the adapter sockets:

| Data | Via Adapter | Connected to | Description |
|---|---|---|---|
| `DT` | `TimeOutSocket` (`ATimeOut`) | `DLY.DT` | Defines the delay time, i.e. the timeout duration. |

### **Data Outputs**

The following data outputs are available through the adapter sockets:

| Data | Via Adapter | Connected from | Description |
|---|---|---|---|
| `ET` | `TimeTickSocket` (`ATimeTick`) | `DLY.ET` | Elapsed time since the timeout was started. |
| `Q` | `TimeTickSocket` (`ATimeTick`) | `DLY.Q` | Status output indicating the delay/timeout state. |
| `PT` | `TimeTickSocket` (`ATimeTick`) | `DLY.PT` | Preset time value, typically the configured timeout duration. |

### **Adapters**

| Adapter | Type | Direction | Description |
|---|---|---|---|
| `TimeOutSocket` | `iec61499::events::ATimeOut` | Socket | Provides the standard event-based timeout interface: `START`, `STOP`, `TimeOut`, and `DT`. |
| `TimeTickSocket` | `adapter::events::TimeOut::ATimeTick` | Socket | Provides a cyclic time-tick interface for querying `ET`, `Q`, and `PT` and for observing start/stop events. |

## Functionality

The `E_FB_TimeOut` block implements a timeout service using an internal `E_FB_DELAY` instance. The internal delay block is responsible for all timing behavior. The outer block only maps events and data between the external adapters and the internal delay block.

When a `START` event is received on the `TimeOutSocket`, the internal delay is started with the duration specified by `DT`. If no `STOP` event is received before the delay expires, the internal delay produces its output event, which is forwarded as the `TimeOut` event on `TimeOutSocket`.

When a `STOP` event is received, the internal delay is stopped and the timeout is cancelled. No `TimeOut` event is emitted in that case.

The `TimeTickSocket` provides a cyclic, query-oriented access path to the same internal delay. A `REQ` event triggers the internal delay to produce a confirmation event `CNF`. At the same time, the current values of `ET`, `Q`, and `PT` are made available on the `TimeTickSocket`. Additionally, `STARTO_IN` and `STOPO_IN` provide event-based notifications when the timeout has been started or stopped.

Because the timeout logic is contained in the internal `E_FB_DELAY`, the external behavior remains predictable and can be reused in different adapter-based applications.

## Technical Features

- Composite function block; no internal ECC or algorithms are required.
- Uses an internal `E_FB_DELAY` block for the actual timing logic.
- No direct event inputs, event outputs, data inputs, or data outputs outside the adapter interface.
- Provides two adapter sockets:
  - Standard `iec61499::events::ATimeOut` adapter.
  - Custom `adapter::events::TimeOut::ATimeTick` adapter.
- Supports both event-driven timeout handling and cyclic polling/querying of status data.
- Designed as an FB_TON-like interface with cyclic behavior.
- Decoupled architecture: the timeout service can be connected to other FBs using matching plug adapters.

## State Overview

Since `E_FB_TimeOut` is a composite block, it does not contain an explicit state machine. The state behavior is delegated to the internal `E_FB_DELAY`. Conceptually, the block can be considered in the following states:

- **Idle:** No timeout is active. The internal delay has not been started.
- **Timing:** A `START` event has been received and the internal delay is counting down. `STOP` can cancel the timing operation.
- **Timeout Expired:** The configured delay time has elapsed, and the `TimeOut` event has been emitted.
- **Stopped:** A `STOP` event has cancelled the timeout before expiration.

The `Q`, `ET`, and `PT` outputs made available through the `TimeTickSocket` allow an external cyclic caller to observe the current state and timing information.

## Application Scenarios

`E_FB_TimeOut` is suitable for applications where a timeout service must be controlled and observed through adapters. Typical scenarios include:

- Communication protocol timeout supervision.
- Cyclic monitoring of process or machine states.
- Integration in adapter-based IEC 61499 applications where the timeout service should be reusable and interchangeable.
- Event-driven timeout control with additional periodic status polling.
- Replacing direct `FB_TON`-like behavior in a distributed or modular control system.

## Comparison with Similar Blocks

Compared to a standard `E_DELAY` or a direct `FB_TON`-style block, `E_FB_TimeOut` has a different interface style:

- It does not expose plain event/data inputs and outputs; instead, it uses adapter sockets.
- It separates the timeout control interface (`ATimeOut`) from the cyclic query interface (`ATimeTick`).
- It provides both an event-driven timeout notification and a cyclic query mechanism.
- It is more modular and can be connected to other adapter-compatible FBs without hard-wired event connections.

Compared to a direct `FB_TON`, the behavior is similar in terms of delay/timeout handling, but the interface is designed around events and adapters rather than Boolean trigger inputs and continuous time monitoring.

## Conclusion

The `E_FB_TimeOut` function block provides a clean, adapter-based timeout service. By combining an internal delay FB with two adapter sockets, it offers both event-driven timeout signaling and cyclic status querying. Its FB_TON-like behavior and modular structure make it useful in a wide range of IEC 61499 applications where timeouts must be controlled, observed, and reused in a decoupled way.