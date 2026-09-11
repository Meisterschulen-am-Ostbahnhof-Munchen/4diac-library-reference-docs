# AUI_AUI_MUX_6_VAL


![AUI_AUI_MUX_6_VAL_network](./AUI_AUI_MUX_6_VAL_network.svg)

![AUI_AUI_MUX_6_VAL](./AUI_AUI_MUX_6_VAL.svg)

* * * * * * * * * *

## Introduction

AUI_AUI_MUX_6_VAL is a composite subapplication that implements a 6‑way multiplexer for UINT values, converting them into AUI adapter outputs. It combines six input events (EI1…EI6) with six data inputs (val1…val6) and a single AUI output adapter. Internally, it uses three functional blocks: an event‑driven multiplexer (AUI_MUX_6), an AUI selection unit (AUI_AUI_MUX_6), and six instances of the initialiser block initval_AUI. This design allows the selection of one of six UINT values via event triggers and provides the selected value as an AUI adapter output.

## Interface Structure

### **Event Inputs**

| Event | Type | Comment |
|-------|------|---------|
| EI1   | Event | Event to select val1 |
| EI2   | Event | Event to select val2 |
| EI3   | Event | Event to select val3 |
| EI4   | Event | Event to select val4 |
| EI5   | Event | Event to select val5 |
| EI6   | Event | Event to select val6 |

### **Event Outputs**

No event outputs are defined.

### **Data Inputs**

| Data Input | Type | Comment |
|------------|------|---------|
| val1       | UINT | Initial output value when EI1 is triggered |
| val2       | UINT | Initial output value when EI2 is triggered |
| val3       | UINT | Initial output value when EI3 is triggered |
| val4       | UINT | Initial output value when EI4 is triggered |
| val5       | UINT | Initial output value when EI5 is triggered |
| val6       | UINT | Initial output value when EI6 is triggered |

### **Data Outputs**

No direct data outputs are present; all output data is carried through the AUI adapter.

### **Adapters**

| Adapter | Type | Comment |
|---------|------|---------|
| OUT     | AUI (adapter::types::unidirectional::AUI) | Selected AUI adapter output |

## Functionality

The subapplication operates as a 6‑to‑1 selector for AUI‑encoded UINT values. When an event input (e.g., EI1) is activated, the corresponding UINT value (val1) is loaded into the respective internal initval_AUI block. These initval_AUI blocks convert the plain UINT data into an AUI adapter stream. The event multiplexer (AUI_MUX_6) forwards the triggering event to the selection block (AUI_AUI_MUX_6), which then switches the appropriate input adapter (from one of the six initval_AUI blocks) to the output adapter. Thus, the selected UINT value becomes available on the OUT adapter only after the corresponding event fires.

## Technical Features

- **Modular design**: Combines dedicated multiplexer, selection, and initialisation blocks.
- **Adapter‑based I/O**: All data exchange uses AUI type adapters, ensuring type safety and compatibility with other AUI‑compatible blocks.
- **Event‑driven selection**: Each input event directly controls the multiplexer, resulting in deterministic switching behaviour.
- **Parameterised initial values**: All six data inputs are UINT and are internally converted to AUI via initval_AUI blocks.
- **Scalable architecture**: The pattern can be extended to more channels by adding parallel blocks and events.

## State Overview

The subapplication does not expose an explicit state machine. Its behaviour is purely combinational and event‑driven: the state is implicitly defined by which event has been most recently received. Each event latches the corresponding UINT value into the AUI output. There is no internal persistent state beyond the current selection; however, the initval_AUI blocks hold the converted AUI values until overwritten by a new event.

## Application Scenarios

- **Control systems**: Selecting between multiple preset speeds, pressures, or setpoints based on discrete control signals.
- **Data routing**: Forwarding one of several measured UINT values to a common monitoring adapter.
- **Configuration management**: Loading predefined parameter sets into an AUI‑based process.
- **Test benches**: Simulating multi‑channel sensor selection for validation purposes.

## Comparison with Similar Blocks

| Feature                | AUI_AUI_MUX_6_VAL                | AUI_MUX_6 (standalone)           | AUI_AUI_MUX_6 (standalone)       |
|------------------------|----------------------------------|----------------------------------|----------------------------------|
| Input data type        | UINT (converted to AUI internally)| UINT (via events)                | AUI (direct adapters)            |
| Output type            | AUI adapter                      | AUI adapter                      | AUI adapter                      |
| Number of channels     | 6                                | 6                                | 6                                |
| Internal initialisers  | Yes (initval_AUI blocks)         | No                               | No                               |
| Event handling         | Yes (EI1…EI6)                    | Yes (EI1…EI6)                    | (depends on implementation)      |

The combined subapplication integrates the initialisation and selection logic, simplifying external wiring and ensuring consistent AUI conversion for all input values.

## Conclusion

AUI_AUI_MUX_6_VAL is a compact and reusable subapplication that provides a reliable 6‑way AUI multiplexer with UINT inputs. By encapsulating the conversion and selection logic, it reduces complexity for application developers and promotes clear, event‑driven control. Its design aligns with IEC 61499 standards and is well suited for industrial automation scenarios where multiple analogue or digital values must be selectively routed to a common adapter output.
