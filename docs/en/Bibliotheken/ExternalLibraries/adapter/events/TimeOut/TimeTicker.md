# TimeTicker

![TimeTicker](./TimeTicker.svg)

* * * * * * * * * *
## Introduction

`TimeTicker` is a composite function block that encapsulates an `iec61499::events::E_CYCLE` function block and exposes an adapter-based interface for periodic tick generation. It provides a configurable cycle time, start/stop handling, and event-based reporting of output values such as process time and elapsed time.

The block is intended to be used as a reusable time-tick service inside IEC 61499 systems where adapter-based communication is preferred over direct event/data wiring.

## Interface Structure

### **Event Inputs**

| Event   | Type   | With | Description                       |
|---------|--------|------|-----------------------------------|
| `INIT`  | `EInit`| `TC` | Initialization request, carries the cycle time. |

### **Event Outputs**

| Event   | Type    | With      | Description                                         |
|---------|---------|-----------|-----------------------------------------------------|
| `INITO` | `EInit` | -         | Initialization confirmation.                         |
| `CNF`   | `Event` | `Q`, `ET` | Reports the current `Q` and `ET` on every tick.      |
| `STARTO`| `Event` | `PT`      | Indicates that the ticker has been started.          |
| `STOPO` | `Event` | -         | Indicates that the ticker has been stopped.          |

### **Data Inputs**

| Data | Type | Initial Value | Description           |
|------|------|---------------|-----------------------|
| `TC` | `TIME` | `T#200ms` | Cycle time of the ticker. |

### **Data Outputs**

| Data | Type   | Description                                        |
|------|--------|----------------------------------------------------|
| `Q`  | `BOOL` | Current output state supplied through the adapter. |
| `PT` | `TIME` | Process time, associated with the start event.     |
| `ET` | `TIME` | Elapsed time, reported with each confirmation.     |

### **Adapters**

| Adapter         | Type                                  | Direction | Description                                        |
|-----------------|---------------------------------------|-----------|----------------------------------------------------|
| `TimeTickSocket`| `adapter::events::TimeOut::ATimeTick` | Plug      | Adapter connection used for tick requests, confirmations, start/stop commands, and time data exchange. |

Internally, the adapter is used with the following logical endpoints:

| Adapter Endpoint  | Role in the Internal Network                        |
|-------------------|-----------------------------------------------------|
| `REQ`             | Receives the periodic event from `E_CYCLE.EO`.      |
| `CNF`             | Triggers the external `CNF` event output.            |
| `STARTO_IN`       | Starts the internal `E_CYCLE` and triggers `STARTO`. |
| `STOPO_IN`        | Stops the internal `E_CYCLE` and triggers `STOPO`.   |
| `Q`, `PT`, `ET`   | Provide the data values connected to the outputs.    |

## Functionality

The `TimeTicker` composite FB contains a single internal `E_CYCLE` block. The cycle time `TC` is connected to the `DT` input of `E_CYCLE`, so the period of the generated ticks is determined by `TC`.

When a start command arrives through the adapter (`STARTO_IN`), the composite FB:

- raises the `STARTO` event output together with `PT`,
- sends a start event to the internal `E_CYCLE`.

While the internal cycle block is running, it generates `EO` events periodically with the configured interval. Each `EO` event is forwarded to the adapter as `REQ`, producing a tick on the adapter interface.

When the adapter returns a confirmation `CNF`, the composite FB raises its own `CNF` event output and publishes the associated data values `Q` and `ET`.

A stop command arriving through the adapter (`STOPO_IN`) is forwarded to the stop input of the internal `E_CYCLE` and also triggers the `STOPO` output.

An `INIT` event is passed directly through to `INITO`, providing an immediate initialization confirmation.

## Technical Features

- Composite FB implementation using an internal `iec61499::events::E_CYCLE`.
- Configurable cycle time through the `TC` data input, defaulting to `200 ms`.
- Adapter-based tick interface using `adapter::events::TimeOut::ATimeTick`.
- Standard event/data association:
  - `INIT` is associated with `TC`.
  - `CNF` is associated with `Q` and `ET`.
  - `STARTO` is associated with `PT`.
- No internal algorithms or state-machines are required; the behavior is entirely defined by the function block network.

## State Overview

The `TimeTicker` does not contain an explicit ECC state machine, but it can be described in terms of logical operational states:

| State     | Description                                                                 |
|-----------|-----------------------------------------------------------------------------|
| Initialized | After `INIT`, the FB confirms initialization via `INITO`. No ticks are generated yet. |
| Stopped   | The internal `E_CYCLE` is not running. No periodic `REQ` events are generated. |
| Running   | After `STARTO_IN`, the internal `E_CYCLE` is started. Ticks are emitted periodically on the adapter. |
| Confirming | After each tick, the adapter returns `CNF`, causing the `CNF` output to fire with `Q` and `ET`. |

## Application Scenarios

- Periodic data polling in automation systems.
- Generation of cyclic time-tick events for adapter-based communication.
- Start/stop controlled time supervision with process time and elapsed time reporting.
- Integration into larger IEC 61499 applications that rely on adapters instead of direct wiring.
- Reusable timing component for service interfaces that require a periodic request/confirmation pattern.

## Comparison with Similar Blocks

| Block        | Comparison                                                                 |
|--------------|-----------------------------------------------------------------------------|
| `E_CYCLE`    | A low-level periodic event generator. `TimeTicker` wraps `E_CYCLE` and adds an adapter interface, start/stop events, and confirmation data. |
| `TON` / `TOF`| Classic timer blocks generate boolean timing outputs. `TimeTicker` generates periodic events and reports time values through an adapter. |
| Plain service FB | Many service FBs require manual event handling. `TimeTicker` encapsulates cycle control and adapter communication in one composite block. |

## Conclusion

The `TimeTicker` FB is a practical composite function block for producing periodic tick events in an adapter-based IEC 61499 environment. By combining an internal `E_CYCLE` with an `ATimeTick` adapter, it provides a clean and reusable interface for start/stop control, cyclic requests, confirmations, and time-related data exchange. Its configurable cycle time and straightforward internal network make it suitable for many event-driven automation and monitoring scenarios.