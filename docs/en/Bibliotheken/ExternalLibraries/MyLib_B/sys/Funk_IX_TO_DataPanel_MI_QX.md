# Funk_IX_TO_DataPanel_MI_QX


![Funk_IX_TO_DataPanel_MI_QX_network](./Funk_IX_TO_DataPanel_MI_QX_network.svg)

![Funk_IX_TO_DataPanel_MI_QX](./Funk_IX_TO_DataPanel_MI_QX.svg)

* * * * * * * * * *

## Introduction

The `Funk_IX_TO_DataPanel_MI_QX` is a generic, event‑based subapplication that bridges a wireless input module (Funk IX) to a DataPanel output module (MI QX). It is designed to be reused in various contexts where a single digital input event (e.g., a key press) must be forwarded to a corresponding digital output on a DataPanel. The subapp encapsulates the wiring and synchronisation between these two function blocks, providing a clean and configurable interface.

## Interface Structure

The subapplication exposes three data inputs, no event inputs or outputs, and no adapters. All communication with the outside world is performed via these data ports.

### **Event Inputs**

None.

### **Event Outputs**

None.

### **Data Inputs**

| Name | Type | Initial Value | Description |
|------|------|---------------|-------------|
| `Input` | `Funk::io::DI::Funk_DI_S` | `Funk_DI::Invalid` | Identifies the incoming digital input (e.g., `DigitalInput_Key_01`). |
| `u8SAMember` | `USINT` | `MI::MI_00` | Node SA address (range 224..239) used for the output module. |
| `Output` | `DataPanel::io::MI::DQ::DataPanel_MI_DO_S` | `Invalid` | Identifies the output channel on the DataPanel (e.g., `DigitalOutput_1A..8B` or `Input_Power_Port_5..8`). |

### **Data Outputs**

None.

### **Adapters**

None.

## Functionality

The subapplication connects an instance of the function block `Funk::io::DI::Funk_IX` to an instance of `DataPanel::io::MI::DQ::DataPanel_MI_QX`. The internal wiring is as follows:

- **Event connection**: The event output `IND` of the Funk block (IX) is connected to the event input `REQ` of the DataPanel block (QX). This ensures that every time a new value is received by the Funk input, the DataPanel output is triggered.
- **Data connection**: The data output `IN` of the Funk block is connected to the data input `OUT` of the DataPanel block. This transfers the actual value (e.g., a button state) from the wireless module to the output stage.

The three external data inputs of the subapplication are passed directly to the corresponding parameters of the internal blocks:

- `Input` is forwarded to `IX.Input`
- `u8SAMember` is forwarded to `QX.u8SAMember`
- `Output` is forwarded to `QX.Output`

Both internal blocks have their `QI` parameter set to `TRUE`, so they are permanently enabled. The `PARAMS` parameter of the Funk block is set to an empty string and is hidden in the configuration.

## Technical Features

- **Generic configuration**: The subapplication can be reused for any combination of Funk digital input and DataPanel output by simply setting the three data inputs.
- **Event‑driven operation**: The forwarding of an input event to the output is synchronous and triggered by the Funk block’s `IND` event.
- **No internal state**: The subapplication itself does not contain any state variables; it only establishes the data and event flow between the two function blocks.
- **Compile‑time optimised**: All connections are defined in the subapplication network, allowing the compiler to inline or optimise the data paths.

## State Overview

The subapplication does not maintain any state of its own. The behaviour is fully determined by the states of the two internal function blocks:

- **Funk IX**: Waits for a new input event. On arrival, it updates its `IN` output and issues an `IND` event.
- **DataPanel QX**: Upon receiving the `REQ` event, it reads the `OUT` data input and applies the value to the configured output channel.

There is no explicit state machine in the subapplication; the overall system state is the combination of the states of the internal blocks.

## Application Scenarios

This subapplication is particularly useful in scenarios where a wireless remote control or sensor needs to control a physical output on a DataPanel. Typical use cases include:

- Wireless keypad to activate a light or motor output.
- Remote button to trigger a digital output on a DataPanel in an industrial or building automation environment.
- Generic bridging of any `Funk_DI` input to a `DataPanel_MI_DO` output, with the flexibility to choose the exact input and output channel at runtime.

Because the configuration is done via data inputs, the same subapplication instance can be re‑parameterised without changing the internal structure, making it ideal for reusable library components.

## Comparison with Similar Blocks

The subapplication is tailored to the specific combination of Funk input and DataPanel output. Similar blocks might include:

- **Direct Function Block Connection**: Instead of a wrapper subapplication, a designer could directly connect a `Funk_IX` to a `DataPanel_MI_QX` in the parent application. This would be more flexible but would expose internal details and require manual wiring for every new instance.
- **Other bridge blocks**: Other subapplications may exist that combine different input and output types (e.g., Modbus to DataPanel). However, this subapplication is specialised for the Funk wireless input family.

Compared to a direct connection, the subapplication offers the advantages of encapsulation, reusability, and a simplified interface. It also ensures that the correct event and data paths are always used, reducing the chance of wiring errors.

## Conclusion

`Funk_IX_TO_DataPanel_MI_QX` is a compact and reusable subapplication that provides a clean bridge between a Funk digital input and a DataPanel digital output. Its event‑based nature and configurable parameters make it a practical building block for automation projects that require wireless‑to‑output connectivity. The subapplication hides the complexity of the internal wiring and can be easily integrated into larger systems, promoting maintainability and consistency.
