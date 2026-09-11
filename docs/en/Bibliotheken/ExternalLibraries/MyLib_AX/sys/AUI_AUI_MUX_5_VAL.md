# AUI_AUI_MUX_5_VAL


![AUI_AUI_MUX_5_VAL_network](./AUI_AUI_MUX_5_VAL_network.svg)

![AUI_AUI_MUX_5_VAL](./AUI_AUI_MUX_5_VAL.svg)

* * * * * * * * * *
## Introduction

The **AUI_AUI_MUX_5_VAL** is a composite subapplication (SubAppType) that implements a 5-way multiplexer for AUI (Application Interconnection Unit) adapter values with UINT-based initial configuration. It allows the selection of one out of five incoming AUI data streams based on a corresponding event trigger. Internally, the subapp combines three functional building blocks: an event-driven multiplexer (`AUI_MUX_5`), a data-path selection adapter (`AUI_AUI_MUX_5`), and five initialization adapters (`initval_AUI`) that convert UINT values into AUI adapter outputs. This structure provides a clean and reusable way to route AUI data in IEC 61499-based automation systems, especially in scenarios where a single output adapter must be switched between multiple sources.

## Interface Structure

The subapplication exposes the following interface elements to the outside world:

### **Event Inputs**

| Name  | Type  | Description                         |
|-------|-------|-------------------------------------|
| EI1   | Event | Event to select input value `val1`  |
| EI2   | Event | Event to select input value `val2`  |
| EI3   | Event | Event to select input value `val3`  |
| EI4   | Event | Event to select input value `val4`  |
| EI5   | Event | Event to select input value `val5`  |

Each event input corresponds to a dedicated selection action. Triggering one of these events causes the internal multiplexer to route the associated data value to the output.

### **Event Outputs**

There are no event outputs in this subapplication. The selection process is purely input-driven; no completion or acknowledgment events are generated.

### **Data Inputs**

| Name  | Type  | Description                              |
|-------|-------|------------------------------------------|
| val1  | UINT  | Initial output value when EI1 is triggered |
| val2  | UINT  | Initial output value when EI2 is triggered |
| val3  | UINT  | Initial output value when EI3 is triggered |
| val4  | UINT  | Initial output value when EI4 is triggered |
| val5  | UINT  | Initial output value when EI5 is triggered |

These UINT values are forwarded to the internal `initval_AUI` adapters, which convert them into AUI adapter data structures. The values are applied whenever the corresponding event input is activated.

### **Data Outputs**

There are no direct data outputs in the interface list. The result of the multiplexing operation is delivered exclusively via the AUI adapter output.

### **Adapters**

| Name | Type                                           | Description                                 |
|------|------------------------------------------------|---------------------------------------------|
| OUT  | `adapter::types::unidirectional::AUI`          | Selected AUI adapter output containing the active data stream |

The `OUT` adapter is a unidirectional AUI interface that carries the data of the currently selected input channel.

## Functionality

The subapplication works as a data-selection switch with event-based control. Upon receiving an event at one of the five event inputs (`EI1` to `EI5`), the internal `AUI_MUX_5` block decodes the event and forwards a corresponding selection signal through its `K` adapter connection to the `AUI_AUI_MUX_5` block. Simultaneously, the UINT value associated with the triggered event (e.g., `val1` for `EI1`) is passed to the respective `initval_AUI` instance, which converts it into an AUI data structure. The `AUI_AUI_MUX_5` block then chooses the corresponding input (`IN1` to `IN5`) and routes its data to the `OUT` adapter.

The internal wiring is organized as follows:

- Each event input `EI1`–`EI5` is directly connected to the corresponding event input of the `AUI_MUX_5` block.
- Each UINT data input `val1`–`val5` feeds the `INIT_VAL` input of a dedicated `initval_AUI` instance.
- The five `initval_AUI` outputs (`OUT`) are connected to the `IN1`–`IN5` adapter ports of the `AUI_AUI_MUX_5` selection block.
- The selection event from `AUI_MUX_5` is transferred via its `K` adapter to the `K` port of `AUI_AUI_MUX_5`.
- The `OUT` port of the selection block is connected to the subapplication's external `OUT` adapter.

