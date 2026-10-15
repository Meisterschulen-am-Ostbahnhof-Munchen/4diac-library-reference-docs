# FillWindowFS_AR


![FillWindowFS_AR_network](./FillWindowFS_AR_network.svg)

![FillWindowFS_AR](./FillWindowFS_AR.svg)

* * * * * * * * * *

## Introduction

The **FillWindowFS_AR** subapplication is a reusable IEC 61499 composite function block designed to colorize a FillAttributes object based on the position of a physical value relative to a configurable window. It accepts a real-valued input through an AR adapter, evaluates whether this value falls within the interval `[rWindowMin, rWindowMax]`, and then commands an internal `Q_FillAttributes` function block (window comparison by an internal `AR_DEADBAND_COMPARE`) to apply either green (inside the window) or red (outside the window) to the specified visualization object. This subapp encapsulates the complete evaluation and coloring logic into a single, network-embeddable component.

## Interface Structure

The subapplication exposes the initialization events `INIT`/`INITO`, the quality signals `QI`/`QO`, one confirmation event, three further data inputs, two further data outputs and one adapter socket.

### **Event Inputs**

| Name | Type  | Comment |
|------|-------|---------|
| INIT | EInit | Initialization request - passed through to the internal `Q_FillAttributes` (`Inner.INIT`). |

### **Event Outputs**

| Name | Type | Comment |
|------|------|---------|
| INITO | EInit | Initialization confirmation (from `Inner.INITO`). |
| CNF | Event | Confirmation of the requested service, passed through from the internal `Q_FillAttributes` block upon completion. |

### **Data Inputs**

| Name | Type | Initial Value | Comment |
|------|------|---------------|---------|
| QI | BOOL | TRUE | Quality Input - TRUE to enable the service (to `Inner.QI`). |
| u16ObjId | UINT | ID_NULL | FillAttributes object ID to command. Identifies the visualization object whose color shall be set. |
| rWindowMin | REAL | — | Lower bound of the window that shows green. Values equal to or greater than this bound are considered inside the window. |
| rWindowMax | REAL | — | Upper bound of the window that shows green. Values equal to or less than this bound are considered inside the window. |

### **Data Outputs**

| Name | Type | Comment |
|------|------|---------|
| QO | BOOL | Quality Output - TRUE if the service was executed successfully (from `Inner.QO`). |
| STATUS | STRING | Service status — passthrough from the internal `Q_FillAttributes` block, providing textual feedback about the last operation. |
| s16result | INT | Return value — passthrough from the internal `Q_FillAttributes` block, providing a numeric result code. |

### **Adapters**

| Name | Type | Direction | Comment |
|------|------|-----------|---------|
| rPhys | adapter::types::unidirectional::AR | Socket | Physical value whose position relative to the window determines the applied color. The incoming AR adapter carries a real number that is evaluated against the configured bounds. |

## Functionality

The subapplication implements a window evaluation combined with a two-color selection scheme. The functional flow proceeds as follows:

1. **Window Constants** — Two `initval_AR` blocks (`WindowMinConst` and `WindowMaxConst`) convert the externally provided `rWindowMin` and `rWindowMax` values into AR adapter values.

2. **Window Comparison** — The incoming AR adapter `rPhys` is connected as measured value `Y` to an `AR_DEADBAND_COMPARE` block (named `CENTER_CMP`), together with `WINDOW_MIN` (from `WindowMinConst`) and `WINDOW_MAX` (from `WindowMaxConst`). Its AX plug `CENTERED` is `TRUE` while `WINDOW_MIN ≤ Y ≤ WINDOW_MAX` holds (the plugs `ABOVE`/`BELOW` are not used here), i.e. the closed interval `[rWindowMin, rWindowMax]`.

3. **Color Selection** — `CENTERED` drives an `AX_SEL` block (named `ColorSel`) with `IN0` fixed to `COLOR_RED` and `IN1` fixed to `COLOR_GREEN`. When `CENTERED` is `TRUE`, green is selected; otherwise, red is selected. The chosen color is passed as the `OUT` value.

4. **Fill Command** — The color output from `ColorSel` is routed to the `u8FillColour` input of the internal `Q_FillAttributes` block (named `Inner`). Simultaneously, the external `u16ObjId` is connected to the block's object ID input. The block is parameterized with `u8FillType = USINT#2` (solid fill) and `u16FillPatternId = ID_NULL`.

5. **Event Handshaking** — The `ColorSel.CNF` event triggers the `Inner.REQ` event, initiating the fill operation. Upon completion, the internal block's `CNF` event is propagated to the subapplication's external `CNF` output.

6. **Result Propagation** — Both `STATUS` and `s16result` from the internal `Q_FillAttributes` block are forwarded directly to the subapplication's corresponding output ports, providing full transparency about the operation's outcome.

7. **Initialization and Quality Signals** — These are passed straight through: `INIT` → `Inner.INIT`, `Inner.INITO` → `INITO`, `QI` → `Inner.QI` and `Inner.QO` → `QO`.

