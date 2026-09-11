# E_FB_TimeOut

![E_FB_TimeOut](./E_FB_TimeOut.svg)

* * * * * * * * * *

## Introduction

E_FB_TimeOut is a composite IEC 61499 function block that provides a simple timeout service. It wraps an internal timer instance and exposes its functionality through two adapter sockets:

- `TimeOutSocket` for starting, stopping, and receiving timeout events.
- `TimeTickSocket` for cyclic querying and monitoring of the timer state.

The block is designed to offer an `FB_TON`-like behavior with an event-driven cyclic interface. It is suitable for applications where a timeout must be armed, cancelled, and periodically observed.

## Interface Structure

The function block has no direct event or data pins. All communication with the outside world is performed through adapter sockets.

| Adapter Socket | Adapter Type | Purpose |
|---|---|---|
| `TimeOutSocket` | `iec61499::events::ATimeOut` | Standard timeout control and timeout notification. |
| `TimeTickSocket` | `adapter::events::TimeOut::ATimeTick` | Cyclic query and status observation of the timer. |

### **Event Inputs**

There are no direct event inputs on the FB. The following incoming events are provided through the adapter sockets:

| Event | Adapter Socket | Description |
|---|---|---|
| `START` | `TimeOutSocket` | Starts the timeout evaluation. |
| `STOP` | `TimeOutSocket` | Stops or cancels the currently active timeout. |
| `REQ` | `TimeTickSocket` | Requests the current timer state; answered with `CNF`. |

### **Event Outputs**

There are no direct event outputs on the FB. The following outgoing events are provided through the adapter sockets:

| Event | Adapter Socket | Description |
|---|---|---|
| `TimeOut` | `TimeOutSocket` | Emitted when the configured timeout duration has elapsed. |
| `CNF` | `TimeTickSocket` | Confirmation for a `REQ` query. |
| `STARTO_IN` | `TimeTickSocket` | Notifies that the internal timer has started. |
| `STOPO_IN` | `TimeTickSocket` | Notifies that the internal timer has stopped. |

### **Data Inputs**

There are no direct data inputs on the FB. The following incoming data is provided through the adapter sockets:

| Data | Adapter Socket | Description |
|---|---|---|
| `DT` | `TimeOutSocket` | Timeout duration value forwarded to the internal timer. |

### **Data Outputs**

There are no direct data outputs on the FB. The following outgoing data is provided through the adapter sockets:

| Data | Adapter Socket | Description |
|---|---|---|
| `ET` | `TimeTickSocket` | Elapsed time of the internal timer. |
| `Q` | `TimeTickSocket` | Timer state output of the internal timer. |
| `PT` | `TimeTickSocket` | Configured or current timer time value. |

The data outputs are meaningful after an associated event such as `CNF`, `STARTO_IN`, `STOPO_IN`, or `TimeOut` has been issued.

### **Adapters**

The FB uses two adapter sockets as its complete external interface:

- `TimeOutSocket` provides the standard timeout operations:
  - `START` and `STOP` as incoming events.
  - `TimeOut` as outgoing event.
  - `DT` as incoming data.

- `TimeTickSocket` provides a cyclic observation interface:
  - `REQ` as incoming query event.
  - `CNF`, `STARTO_IN`, and `STOPO_IN` as outgoing notification events.
  - `ET`, `Q`, and `PT` as outgoing data values.

## Functionality

The behavior of E_FB_TimeOut is implemented by an internal instance of the event-based delay function block `adapter::events::TimeOut::E_FB_DELAY`.

The block works as follows:

1. When `TimeOutSocket.START` is received, the event is forwarded to the internal timer. The data value `TimeOutSocket.DT` is supplied as the delay/timeout duration.
2. The internal timer starts and counts the configured time.
3. If no `STOP` event arrives, the internal timer eventually generates an expiration event. This event is forwarded to `TimeOutSocket.TimeOut`.
4. When `TimeOutSocket.STOP` is received, it is forwarded to the internal timer and cancels the active timeout.
5. A cyclic query can be made through `TimeTickSocket.REQ`. The internal timer responds with `CNF` and provides the current values of `Q`, `ET`, and `PT` through `TimeTickSocket`.
6. Start and stop transitions of the internal timer are reported through `TimeTickSocket.STARTO_IN` and `TimeTickSocket.STOPO_IN`.

