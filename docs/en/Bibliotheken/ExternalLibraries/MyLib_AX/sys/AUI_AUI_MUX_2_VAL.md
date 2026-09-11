# AUI_AUI_MUX_2_VAL


![AUI_AUI_MUX_2_VAL_network](./AUI_AUI_MUX_2_VAL_network.svg)

![AUI_AUI_MUX_2_VAL](./AUI_AUI_MUX_2_VAL.svg)

* * * * * * * * * *
## Introduction

The **AUI_AUI_MUX_2_VAL** subapplication implements a 2-way multiplexer for AUI adapter values. It accepts two `UINT` input values and two event inputs. Depending on which event is triggered, one of the two input values is converted into an AUI value and provided at the `OUT` adapter plug.

The internal structure combines:
- two `initval_AUI` instances to convert `UINT` values into AUI adapter outputs,
- an `AUI_MUX_2` block for event handling,
- an `AUI_AUI_MUX_2` block for selecting between two AUI input values.

This makes the subapplication a convenient, encapsulated solution for selecting between two AUI values based on events.

* * * * * * * * * *
## Interface Structure

### **Event Inputs**

| Name | Type | Comment |
|------|------|---------|
| `EI1` | `Event` | Event to select `val1` |
| `EI2` | `Event` | Event to select `val2` |

### **Event Outputs**

None.

### **Data Inputs**

| Name | Type | Comment |
|------|------|---------|
| `val1` | `UINT` | Initial output value when `EI1` is triggered |
| `val2` | `UINT` | Initial output value when `EI2` is triggered |

### **Data Outputs**

None.

### **Adapters**

| Name | Type | Comment |
|------|------|---------|
| `OUT` | `adapter::types::unidirectional::AUI` | Selected AUI adapter output |

* * * * * * * * * *
## Functionality

The subapplication acts as a 2-input multiplexer for AUI adapter values:

1. The `UINT` values `val1` and `val2` are connected to two internal `initval_AUI` blocks. These blocks convert the `UINT` values into internal AUI adapter signals.
2. The event inputs `EI1` and `EI2` are connected to the internal `AUI_MUX_2` block. This block determines which input should be selected.
3. The selection information is forwarded to `AUI_AUI_MUX_2`, which receives the two prepared AUI values on its `IN1` and `IN2` adapter inputs.
4. The selected AUI value is routed to the external `OUT` adapter plug.

In practice:
- Triggering `EI1` routes `val1` to `OUT`.
- Triggering `EI2` routes `val2` to `OUT`.

* * * * * * * * * *
## Technical Features

- Implements a 2-way multiplexer for AUI adapter values.
- Accepts simple `UINT` values instead of requiring the caller to provide AUI adapter inputs.
- Uses internal `initval_AUI` blocks for `UINT`-to-AUI conversion and initialization.
- Uses `AUI_MUX_2` for event-controlled selection.
- Uses `AUI_AUI_MUX_2` for final adapter output selection.
- Provides a single unidirectional AUI adapter output.
- Encapsulates the complete multiplexing logic inside a reusable subapplication.

* * * * * * * * * *
## State Overview

The subapplication does not expose an explicit state machine, but its behavior can be described as a two-state selector:

| Event | Selected Input | Output |
|-------|----------------|--------|
| `EI1` | `val1` | AUI representation of `val1` via `OUT` |
| `EI2` | `val2` | AUI representation of `val2` via `OUT` |
| None | Last selection remains active | Depends on the last received event |

After one event has been processed, the selected AUI value remains available at `OUT` until the other event is triggered.

* * * * * * * * * *
## Application Scenarios

The `AUI_AUI_MUX_2_VAL` subapplication is useful in IEC 61499 applications where:

- Two predefined `UINT` values must be selected dynamically by events.
- AUI adapter connections are used for data exchange between function blocks.
- A compact and reusable multiplexer is needed for AUI-based signals.
- Two alternative configuration values must be switched, such as:
  - manual and automatic setpoints,
  - normal and backup values,
  - two operating modes with different parameters.

* * * * * * * * * *
## Comparison with Similar Blocks

| Block / Subapplication | Data Type | Selection Mechanism | Additional Features |
|------------------------|-----------|---------------------|---------------------|
| Standard `MUX` | Basic data types | Boolean or event selection | No AUI adapter support |
| `AUI_MUX_2` | AUI | Event inputs | Handles AUI event selection |
| `AUI_AUI_MUX_2` | AUI | Internal AUI selection | Requires already available AUI inputs |
| `AUI_AUI_MUX_2_VAL` | `UINT` inputs, AUI output | `EI1` / `EI2` events | Converts `UINT` values to AUI internally |

Compared to a direct AUI multiplexer, this subapplication adds the convenience of using plain `UINT` values while keeping the AUI adapter interface at the output.

* * * * * * * * * *
## Conclusion

`AUI_AUI_MUX_2_VAL` is a compact and reusable subapplication for event-driven selection between two AUI values. It hides the internal AUI initialization and selection logic behind a simple interface consisting of two events, two `UINT` inputs, and one AUI adapter output. It is particularly suitable for applications that require fast switching between two predefined values within an AUI-based IEC 61499 environment.