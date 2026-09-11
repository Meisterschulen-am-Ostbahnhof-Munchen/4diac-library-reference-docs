# PositionMarkerFS

![PositionMarkerFS](./PositionMarkerFS.svg)

* * * * * * * * * *

## Introduction

**PositionMarkerFS** is a function block designed to move a Virtual Terminal (VT) marker object so that it reflects an arbitrary incoming REAL value. It wraps a single **Q_ChildPosition** instance (ISO 11783-6, Annex F.16) and provides a clean, high-level interface for positioning a marker (e.g., a triangle or other graphical element) within its parent container on an ISOBUS VT display.

The block accepts a structured configuration object (`PositionMarker_S`) containing the marker's object pool identifiers, travel bounds, and center offset. It snapshots this structure once at initialization, then on each request:

1. Offsets the incoming value by the configured center offset.
2. Clamps the result to the marker's minimum/maximum travel range.
3. Converts the clamped value to an integer and forwards it as the marker's X position.
4. Passes through the Y position unchanged.

Clamping events are reported via dedicated status outputs, and all results from the underlying `Q_ChildPosition` service are propagated to the caller.

## Interface Structure

### **Event Inputs**

| Event | Type | With Variables | Description |
|-------|------|----------------|-------------|
| `INIT` | EInit | `stObj`, `xScale` | Service initialization. Snapshots the marker configuration and initializes the internal `Q_ChildPosition` instance. |
| `REQ` | Event | `rValue` | Requests the marker to move to reflect the newly supplied value. |

### **Event Outputs**

| Event | Type | With Variables | Description |
|-------|------|----------------|-------------|
| `INITO` | EInit | — | Initialization confirm. Emitted after `INIT` has been successfully processed. |
| `CNF` | Event | `STATUS`, `s16result`, `xOver`, `xUnder` | Confirmation of a requested move, carrying the service status, return value, and clamp indicators. |

### **Data Inputs**

| Name | Type | Initial Value | Description |
|------|------|---------------|-------------|
| `stObj` | `isobus::utils::childposition::PositionMarker_S` | — | Marker object pool properties: child ID, parent ID, travel bounds (`r32MinPos`, `r32MaxPos`), center offset (`r32Center`), and fixed Y position (`s16YPosition`). Snapshot once at `INIT`. |
| `xScale` | `BOOL` | `FALSE` | When `FALSE` (default), X and Y positions are passed through unchanged. When `TRUE`, they are scaled by the DM/SKM factor for the parent object. Passed straight through to the internal `Q_ChildPosition`. |
| `rValue` | `REAL` | — | The physical value to display. Can originate from any source — a VT input number, a sensor reading, or any other logic. |

### **Data Outputs**

| Name | Type | Description |
|------|------|-------------|
| `STATUS` | `STRING` | Service status, passed through directly from the internal `Q_ChildPosition` instance. |
| `s16result` | `INT` | Return value from the internal `Q_ChildPosition`, indicating the outcome of the child-position command. |
| `xOver` | `BOOL` | Set when the requested value (after adding `r32Center`) exceeded `r32MaxPos` and was clamped. |
| `xUnder` | `BOOL` | Set when the requested value (after adding `r32Center`) fell below `r32MinPos` and was clamped. |

### **Adapters**

This function block does not expose any adapters.

## Functionality

The block operates in two distinct phases:

**Initialization** — When `INIT` is triggered, the input structure `stObj` is copied into an internal snapshot using an `F_MOVE` function block. This snapshot is then used to configure the embedded `Q_ChildPosition` service with the correct child ID, parent ID, and scale flag. The `INITO` event confirms successful initialization.

**Value Processing** — On each `REQ` event, the following pipeline is executed:

1. **Offset**: The incoming `rValue` is added to the snapshot's `r32Center` value using `F_ADD`. This accounts for any centering offset of the marker within its travel range.
2. **Clamp**: The offset result is passed through `F_ClampReal`, which restricts the value to the interval `[r32MinPos, r32MaxPos]`. If clamping occurs, the `xOver` or `xUnder` flags are set accordingly.
3. **Conversion**: The clamped REAL value is converted to an INT using `F_REAL_TO_INT` (truncation).
4. **Positioning**: The converted value is written to `s16Xposition` of the internal `Q_ChildPosition` service, along with the snapshot's `s16YPosition` (passed through unchanged). The service is then requested to move the marker.