Thus, the overall behavior is a synchronous event-driven multiplexer: only one output is active at any time, and the active channel is determined exclusively by the most recent event input.

## Technical Features

- **Composite subapplication**: Encapsulates a complete multiplexing logic into a single reusable component.
- **Event-driven selection**: Selection is controlled via five discrete event inputs, making the block suitable for cyclic or edge-triggered control schemas.
- **UINT-to-AUI conversion**: The internal `initval_AUI` adapters handle the conversion from simple UINT data to the compound AUI adapter format, simplifying the external interface.
- **Modular internal design**: The subapp is built from three different block types (`AUI_MUX_5`, `AUI_AUI_MUX_5`, and `initval_AUI`), each with a clear and focused responsibility, improving maintainability and testability.
- **Unidirectional AUI output**: The external adapter interface is unidirectional, which fits well in publish-subscribe or point-to-point data flow architectures.
- **No explicit state variables**: The subapp relies entirely on the internal blocks' behavior; no additional state management is required in the composite layer.

## State Overview

The subapplication itself does not define an explicit state machine; its behavior is derived from the internal blocks. However, the effective operational states can be described as follows:

| State          | Trigger           | Description                                                                 |
|----------------|-------------------|-----------------------------------------------------------------------------|
| Idle           | No event received | The output retains the last selected value; no switching occurs.             |
| Channel 1 active | Event EI1       | Input `val1` is converted and routed to the `OUT` adapter.                   |
| Channel 2 active | Event EI2       | Input `val2` is converted and routed to the `OUT` adapter.                   |
| Channel 3 active | Event EI3       | Input `val3` is converted and routed to the `OUT` adapter.                   |
| Channel 4 active | Event EI4       | Input `val4` is converted and routed to the `OUT` adapter.                   |
| Channel 5 active | Event EI5       | Input `val5` is converted and routed to the `OUT` adapter.                   |

Each state transition is triggered by a rising edge of the corresponding event input. The output remains stable in the absence of new events.

## Application Scenarios

- **Multi-source data routing**: In distributed automation systems where a single consumer must receive data from one out of several producers, this block can serve as a central selection switch.
- **Parameterizable initialization**: When different startup values are required for different operational modes, the UINT inputs allow runtime or configuration-time initialization of each channel.
- **Mode switching**: In machine control applications, the block can switch between different reference values (e.g., speed, position, temperature) based on operator events.
- **Test and simulation environments**: The subapp can be used to inject different test values into a processing chain by simply toggling the event inputs.

## Comparison with Similar Blocks

| Feature                        | AUI_AUI_MUX_5_VAL                     | Simple UINT MUX                    | Generic AUI MUX (without init)   |
|--------------------------------|---------------------------------------|------------------------------------|----------------------------------|
| Input data type                | UINT (converted internally to AUI)    | UINT only                          | AUI (requires pre-configured sources) |
| Output data type               | AUI adapter                           | UINT                               | AUI adapter                      |
| Event inputs                   | 5 (EI1–EI5)                           | Typically 1 (with selector input)  | 5 (EI1–EI5)                      |
| Internal initialization        | Yes, via `initval_AUI` blocks         | No                                 | No                               |
| Type conversion handling       | Integrated, transparent to the user   | Not applicable                     | Not applicable                   |
| Use case complexity            | Medium – suitable for composite control | Low – simple data selection        | Medium – requires external AUI sources |

Compared to a plain UINT multiplexer, this subapp adds the advantage of AUI adapter compatibility. Compared to a generic AUI mux, it offers built-in UINT initialization, which reduces external wiring and makes it easier to configure values at design time or from HMI/PLC variables.

## Conclusion

The `AUI_AUI_MUX_5_VAL` subapplication provides a robust and modular solution for selecting between five AUI data streams based on event triggers. Its internal architecture cleanly separates event decoding, data conversion, and data selection, resulting in a highly reusable component. The integration of UINT-based initialization values simplifies configuration and makes the block particularly useful in applications where parameterized startup values or dynamic channel switching are required. By maintaining a purely unidirectional external interface and relying on standard 4diac adapter patterns, the block fits seamlessly into IEC 61499-compliant engineering workflows and can be deployed across a wide range of automation scenarios.