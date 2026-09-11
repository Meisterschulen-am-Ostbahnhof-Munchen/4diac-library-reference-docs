# PositionMarkerFSA

![PositionMarkerFSA](./PositionMarkerFSA.svg)

* * * * * * * * * *

## Introduction

PositionMarkerFSA is an adapter wrapper around the `PositionMarkerFS` function block. It accepts a physical REAL value (Sollwert) via an AR (incoming) adapter socket and forwards it as the setpoint to an internally instantiated `PositionMarkerFS`. This design follows the same pattern as `Q_NumericValue_PHYSA` around `Q_NumericValue_PHYS`, re‑exposing the over‑ and under‑limit indications through AX (outgoing) adapter plugs instead of distinct event/data ports.

The wrapper handles all communication between the adapter-based interface and the internal FB, relieving the developer from manually connecting the `REQ`/`rValue` pair for each invocation.

## Interface Structure

### **Event Inputs**

| Event | Type | Comment |
|-------|------|---------|
| `INIT` | `EInit` | Service initialization; snapshots `stObj` and `xScale` into the internal `PositionMarkerFS` instance. |

### **Event Outputs**

| Event | Type | Comment |
|-------|------|---------|
| `INITO` | `EInit` | Initialization confirm – emitted after the internal FB has been successfully initialized. |
| `CNF` | `Event` | Confirmation of a requested service – raised whenever the internal FB confirms a position-rating request. Carries `STATUS` and `s16result` as data. |

### **Data Inputs**

| Data | Type | Comment |
|------|------|---------|
| `stObj` | `isobus::utils::childposition::PositionMarker_S` | Marker object pool properties: child/parent IDs, travel bounds, and center offset. This structure is captured at `INIT` and passed unchanged to the internal FB. |
| `xScale` | `BOOL` | Scaling control: `FALSE` (default) bypasses the DM/SKM scaling factor; `TRUE` enables it. This flag is forwarded to the internal `PositionMarkerFS`. |

### **Data Outputs**

| Data | Type | Comment |
|------|------|---------|
| `STATUS` | `STRING` | Service status – a direct passthrough from the internal `PositionMarkerFS`. |
| `s16result` | `INT` | Return value – a direct passthrough from the internal `PositionMarkerFS`. |

### **Adapters**

| Adapter | Direction | Type | Comment |
|---------|-----------|------|---------|
| `rPhys` | Socket (Input) | `adapter::types::unidirectional::AR` | Physical REAL value input – the Sollwert that drives the internal FB. |
| `xOver` | Plug (Output) | `adapter::types::unidirectional::AX` | Signals when the actual position (after adding `stObj.r32Center`) exceeds `stObj.r32MaxPos` and is clamped. |
| `xUnder` | Plug (Output) | `adapter::types::unidirectional::AX` | Signals when the actual position falls below `stObj.r32MinPos` and is clamped. |

## Functionality

PositionMarkerFSA encapsulates the complete data and event flow of a position marker calculation. On `INIT`, it passes the `stObj` and `xScale` values to the internal `PositionMarkerFS` instance, which initializes its internal state. Later, each incoming event from the `rPhys` adapter (via its internal `E1` output) triggers the `REQ` event of the internal FB, simultaneously transferring the physical value (`rPhys.D1`) as the new setpoint (`rValue`). The internal FB performs the marker position analysis and responds with a `CNF` event.

The wrapper forwards the `INITO` and `CNF` events to its own outputs, and also re‑emits the `CNF` event to both adapter plugs `xOver` and `xUnder` so that limit violations can be propagated asynchronously. The data outputs `STATUS` and `s16result` are copied directly from the internal FB.

## Technical Features

- **Adapter‑based I/O**: The input (physical value) and outputs (limit flags) are provided using unidirectional AR/AX adapter types, making the block easy to connect in CAN‑based environments where such adapter patterns are common.
- **Single internal FB instance**: The block instantiates exactly one `PositionMarkerFS`, keeping the implementation lean and preserving all its original semantics.
- **Passthrough of status and result**: `STATUS` and `s16result` are forwarded without modification, allowing the host system to inspect detailed results.
- **Scaling control**: The `xScale` flag is passed straight through to the internal FB, giving developers the option to enable or disable scaling without altering the adapter interface.
- **Automatic event wiring**: The adapter’s event output (`E1`) is directly connected to the internal `REQ` event; the internal `CNF` event is distributed to both the main `CNF` output and the two adapter plugs.

## State Overview

PositionMarkerFSA does not introduce its own state machine. Instead, it delegates all state handling to the internal `PositionMarkerFS`. Therefore, the state behaviour matches that of the underlying block:

- **Initialization phase**: After `INIT`, the internal FB is configured with the marker object pool and scaling flag. It then emits `INITO`.
- **Request/confirmation phase**: Each request via `rPhys` (or the corresponding adapter event) leads to a calculation cycle. On completion, the internal FB emits `CNF` and updates `STATUS` and `s16result`. Additionally, if the calculated position exceeds or falls below the given limits, the corresponding adapter plug (`xOver`/`xUnder`) is activated.

## Application Scenarios

PositionMarkerFSA is best suited for systems that use adapter‑based communication, particularly in agricultural or mobile machinery applications following the ISO 11783 (ISOBUS) standard. Common use cases include:

- Integrating a position marker into a CAN network where the setpoint is transmitted as a physical REAL value and the limit flags are expected as separate adapter‑based outputs.
- Replacing older designs that used plain `REQ`/`rValue` input pairs with a more structured adapter interface, without changing the core calculation logic.
- Using the `xScale` flag to conditionally apply scaling factors, making the block adaptable to different machine configurations.

## Comparison with Similar Blocks

- **PositionMarkerFS** – This is the underlying FB, which provides a plain `REQ`/`rValue` interface and separate `xOver`/`xUnder` data outputs. PositionMarkerFSA wraps it to present an adapter‑based interface, simplifying integration in adapter‑friendly networks.
- **Q_NumericValue_PHYSA** – A similar wrapper pattern that wraps `Q_NumericValue_PHYS`; PositionMarkerFSA follows the same design philosophy but is specifically tailored for the position marker logic.
- **PositionMarker_S** – This is a data structure used as input, not an FB itself. In comparison, PositionMarkerFSA is a complete function block that consumes `PositionMarker_S` and produces status and limit indications.

## Conclusion

PositionMarkerFSA provides a clean, adapter‑oriented interface for driving a position marker calculation. By wrapping the established `PositionMarkerFS` FB, it preserves all functional behaviour while improving integration flexibility in adapter‑based systems. The clear separation of the input (physical value), outputs (status, result, and limit flags), and the optional scaling control makes it a valuable building block for ISOBUS‑compliant implementations.
