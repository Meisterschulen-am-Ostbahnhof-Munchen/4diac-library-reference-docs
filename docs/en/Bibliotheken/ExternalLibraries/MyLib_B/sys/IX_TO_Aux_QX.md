# IX_TO_Aux_QX


![IX_TO_Aux_QX_network](./IX_TO_Aux_QX_network.svg)

![IX_TO_Aux_QX](./IX_TO_Aux_QX.svg)

* * * * * * * * * *

## Introduction

The IX_TO_Aux_QX is an event-based subapplication designed to map digital inputs from a logiBUS (IX) to an isobus auxiliary output (Aux_QX). It provides a generic interface for converting logiBUS digital input signals into isobus auxiliary output requests, suitable for scenarios where auxiliary functions need to be controlled based on input signals. This subapplication encapsulates the logic to bridge the two different industrial communication systems, simplifying integration and promoting reusability.

## Interface Structure

### Event Inputs

- **None**: This subapplication does not expose any event input interfaces. All event handling is performed internally.

### Event Outputs

- **None**: This subapplication does not expose any event output interfaces. The internal event flow is managed within the encapsulated network.

### Data Inputs

- **Input** (Type: `logiBUS::io::DI::logiBUS_DI_S`): Identifies the logiBUS input signal (e.g., Input_I1 to Input_I8). The initial value is set to `Invalid`.
- **iInpNr** (Type: `USINT`): Specifies the auxiliary array number, corresponding to the order in the pool. The first auxiliary input has an `iInpNr` of 0. The initial value is 0.

### Data Outputs

- **None**: This subapplication does not provide any data output interfaces. The processed data is used internally or forwarded to connected blocks.

### Adapters

- **None**: No adapters are defined in the subapplication interface.

## Functionality

The IX_TO_Aux_QX subapplication encapsulates two primary function blocks: `logiBUS_IX` (IX) and `isobus::UT::io::Auxiliary::OUT::Aux_QX` (QX). It operates as follows:

- When the `logiBUS_IX` block receives a valid input signal, it triggers its `IND` event output.
- The `IND` event is connected to the `REQ` event input of the `Aux_QX` block, initiating a request to the isobus auxiliary output.
- Data mapping is performed via internal data connections:
  - The `Input` data variable is forwarded to `IX.Input`.
  - The `iInpNr` data variable is forwarded to `QX.iInpNr`.
  - The `IX.IN` output is internally connected to `QX.OUT`, although this does not expose a direct output from the subapplication.

This structure allows the subapplication to process a logiBUS digital input and trigger an isobus auxiliary output request based on the configured input number and signal.

## Technical Features

- **Event-driven Processing**: The subapplication uses an event-based mechanism to connect the input event from `IX` to the output request event of `QX`, ensuring timely response to input changes.
- **Parameterized Configuration**: The `Input` variable allows selection of specific logiBUS inputs (I1–I8), while `iInpNr` defines the order of the auxiliary array, enabling flexible setup.
- **Encapsulation**: By wrapping the function blocks, the subapplication provides a clean interface and hides the complexity of direct block wiring.
- **Reusability**: Designed as a generic solution, it can be reused across different projects requiring logiBUS-to-isobus bridging.
- **Internal Data Flow**: The data connection from `IX.IN` to `QX.OUT` indicates that the processed input data is passed internally, though not exposed externally.

## State Overview

This subapplication does not implement a complex state machine. Instead, it relies on the behavior of its internal function blocks. The state is primarily event-driven, where the activation of the `IND` event in `IX` directly influences the `REQ` event in `QX`. The subapplication is stable in idle state until an input event occurs, at which point it processes the data and generates a corresponding output request.

## Application Scenarios

- **Agricultural Machinery**: Used to convert physical digital inputs (e.g., switches on a control panel) into isobus auxiliary control commands, such as activating hydraulic functions or implement operations.
- **Industrial Automation**: Bridges logiBUS-based sensor inputs with isobus-controlled auxiliary devices in automated systems.
- **System Integration**: Provides a standardized interface for connecting logiBUS and isobus networks, reducing development effort and minimizing wiring errors.

## Comparison with Similar Blocks

Compared to directly implementing `logiBUS_IX` and `Aux_QX` individually, the IX_TO_Aux_QX subapplication offers several advantages:

- **Simplified Interface**: Exposes only two data inputs, making it easier to configure than separately wiring multiple blocks.
- **Generic Design**: The subapplication is parameterized to support different input numbers and array indices, whereas manual wiring may require reconfiguration for each instance.
- **Reusability**: As a self-contained unit, it can be copied and integrated into different projects without additional modifications.
- **Maintainability**: Encapsulation improves code organization and simplifies debugging, as the internal logic is isolated.

Similar blocks might include direct block combinations or other bridging subapplications, but IX_TO_Aux_QX is specifically optimized for logiBUS to isobus auxiliary output conversion, offering a balance of flexibility and simplicity.

## Conclusion

The IX_TO_Aux_QX subapplication is a valuable component for integrating logiBUS digital inputs with isobus auxiliary outputs. Its event-driven design, parameterized configuration, and encapsulated structure make it a practical solution for systems requiring efficient and reliable signal bridging. By simplifying the interface and promoting reusability, it enhances development productivity and system performance.
