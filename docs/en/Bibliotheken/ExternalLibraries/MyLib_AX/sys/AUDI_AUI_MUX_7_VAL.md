# AUDI_AUI_MUX_7_VAL


![AUDI_AUI_MUX_7_VAL_network](./AUDI_AUI_MUX_7_VAL_network.svg)

![AUDI_AUI_MUX_7_VAL](./AUDI_AUI_MUX_7_VAL.svg)

* * * * * * * * * *
## Introduction

The **AUDI_AUI_MUX_7_VAL** is a 7‑way multiplexer subapplication designed to select one of seven AUDI values based on an event input. It extends the concept of a 3‑channel variant to support seven independent input channels. The block integrates event‑based selection with adapter‑based data transfer, making it suitable for scenarios where a single AUDI output must be dynamically switched among multiple pre‑configured values.

## Interface Structure

### **Event Inputs**

| Name  | Type  | Comment                        |
|-------|-------|--------------------------------|
| EI1   | Event | Event zur Auswahl von val1     |
| EI2   | Event | Event zur Auswahl von val2     |
| EI3   | Event | Event zur Auswahl von val3     |
| EI4   | Event | Event zur Auswahl von val4     |
| EI5   | Event | Event zur Auswahl von val5     |
| EI6   | Event | Event zur Auswahl von val6     |
| EI7   | Event | Event zur Auswahl von val7     |

### **Event Outputs**

None – the subapplication does not provide any event outputs.

### **Data Inputs**

| Name | Type   | Comment                                |
|------|--------|----------------------------------------|
| val1 | UDINT  | Initialer Ausgabewert bei EI1          |
| val2 | UDINT  | Initialer Ausgabewert bei EI2          |
| val3 | UDINT  | Initialer Ausgabewert bei EI3          |
| val4 | UDINT  | Initialer Ausgabewert bei EI4          |
| val5 | UDINT  | Initialer Ausgabewert bei EI5          |
| val6 | UDINT  | Initialer Ausgabewert bei EI6          |
| val7 | UDINT  | Initialer Ausgabewert bei EI7          |

### **Data Outputs**

None – all output data is provided through the adapter interface.

### **Adapters**

| Name | Type                                   | Comment                              |
|------|----------------------------------------|--------------------------------------|
| OUT  | adapter::types::unidirectional::AUDI   | Ausgewählter AUDI-Adapter Output     |

## Functionality

The subapplication operates as an event‑driven selector. When one of the event inputs (EI1…EI7) receives a rising edge or a trigger, the corresponding data value (val1…val7) is made available at the output adapter **OUT**. Internally, the following components work together:

- Seven **initval_AUDI** function blocks are used to convert the UDINT input values into the AUDI adapter format. Each block receives one of the `val` inputs and provides an AUDI‑typed output.
- The **AUI_MUX_7** event multiplexer forwards the incoming event from the selected input to a corresponding internal selection signal.
- The **AUDI_AUI_MUX_7** data multiplexer then routes the AUDI value from the selected channel to the output adapter.

The selection is purely event‑based; no additional control input is required. The output continuously reflects the value of the most recently triggered input channel until another event changes it.

## Technical Features

- **IEC 61499‑compliant** subapplication structure.
- **Event‑driven selection** – no polling or cycle‑based updates.
- **UDINT data type** for the input values, providing 32‑bit unsigned integer range.
- **Adapter‑based output** (AUDI) enabling seamless integration with other components of an AUDI‑based system.
- **Scalable design** – the internal architecture can be extended to support more channels if needed.
- **No explicit state variables**; the behaviour is deterministic and purely reactive.

## State Overview

The subapplication does not maintain an internal state machine. Its behaviour is purely combinatorial with respect to the last received event. The active channel is determined solely by the most recent occurrence of an event on EI1…EI7. Consequently, there is no hysteresis or memory beyond the current output value.

## Application Scenarios

- **Audio/video switching** – selecting among up to seven different audio sources (e.g., in a multimedia system).
- **Industrial control** – choosing one of several sensor values (encoded as AUDI) for further processing.
- **Configuration management** – switching between preset parameter sets at runtime.
- **Redundancy handling** – selecting a backup value if the primary becomes unavailable.

## Comparison with Similar Blocks

Compared to the ‑3‑channel version (`AUDI_AUI_MUX_3_VAL`), this block:

- Provides **seven input channels** instead of three.
- Has a **larger input data port set** (val1…val7).
- Uses a **seven‑fold internal replication** of the initialization logic.
- Fulfils the same functional role but for applications requiring more selection options.

Other multiplexer blocks might use a numeric selector input (e.g., QW_INT) instead of multiple event inputs; this variant excels when each channel is triggered by a distinct event (e.g., individual buttons or sensor events).

## Conclusion

The **AUDI_AUI_MUX_7_VAL** subapplication is a robust and flexible solution for event‑driven selection among up to seven AUDI values. By combining standard IEC 61499 elements with a clear event‑based interface, it offers a simple yet powerful mechanism to switch data sources in distributed automation and media systems. Its design ensures high reusability and easy integration into existing AUDI‑based architectures.