The `CNF` event propagates the `STATUS`, `s16result`, `xOver` and `xUnder` outputs from the internal service and the clamping stage, giving the caller full visibility into the operation's outcome.

## Technical Features

- **ISO 11783-6 compliant** — the wrapped `Q_ChildPosition` service follows the ISOBUS VT child position semantics (Annex F.16).
- **Snapshot-based configuration** — the `PositionMarker_S` structure is captured once at `INIT`, preventing later modifications from corrupting the working state.
- **Integer conversion** — the REAL input is converted to an INT only once, at the point where it is needed by the VT service, minimizing unnecessary conversions.
- **Clamp reporting** — over-range and under-range conditions are explicitly reported, allowing the caller to react to out-of-bounds input values.
- **Scale pass-through** — the `xScale` flag is forwarded verbatim to the internal `Q_ChildPosition`, giving direct control over DM/SKM scaling behavior.
- **Fully event-driven** — both initialization and operation follow the strict `REQ`/`CNF` and `INIT`/`INITO` handshake patterns common to 4diac service interface function blocks.

## State Overview

The block does not maintain an explicit state machine but relies on the internal `Q_ChildPosition` service for its lifecycle. The externally observable states are:

- **Uninitialized** — before `INIT` has been processed, or after an unsuccessful initialization. A `REQ` issued in this state will produce undefined behavior and should be avoided.
- **Initialized / Idle** — after `INITO` is emitted, the block is ready to accept `REQ` events. It remains idle while no request is being processed.
- **Processing** — between receiving a `REQ` and emitting the corresponding `CNF`. During this window, the internal pipeline (add → clamp → convert → move) is active.
- **Clamped** — a sub-state during processing where the value was out of range; indicated by `xOver` or `xUnder` being `TRUE` on the `CNF` output.

## Application Scenarios

- **ISOBUS VT input displays** — Display a numeric value (e.g., from a `NumericValue_PHYS` object) as a moving marker on a VT screen.
- **Sensor value visualization** — Map a continuous sensor reading (temperature, pressure, position) to the position of a graphical indicator on a machine display.
- **Process control dashboards** — Drive a marker inside a tank, gauge, or scale graphic, where the marker's horizontal position encodes a real-world measurement.
- **Machine automation** — Combine with any REAL-valued process variable to provide immediate visual feedback on a VT, including automatic clamping when limits are exceeded.

## Comparison with Similar Blocks

| Feature | `PositionMarkerFS` | Typical raw `Q_ChildPosition` usage |
|---------|-------------------|--------------------------------------|
| Input type | Accepts a physical `REAL` value | Requires an already-formatted INT position |
| Center offset handling | Built-in via `r32Center` | Must be handled by the caller |
| Clamp protection | Automatic, with status flags | Not provided; out-of-range values would corrupt the display |
| Configuration | Snapshot-based, single `stObj` struct | Requires manual set-up of multiple parameters |
| Y-position handling | Passed through automatically | Must be managed separately |
| Scale handling | Optional pass-through `xScale` | Caller must know when to scale |

Compared to using `Q_ChildPosition` directly, `PositionMarkerFS` abstracts away the type conversion, clamping, and center offset, resulting in fewer errors at the call site and a clearer data flow. It is more specialized than a generic clamp/scale block but significantly more convenient for VT marker positioning tasks.

## Conclusion

**PositionMarkerFS** bridges the gap between arbitrary REAL-valued process data and precise, clamp-safe VT marker positioning in the ISOBUS environment. By encapsulating the offset, clamping, conversion, and child-position service calls into a single, well-defined interface, it simplifies application code and reduces the risk of marker positioning errors. The block is particularly suited for dashboards, gauges, and any scenario where a physical value must be rendered visually on a Virtual Terminal with predictable behavior at range limits.
