# AUDI_AUI_MUX_8_VAL


![AUDI_AUI_MUX_8_VAL_network](./AUDI_AUI_MUX_8_VAL_network.svg)

![AUDI_AUI_MUX_8_VAL](./AUDI_AUI_MUX_8_VAL.svg)

* * * * * * * * * *

## Introduction

AUDI_AUI_MUX_8_VAL is a composite subapplication that implements an 8‑channel multiplexer for values of type `UDINT`. It provides eight event inputs (`EI1` … `EI8`) and eight corresponding data inputs (`val1` … `val8`). When an event is triggered, the associated data value is routed through an internal adapter network and made available at the single AUDI adapter output. The subapplication combines an event multiplexer (`AUI_MUX_8`), an adapter selection block (`AUDI_AUI_MUX_8`), and eight initializer blocks (`initval_AUDI_1` … `initval_AUDI_8`) to achieve a clean, event‑driven selection of up to eight `UDINT` values.

## Interface Structure

The subapplication exposes a simple, purpose‑oriented interface: eight event inputs, eight data inputs, and one adapter output. There are no event outputs, data outputs, or additional adapters.

### **Event Inputs**

| Event Name | Description |
|------------|-------------|
| `EI1` | Event to select `val1` as the output value. |
| `EI2` | Event to select `val2` as the output value. |
| `EI3` | Event to select `val3` as the output value. |
| `EI4` | Event to select `val4` as the output value. |
| `EI5` | Event to select `val5` as the output value. |
| `EI6` | Event to select `val6` as the output value. |
| `EI7` | Event to select `val7` as the output value. |
| `EI8` | Event to select `val8` as the output value. |

### **Event Outputs**

None.

### **Data Inputs**

| Data Name | Type   | Description |
|-----------|--------|-------------|
| `val1`    | `UDINT` | Value to be output when `EI1` is triggered. |
| `val2`    | `UDINT` | Value to be output when `EI2` is triggered. |
| `val3`    | `UDINT` | Value to be output when `EI3` is triggered. |
| `val4`    | `UDINT` | Value to be output when `EI4` is triggered. |
| `val5`    | `UDINT` | Value to be output when `EI5` is triggered. |
| `val6`    | `UDINT` | Value to be output when `EI6` is triggered. |
| `val7`    | `UDINT` | Value to be output when `EI7` is triggered. |
| `val8`    | `UDINT` | Value to be output when `EI8` is triggered. |

### **Data Outputs**

None.

### **Adapters**

| Adapter Name | Type                                        | Description |
|--------------|---------------------------------------------|-------------|
| `OUT`        | `adapter::types::unidirectional::AUDI`      | Adapter output carrying the selected AUDI value. |

## Functionality

The subapplication operates purely in an event‑driven manner. Each of the eight event inputs is directly connected to the corresponding event input of the internal `AUI_MUX_8` block. That block uses the active event to produce a selector index `K` (an integer value) on its output. Simultaneously, each `val` input is passed to a dedicated `initval_AUDI` block, which converts the raw `UDINT` value into an AUDI‑typed adapter instance. The eight adapter outputs of the initializers are fed into the eight input channels of the `AUDI_AUI_MUX_8` block. Using the selector index `K` provided by `AUI_MUX_8`, the selection block forwards exactly one of the eight incoming AUDI adapters to its output, which is then directly connected to the subapplication’s `OUT` adapter.

In summary, triggering any event `EI<n>` causes the value `val<n>` to appear at the AUDI output, with no additional data processing. The internal structure ensures that the selection is synchronous with the triggering event and that only one value is active at a time.

## Technical Features

- **8‑Channel Selection**: Supports up to eight independent input values.
- **Pure Event‑Driven Operation**: No continuous polling; each selection is activated by an explicit event.
- **Adapter‑Based Output**: The result is provided as a typed AUDI adapter, allowing seamless integration with other AUDI‑compatible function blocks.
- **Internal Value Conversion**: The `initval_AUDI` blocks encapsulate the conversion from `UDINT` to the AUDI adapter type, keeping the interface simple.
- **Modular Composition**: Uses separate blocks for event multiplexing and adapter selection, which improves reusability and maintainability.
- **No Internal State**: The subapplication is stateless; output selection is fully determined by the most recent event.

## State Overview

The subapplication does not maintain any persistent internal state. Its behavior is purely combinational with respect to the triggering event:

- After a reset or at initial startup, no output is selected until one of the events `EI1`…`EI8` is raised.
- When an event `EI<n>` occurs, the corresponding `val<n>` is immediately routed to `OUT` and remains there until another event is triggered.
- The output value is only updated on event edges; data input changes without an associated event do not alter the output.

Because no explicit state variables exist, the subapplication is inherently safe and deterministic.

## Application Scenarios

This subapplication is particularly useful in control and automation systems where a single AUDI‑typed signal must be selected from a set of predefined values based on asynchronous events. Typical use cases include:

- **Mode Selection**: Switching between different operational parameter sets in a machine controller.
- **Recipe Management**: Selecting a recipe (as a `UDINT` code) and forwarding it to a downstream AUDI‑based processor.
- **Test Bench Configuration**: Choosing a test profile for a device under test.
- **Event‑Driven Calibration**: Changing calibration constants on the fly by sending the appropriate event.

Because the interface is minimal (only events and `UDINT` inputs), it can be easily integrated into larger IEC 61499 applications.

## Comparison with Similar Blocks

- **AUDI_AUI_MUX_3_VAL**: A three‑channel version of the same subapplication. The 8‑channel variant offers a larger selection count, making it suitable for applications with more options, but consumes more input events and data ports.
- **Standard MUX Function Blocks**: Typical multiplexers (e.g., based on a selector input) are not event‑driven and often require a permanent selector value. This subapplication, in contrast, uses events to trigger the selection, which simplifies integration in event‑oriented designs.
- **Simple Selector Adapters**: Some libraries provide direct adapter multiplexers without event synchronization. Here, the event input ensures that the output only changes at explicit trigger points, avoiding glitches or race conditions.

The key advantage of this subapplication is the combination of event‑based triggering, typed AUDI adapter output, and the internal separation of event and adapter logic.

## Conclusion

AUDI_AUI_MUX_8_VAL is a compact yet powerful subapplication that provides clean, event‑driven multiplexing of up to eight `UDINT` values into a single AUDI adapter output. Its modular internal structure and stateless behaviour make it both reliable and easy to integrate into complex IEC 61499 systems. With a simple interface and clear semantics, it is an excellent choice for any application requiring event‑synchronised signal selection in AUDI‑based environments.
