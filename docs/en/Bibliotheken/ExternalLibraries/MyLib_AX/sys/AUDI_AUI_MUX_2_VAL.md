# AUDI_AUI_MUX_2_VAL


![AUDI_AUI_MUX_2_VAL_network](./AUDI_AUI_MUX_2_VAL_network.svg)

![AUDI_AUI_MUX_2_VAL](./AUDI_AUI_MUX_2_VAL.svg)

* * * * * * * * * *

## Introduction

The **AUDI_AUI_MUX_2_VAL** is a composite application block (SubApp) that implements a two‑channel multiplexer for AUDI data values. It selects between two initialization values (`val1` and `val2`) based on incoming event triggers (`EI1` and `EI2`). The selected value is provided as an AUDI adapter output. Internally, the block combines an event‑driven selector (AUI_MUX_2) with a dedicated adapter multiplexer (AUDI_AUI_MUX_2) and two initial‑value generators (initval_AUDI). This design allows clean orchestration of event‑controlled value switching while maintaining the structure of AUDI adapters.

## Interface Structure

The block is a SubApp with the following interface elements:

### **Event Inputs**

| Name | Type   | Description |
|------|--------|-------------|
| `EI1` | Event  | Triggers selection of `val1` |
| `EI2` | Event  | Triggers selection of `val2` |

### **Event Outputs**

The block has no explicit event outputs. Internal event handling is entirely contained within the subapplication.

### **Data Inputs**

| Name | Type  | Description |
|------|-------|-------------|
| `val1` | UDINT | First value to be output when `EI1` is triggered |
| `val2` | UDINT | Second value to be output when `EI2` is triggered |

### **Data Outputs**

The block does not provide direct data outputs. The result is delivered through the adapter output.

### **Adapters**

| Name | Type | Description |
|------|------|-------------|
| `OUT` | `adapter::types::unidirectional::AUDI` | Output adapter carrying the currently selected AUDI value |

## Functionality

The internal network consists of three functional parts:

1. **Event decoding** – The incoming events `EI1` and `EI2` are routed to the internal FB `AUI_MUX_2` (type `adapter::events::unidirectional::AUI_MUX_2`). This FB acts as an event selector, forwarding the corresponding selection signal to the adapter multiplexer.

2. **Value preparation** – The two input values `val1` and `val2` are fed to two instances of `initval_AUDI` (type `adapter::types::unidirectional::AUDI::initval::initval_AUDI`). These FBs convert the primitive UDINT data into AUDI adapter format, making them suitable for the adapter‑based multiplexing stage.

3. **Adapter multiplexing** – The internal FB `AUDI_AUI_MUX_2` (type `adapter::selection::unidirectional::AUDI_AUI_MUX_2`) receives the selection control from `AUI_MUX_2` and the two prepared AUDI‑formatted values from the `initval_AUDI` instances. Based on the control signal, it passes either `IN1` or `IN2` to its output `OUT`, which is then propagated to the block’s external adapter output.

In summary, when `EI1` arrives, `val1` is output; when `EI2` arrives, `val2` is output. The output remains at the last selected value until the other event is received.

## Technical Features

- **Modular composition** – Uses event‑, data‑, and adapter‑oriented FBs in a clear separation of concerns.
- **Adapter‑based data flow** – All AUDI data is handled through dedicated adapter types, ensuring type‑safe connections.
- **Initial value handling** – The `initval_AUDI` FBs ensure that the UDINT input values are correctly wrapped into the AUDI adapter format before multiplexing.
- **Event‑driven selection** – Selection is performed via event inputs, allowing precise timing control.
- **Unidirectional output** – Only one AUDI output adapter is provided; no feedback or bidirectional interfaces are used.

## State Overview

The block does not maintain a persistent internal state machine; it is purely combinatorial with respect to data, while events drive the selection process. However, the output effectively “latches” the last selected value until an opposite event occurs. In that sense, the block can be considered to have two stable output states:

- **State A**: `val1` is active (after `EI1` has been received)
- **State B**: `val2` is active (after `EI2` has been received)

Since no internal memory elements are present, the state is determined solely by the most recent event that has been processed.

## Application Scenarios

This block is particularly useful in automation and control systems where a single AUDI‑type data stream must be switched between two different source values based on discrete events. Typical applications include:

- **Mode switching** – Selecting between different operating parameters (e.g., speed or temperature setpoints) based on operational mode events.
- **Redundant value selection** – Choosing between a primary and a backup measurement value depending on failure detection events.
- **Commanded swapping** – Alternating between two preset values (e.g., for test or calibration purposes) via digital triggers.

## Comparison with Similar Blocks

The `AUDI_AUI_MUX_2_VAL` is a specialized variant of a generic multiplexer. Unlike a plain data multiplexer (e.g., `MUX`), it works with AUDI adapter data and incorporates the necessary initial‑value conversion. Compared to a three‑channel version (`AUDI_AUI_MUX_3_VAL`), this block offers only two inputs, making it more compact for simple two‑source scenarios. The internal separation into event selector and adapter selector allows reuse of the individual components in other contexts, but for an end‑user the block behaves as a single, ready‑to‑use entity.

## Conclusion

The `AUDI_AUI_MUX_2_VAL` subapplication provides a clean and efficient way to switch between two AUDI‑formatted values based on event triggers. Its internal architecture leverages existing dedicated FBs, ensuring robustness and reusability. With its simple event‑driven interface and adapter‑based output, it integrates seamlessly into larger 4diac projects that require controlled data selection.