## Technical Features

- **Composite SubApplication Type** — Implemented as a `SubAppType` rather than a custom FBType, allowing easy reuse and modification through standard 4diac-IDE tooling.
- **Adapter-Based Input** — The physical value is acquired via a unidirectional AR adapter, decoupling the subapp from specific sensor or data source implementations and promoting type-safe connections.
- **Constant Initialization Pattern** — Utilizes `initval_AR` blocks to convert runtime REAL variables into adapter-based constant values, leveraging the adapter framework's type system.
- **Window Comparison** — Closed interval evaluation by `AR_DEADBAND_COMPARE` (`CENTERED` output), guaranteeing that boundary values are treated as inside the window. Since version 2.0 this replaces the former own split/GE/LE/AND wiring.
- **Color Mapping** — Pre-configured `AX_SEL` parameters with `COLOR_RED` and `COLOR_GREEN` constants, drawing from the `isobus::UT::Q::const::colours` namespace.
- **Event Propagation Chain** — A clean event cascade from selection confirmation to fill request, then back to the caller via the `CNF` output. Apart from `INIT` there are no event inputs; the coloring is triggered by new data on the adapter `rPhys` (or on the window bounds).
- **Status Passthrough** — Both status string and result integer are exposed at the subapp boundary, enabling callers to monitor and log operations without inspecting internal state.
- **Quality Pattern** — `QI` (default `TRUE`) enables the service, `QO` reports the success of the internal `Q_FillAttributes`.
- **Namespace Imports** — Explicit imports of `ID_NULL` and the color constants ensure compile-time correctness in the target runtime environment.

## State Overview

`FillWindowFS_AR` does not maintain an explicit internal state machine of its own (`AR_DEADBAND_COMPARE` only reports changes of its outputs, and `INIT`/`INITO` are merely passed through). The observable behavior per service cycle can be summarized as:

| Condition | Evaluation Result | Selected Color | Fill Command |
|-----------|-------------------|----------------|--------------|
| `rPhys < rWindowMin` | Outside (below) | RED | Solid red fill |
| `rWindowMin ≤ rPhys ≤ rWindowMax` | Inside window | GREEN | Solid green fill |
| `rPhys > rWindowMax` | Outside (above) | RED | Solid red fill |

The behavior is fully deterministic for a given set of inputs.

## Application Scenarios

- **Visualization of Process Variables** — In a process control HMI, the subapp can color-code a numeric indicator (e.g., temperature, pressure, level) green when it lies within an acceptable operating range and red when it deviates, providing immediate visual feedback to operators.
- **Alarm and Limit Monitoring** — When integrated with alarm handling logic, the subapp can be used to repaint warning icons or trend markers based on whether a measured value violates configured thresholds.
- **Quality Control and Sorting** — In manufacturing or inspection systems, the block can visually flag measured dimensions or test results that fall outside tolerance windows.
- **Interactive Panels and Dashboards** — The subapp can drive color changes on dashboard widgets where a physical quantity's range (e.g., speed, fuel level, temperature) determines the displayed color.
- **Educational Demonstrations** — As a clearly structured composite, it serves as a learning example for adapter-based data flow, window comparisons, and event-driven service invocation in IEC 61499 systems.

## Comparison with Similar Blocks

| Feature | FillWindowFS_AR | Custom FBType with ST Helper | GreenRedBackground1_AX |
|---------|----------------|------------------------------|------------------------|
| Implementation | SubAppType composite | Custom compiled FBType | Similar SubApp approach |
| Input Paradigm | AR adapter for physical value | Direct data inputs | AR adapter |
| Window Evaluation | Internal `AR_DEADBAND_COMPARE` (`CENTERED`) | Inline ST condition | Configurable via parameters |
| Color Constants | Fixed RED/GREEN | Depends on implementation | Fixed RED/GREEN |
| Event Output | Single CNF | Depends on design | Single CNF |
| Reconfigurability | High (network editable) | Medium (code changes) | High |
| Runtime Dependencies | Only standard adapter FBs | Custom compiled code | Standard adapter FBs |

Compared to a hand-coded FBType with ST logic, this subapp leverages standard building blocks, making it more transparent, portable, and inspectable at the network level. Relative to similar background-coloring subapps, it adds the window-based comparison and the physical-value adapter input in an integrated form.

## Conclusion

The **FillWindowFS_AR** subapplication provides a compact, well-structured, and reusable solution for range-based color visualization of physical values on FillAttributes objects. By combining the standard window comparison block `AR_DEADBAND_COMPARE`, a selection block, and a `Q_FillAttributes` service block, it implements a complete green-inside / red-outside logic with minimal external wiring. The clear interface, event passthrough (`INIT`/`INITO`, `CNF`), quality signals (`QI`/`QO`) and status propagation make it straightforward to integrate into larger IEC 61499 applications, while the adapter-based input ensures compatibility with a wide range of sensor and data sources. It is a practical building block for HMI development, alarm visualization, and quality-related color-coding tasks in industrial automation environments.

