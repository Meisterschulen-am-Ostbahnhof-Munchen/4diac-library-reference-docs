# AUDI_AUI_MUX_5_VAL


![AUDI_AUI_MUX_5_VAL_network](./AUDI_AUI_MUX_5_VAL_network.svg)

![AUDI_AUI_MUX_5_VAL](./AUDI_AUI_MUX_5_VAL.svg)

* * * * * * * * * *

## Introduction

The **AUDI_AUI_MUX_5_VAL** is a composite subapplication that implements a 5-way multiplexer for AUDI values. It combines an event-based selection mechanism with five independent initial value generators, allowing the user to select one of five AUDI data streams via dedicated event inputs. The block is designed to be used in scenarios where multiple AUDI sources need to be switched to a single output line based on control events.

## Interface Structure

### **Event Inputs**

| Event Name | Description |
|------------|-------------|
| EI1 | Event to select val1 as the active input |
| EI2 | Event to select val2 as the active input |
| EI3 | Event to select val3 as the active input |
| EI4 | Event to select val4 as the active input |
| EI5 | Event to select val5 as the active input |

### **Event Outputs**

None.

### **Data Inputs**

| Data Name | Type   | Description                        |
|-----------|--------|------------------------------------|
| val1      | UDINT  | Initial value used when EI1 is triggered |
| val2      | UDINT  | Initial value used when EI2 is triggered |
| val3      | UDINT  | Initial value used when EI3 is triggered |
| val4      | UDINT  | Initial value used when EI4 is triggered |
| val5      | UDINT  | Initial value used when EI5 is triggered |

### **Data Outputs**

None.

### **Adapters**

| Plug Name | Type                                 | Description                        |
|-----------|--------------------------------------|------------------------------------|
| OUT       | adapter::types::unidirectional::AUDI | Selected AUDI adapter output       |

## Functionality

The subapplication is built around three functional blocks:

- **AUI_MUX_5** – receives the five event inputs and generates a selection signal (K) based on which event has occurred.
- **initval_AUDI_1 … initval_AUDI_5** – each of these blocks holds one of the UDINT input values and converts it into an AUDI adapter output.
- **AUDI_AUI_MUX_5** – a dedicated AUDI multiplexer that takes the five AUDI inputs and the selection signal K, and routes the selected input to the output adapter.

When an event (EI1 … EI5) is triggered, the corresponding `val1 … val5` value is propagated via the internal initval block to the multiplexer, and the output adapter `OUT` provides the selected AUDI data stream.

## Technical Features

- **Composite subapplication** – encapsulates the entire logic in a single reusable block.
- **Five independent input channels** – each with its own event and data value.
- **UDINT data type** – supports 32-bit unsigned integer values for the initial value parameters.
- **AUDI adapter interface** – output is a standard unidirectional AUDI adapter, enabling easy integration with other AUDI-based components.
- **Event‑driven selection** – the output is updated only when one of the selection events occurs.
- **No event or data outputs** – all outputs are carried via the adapter plug.

## State Overview

The block does not have an explicit state machine, but its behavior can be described as:

- **Idle** – no event has been received; the output adapter holds the last selected value.
- **Selection** – upon receiving one of the five events, the corresponding input is latched and the output adapter is updated accordingly.

The internal AUI_MUX_5 and AUDI_AUI_MUX_5 ensure that exactly one input is routed to the output at any time.

## Application Scenarios

- **Industrial control** – switching between different sensor values (e.g., temperature, pressure) based on operational modes.
- **Automation** – selecting different recipe data for a production line.
- **Test and measurement** – multiplexing multiple measurement channels into a single acquisition system.
- **Component integration** – providing a unified AUDI interface to a higher‑level controller while allowing channel selection via events.

## Comparison with Similar Blocks

The block is a **5‑input variant** of the `AUDI_AUI_MUX_3_VAL` subapplication. Compared to a basic `AUI_MUX_5` (which only provides the selection signal), this block adds the initial value generation and the AUDI multiplexing in a single package, making it more self‑contained. It also differs from a simple `AUDI_MUX_5` (which works on pre‑configured AUDI inputs) by taking raw UDINT values as input and automatically converting them to the AUDI format.

## Conclusion

The `AUDI_AUI_MUX_5_VAL` subapplication provides a clean, event‑driven solution for multiplexing up to five AUDI channels. Its internal structure isolates the selection and conversion logic, making it easy to integrate into larger systems. The use of standard AUDI adapter interfaces ensures compatibility with existing 4diac components and simplifies wiring.