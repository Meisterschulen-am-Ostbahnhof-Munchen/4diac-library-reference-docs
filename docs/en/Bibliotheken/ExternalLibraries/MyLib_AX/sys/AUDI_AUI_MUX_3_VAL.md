# AUDI_AUI_MUX_3_VAL


![AUDI_AUI_MUX_3_VAL_network](./AUDI_AUI_MUX_3_VAL_network.svg)

![AUDI_AUI_MUX_3_VAL](./AUDI_AUI_MUX_3_VAL.svg)

* * * * * * * * * *

## Introduction

The **AUDI_AUI_MUX_3_VAL** is a subapplication that implements a 3-way multiplexer for AUDI adapter values. It combines three internal `initval_AUDI` function blocks, an `AUI_MUX_3` event selection block, and an `AUDI_AUI_MUX_3` adapter multiplexer. Depending on which event input (`EI1`, `EI2`, or `EI3`) is triggered, one of the three UDINT input values (`val1`, `val2`, `val3`) is selected and made available as an AUDI adapter output. The subapp enables direct wiring of raw numeric values into the AUDI adapter domain without requiring pre-initialized adapter instances externally.

## Interface Structure

### **Event Inputs**

| Event | Description |
|-------|-------------|
| `EI1` | Event to select `val1` as the active output |
| `EI2` | Event to select `val2` as the active output |
| `EI3` | Event to select `val3` as the active output |

### **Event Outputs**

No event outputs are provided by this subapplication.

### **Data Inputs**

| Data | Type | Description |
|------|------|-------------|
| `val1` | UDINT | Initial output value when `EI1` is triggered |
| `val2` | UDINT | Initial output value when `EI2` is triggered |
| `val3` | UDINT | Initial output value when `EI3` is triggered |

### **Data Outputs**

No data outputs are provided by this subapplication.

### **Adapters**

| Adapter | Type | Description |
|---------|------|-------------|
| `OUT` | `adapter::types::unidirectional::AUDI` | Selected AUDI adapter output carrying the multiplexed value |

## Functionality

The subapplication operates as a complete selection chain for AUDI values:

1. The three raw UDINT values `val1`, `val2`, and `val3` are passed to three internal `initval_AUDI` function blocks (`initval_AUDI_1`, `initval_AUDI_2`, `initval_AUDI_3`). Each block converts the corresponding UDINT value into an AUDI adapter initial-value stream.
2. The internal `AUI_MUX_3` block receives the event inputs `EI1`, `EI2`, and `EI3` and performs event-based selection logic.
3. The selection information is forwarded via the adapter connection `AUI_MUX_3.K` to the internal `AUDI_AUI_MUX_3` block.
4. The `AUDI_AUI_MUX_3` block multiplexes the three AUDI adapter inputs (`IN1`, `IN2`, `IN3`) originating from the `initval_AUDI` blocks and routes the chosen adapter stream to the output adapter `OUT`.

The entire chain is event-driven: only when an event is raised on `EI1`, `EI2`, or `EI3` does the output adapter reflect the corresponding input value.

## Technical Features

- **Three-way selection** via dedicated event inputs (`EI1`, `EI2`, `EI3`), providing explicit and deterministic switching behavior.
- **UDINT input type** for `val1`, `val2`, and `val3`, allowing direct connection of unsigned double integer values from the application logic.
- **Adapter-based output** using the unidirectional `AUDI` adapter type, enabling seamless integration with other AUDI-compatible blocks.
- **Internal composition** combines `initval_AUDI` (value conversion), `AUI_MUX_3` (event selection), and `AUDI_AUI_MUX_3` (adapter multiplexing) into a single reusable subapplication.
- **No additional event or data outputs**, keeping the interface clean and focused on the multiplexing purpose.

## State Overview

The subapplication does not maintain an explicit internal state machine. Selection behavior is purely event-driven: each trigger on `EI1`, `EI2`, or `EI3` immediately determines which of the three values is propagated to the output. Consequently, the active output corresponds to the most recently triggered event input, and no intermediate states are exposed to the user.

## Application Scenarios

Typical use cases for the `AUDI_AUI_MUX_3_VAL` include:

- **Mode or channel selection** in automation processes where one of three preconfigured AUDI values must be chosen based on external events (e.g., operating mode switches, recipe selection).
- **Parameter switching** in control systems, enabling dynamic changes between different setpoints or configuration values that are supplied as raw UDINT values.
- **Redundant value handling** in safety-critical applications, where a fallback or alternative value can be selected via specific event triggers.
- **Integration into larger adapter-based networks**, providing a convenient bridge between raw numeric inputs and AUDI adapter streams without manual adapter initialization.

## Comparison with Similar Blocks

Compared to the standard `AUDI_AUI_MUX_3` block, the `AUDI_AUI_MUX_3_VAL` introduces an integrated initialization layer through the `initval_AUDI` blocks. This eliminates the need for the user to create and configure adapter instances beforehand; raw UDINT values can be wired directly. In contrast to a generic multiplexer without adapter support, this subapplication provides a complete, self-contained solution tailored to the AUDI adapter type, reducing external wiring complexity and improving reusability.

## Conclusion

The `AUDI_AUI_MUX_3_VAL` subapplication offers a compact and practical solution for three-way multiplexing of AUDI adapter values driven by event inputs. By internally combining value initialization, event selection, and adapter multiplexing, it simplifies the integration of numeric data into AUDI-based communication chains. Its clear interface, event-driven behavior, and adapter-oriented design make it suitable for a wide range of automation and control scenarios where flexible value switching is required.