The internal wiring is summarized in the following table:

| E_FB_TimeOut Interface | Internal E_FB_DELAY Connection |
|---|---|
| `TimeOutSocket.START` | `DLY.START` |
| `TimeOutSocket.STOP` | `DLY.STOP` |
| `TimeOutSocket.DT` | `DLY.DT` |
| `DLY.EO` | `TimeOutSocket.TimeOut` |
| `TimeTickSocket.REQ` | `DLY.REQ` |
| `DLY.CNF` | `TimeTickSocket.CNF` |
| `DLY.STARTO` | `TimeTickSocket.STARTO_IN` |
| `DLY.STOPO` | `TimeTickSocket.STOPO_IN` |
| `DLY.ET` | `TimeTickSocket.ET` |
| `DLY.Q` | `TimeTickSocket.Q` |
| `DLY.PT` | `TimeTickSocket.PT` |

## Technical Features

- Composite function block implementation.
- Contains a single internal function block instance: `adapter::events::TimeOut::E_FB_DELAY`.
- No plain event or data pins; the entire interface is adapter-based.
- Supports both event-driven timeout signaling and cyclic state polling.
- Uses standard IEC 61499 event and data connections for internal wiring.
- Provides `Q`, `ET`, and `PT` values for monitoring and diagnosis.
- Can be embedded in larger IEC 61499 applications where reusable timeout services are required.

## State Overview

Although the block is a composite FB and does not define its own ECC state machine, the following observable states can be derived from the timer behavior:

| State | Description |
|---|---|
| Idle | No timeout is active. The timer has not been started or has been stopped. |
| Running | A `START` event has been accepted and the timer is counting. `STARTO_IN` is emitted when this state is entered. |
| Expired | The configured timeout duration has elapsed. `TimeOut` is emitted and the timer state output `Q` reflects the timeout condition. |
| Stopped | A `STOP` event has cancelled the active timeout. `STOPO_IN` is emitted when this state is entered. |

During all states, the current timer values can be queried through `TimeTickSocket.REQ` and `TimeTickSocket.CNF`.

## Application Scenarios

E_FB_TimeOut is suitable for the following use cases:

- Watchdog supervision of external operations.
- Timeout handling in communication protocols.
- Event-driven state machines requiring a start/stop timeout service.
- Cyclic control applications where the timeout state must be checked every PLC or IEC 61499 execution cycle.
- Reusable service blocks in distributed IEC 61499 systems.

The combination of `ATimeOut` and `ATimeTick` allows both a simple event-based usage and a more detailed cyclic monitoring of timing values.

## Comparison with Similar Blocks

| Block / Approach | Interface | Timeout Notification | Cyclic Query |
|---|---|---|---|
| `FB_TON` | Boolean input/output and data pins | `Q` changes, no timeout event | Usually not available |
| `E_DELAY` | Discrete event/data pins | Expiration event | Often not supported |
| `E_FB_TimeOut` | Adapter sockets `ATimeOut` and `ATimeTick` | `TimeOut` event | `REQ` / `CNF` with `Q`, `ET`, `PT` |

Compared with a simple TON function block, E_FB_TimeOut provides an event-oriented timeout service and additionally exposes a cyclic query interface through `TimeTickSocket`.

## Conclusion

E_FB_TimeOut is a compact, adapter-based timeout function block for IEC 61499 applications. It separates timeout control from cyclic status observation, which makes it flexible and reusable. Because it is a composite FB, its behavior is fully determined by the internal `E_FB_DELAY` instance and the adapter connections. This design provides a clean interface for event-driven timeout handling as well as periodic monitoring in automation and control systems.
