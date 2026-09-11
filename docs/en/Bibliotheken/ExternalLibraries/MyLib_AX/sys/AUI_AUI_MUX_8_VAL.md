# AUI_AUI_MUX_8_VAL


![AUI_AUI_MUX_8_VAL_network](./AUI_AUI_MUX_8_VAL_network.svg)

![AUI_AUI_MUX_8_VAL](./AUI_AUI_MUX_8_VAL.svg)

* * * * * * * * * *

## Introduction

The **AUI_AUI_MUX_8_VAL** is a composite subapplication that provides an 8‑way multiplexer for **UINT** values, which are presented on an **AUI** adapter interface. It selects one of eight input values based on an incoming event and forwards the selected value as an AUI output. This block is especially useful when a control system needs to switch between different parameter sets or configuration values in a structured, event‑driven manner.

## Interface Structure

### **Event Inputs**

| Name | Type   | Comment                       |
|------|--------|-------------------------------|
| EI1  | Event  | Event to select val1          |
| EI2  | Event  | Event to select val2          |
| EI3  | Event  | Event to select val3          |
| EI4  | Event  | Event to select val4          |
| EI5  | Event  | Event to select val5          |
| EI6  | Event  | Event to select val6          |
| EI7  | Event  | Event to select val7          |
| EI8  | Event  | Event to select val8          |

### **Event Outputs**

*None.*

### **Data Inputs**

| Name | Type  | Comment                          |
|------|-------|----------------------------------|
| val1 | UINT  | Initial output value for EI1     |
| val2 | UINT  | Initial output value for EI2     |
| val3 | UINT  | Initial output value for EI3     |
| val4 | UINT  | Initial output value for EI4     |
| val5 | UINT  | Initial output value for EI5     |
| val6 | UINT  | Initial output value for EI6     |
| val7 | UINT  | Initial output value for EI7     |
| val8 | UINT  | Initial output value for EI8     |

### **Data Outputs**

*None.*

### **Adapters**

| Name | Type                                       | Comment                     |
|------|--------------------------------------------|-----------------------------|
| OUT  | adapter::types::unidirectional::AUI        | Selected AUI adapter output |

## Functionality

The subapplication acts as a demultiplexer/selector for AUI‑encapsulated values. Internally, it combines three functional elements:

1. **AUI_MUX_8** – an event‑driven multiplexer block that receives the eight event inputs and converts them into a single selection signal (available on its `K` adapter output).
2. **initval_AUI_1 … initval_AUI_8** – eight instances of an AUI initialisation block. Each block accepts a UINT value (`INIT_VAL`) and provides it as an AUI output.
3. **AUI_AUI_MUX_8** – a selection block that receives the selection signal `K` from the first block and the eight AUI inputs from the initval blocks. It forwards exactly one of these inputs to the subapplication’s `OUT` adapter, depending on the active event.

When an event is received on `EIx` (x = 1 … 8), the corresponding input value `valx` is propagated to the output adapter. The internal architecture ensures that only the selected value is presented on `OUT`, while all other values remain isolated.

## Technical Features

- **8‑channel selection** – up to eight independent UINT input values can be managed.
- **Event‑driven switching** – selection occurs only upon an event, providing deterministic behaviour.
- **AUI adapter output** – the selected value is provided as an AUI type, allowing seamless integration with other adapter‑based components in the 4diac environment.
- **Internal initialisation** – each input value is converted to an AUI representation using dedicated initval blocks, simplifying the connection to the selection logic.
- **Modular design** – built from reusable standard blocks, making the subapplication easy to maintain and extend.

## State Overview

The subapplication does not maintain a persistent state. It is purely combinational with respect to the event inputs: after the reception of an event on `EIx`, the output reflects the value of `valx`. No internal state variables are used, and the block behaves identically for every execution cycle.

## Application Scenarios

Typical use cases for the `AUI_AUI_MUX_8_VAL` include:

- **Parameter switching** – selecting between different configuration sets (e.g., different motor profiles, sensor calibration values) based on external control events.
- **Mode selection** – choosing between operating modes in a machine or process, where each mode has its own set of parameters.
- **Test and simulation** – dynamically feeding different test values into a component without reconfiguring the network.
- **Redundancy management** – switching between primary and backup values in case of failure detection.

## Comparison with Similar Blocks

Compared to simpler multiplexers that operate directly on UINT data, `AUI_AUI_MUX_8_VAL` provides an **AUI‑typed output**, which is essential when the downstream components expect an adapter interface. Unlike a conventional event‑driven multiplexer that may require additional conversion steps, this subapplication integrates the conversion and selection in one block. It also supports eight channels, while many standard multiplexers are limited to fewer inputs. However, it does not offer an explicit “no selection” or invalid event state – only the eight defined event inputs are accepted.

## Conclusion

The `AUI_AUI_MUX_8_VAL` is a compact and reusable subapplication for event‑based selection among eight UINT values, presented as an AUI output. Its internal structure leverages proven building blocks, resulting in a clean and deterministic behaviour. It is suitable for a wide range of industrial automation and control applications where multiple parameter sets or modes need to be switched dynamically.
