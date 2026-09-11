# Q_ChildPosition_AI

![Q_ChildPosition_AI](./Q_ChildPosition_AI.svg)

* * * * * * * * * *
## Introduction

The **Q_ChildPosition_AI** function block implements a command to change the position of a child object (Part 6, F.16) within an ISO 11783-6 compliant automation system. It acts as a wrapper around the `Q_ChildPosition` function block, replacing the conventional REQ/data input interface with two unidirectional AI adapter sockets. This allows the X and Y position values to be delivered through adapter connections, decoupling the position source from the command logic. A scaling option (xScale) enables the position values to be adjusted by the display-master/slave (DM/SKM) factor of the parent object.

## Interface Structure

### **Event Inputs**

| Event   | Type  | Comment                                                                    |
|---------|-------|----------------------------------------------------------------------------|
| INIT    | EInit | Service Initialization – triggers the initialization of the inner block.   |

The `INIT` event is associated with the data inputs `u16ObjId`, `u16ObjIdParent`, and `xScale`.

### **Event Outputs**

| Event | Type  | Comment                                                        |
|-------|-------|----------------------------------------------------------------|
| INITO | EInit | Initialization Confirm – acknowledged after `INIT` completes.  |
| CNF   | Event | Confirmation of the requested position-change service.          |

The `CNF` event is associated with the data outputs `STATUS`, `s16OldXposition`, `s16OldYposition`, and `s16result`.

### **Data Inputs**

| Name           | Type  | Comment                                                                                              |
|----------------|-------|------------------------------------------------------------------------------------------------------|
| u16ObjId       | UINT  | Object ID of the child object whose position is to be changed. Defaults to `ID_NULL`.                |
| u16ObjIdParent | UINT  | Object ID of the parent object (reference coordinate system). Defaults to `ID_NULL`.                 |
| xScale         | BOOL  | If FALSE (default), the X/Y positions are passed through unchanged. If TRUE, they are scaled by the DM/SKM factor of the parent object. |

### **Data Outputs**

| Name              | Type   | Comment                                                                 |
|-------------------|--------|-------------------------------------------------------------------------|
| STATUS            | STRING | Service status – passthrough from the internal `Q_ChildPosition`.       |
| s16OldXposition   | INT    | Previous X position of the child object – passthrough from the inner block. |
| s16OldYposition   | INT    | Previous Y position of the child object – passthrough from the inner block. |
| s16result         | INT    | Return value – passthrough from the internal `Q_ChildPosition`.        |

### **Adapters**

| Adapter Name   | Type                              | Comment                                                                 |
|----------------|-----------------------------------|-------------------------------------------------------------------------|
| s16Xposition   | adapter::types::unidirectional::AI| New X position relative to the top-left corner of the parent object.    |
| s16Yposition   | adapter::types::unidirectional::AI| New Y position relative to the top-left corner of the parent object.    |

Both adapters are unidirectional inputs (sockets). Each provides one event (`E1`) and one data value (`D1`). The adapter's event triggers the position update.

## Functionality

The block encapsulates an internal instance of `Q_ChildPosition`, called `Inner`. Its public interface is adapted as follows:

- On the `INIT` event, the inner block is initialized with the current values of `u16ObjId`, `u16ObjIdParent`, and `xScale`. The resulting `INITO` of the inner block is forwarded to the external `INITO`.
- A position update is triggered either by the event of the `s16Xposition` adapter or by the event of the `s16Yposition` adapter. In both cases, the internal `Q_ChildPosition.REQ` is fired. The actual position values are always read from both adapters' `D1` data pins at that moment – so an update triggered by the Y adapter will also pick up the latest X value, and vice versa. This design ensures that the command always uses the most current values from both sources.
- The data values `D1` of the adapters are connected directly to the corresponding inputs `s16Xposition` and `s16Yposition` of the inner block.
- All outputs (`STATUS`, `s16OldXposition`, `s16OldYposition`, `s16result`) of the inner block are passed through unchanged to the outer interface.

The block therefore acts as a pure adapter layer that preserves the full semantics and state machine of the underlying `Q_ChildPosition`.

## Technical Features

- **Adapter-based input decoupling** – X and Y positions are received via separate unidirectional AI adapter sockets rather than conventional event/data pairs. This allows a clean separation of data source and command logic in the surrounding network.
- **Independent event triggering** – Either adapter's event can initiate the position update, providing flexibility in the data flow topology.
- **Optional scaling** – The `xScale` flag enables automatic scaling of the position values using the DM/SKM factor of the specified parent object, without requiring external preprocessing.
- **Passthrough semantics** – All internal status and result data are forwarded transparently to the outer interface, so diagnostics and old-position reporting remain fully available.
- **Standard compliance** – Designed in accordance with ISO 11783-6, Part 6, command set F.16.

## State Overview

The block does not maintain its own state machine; it inherits the behavior of the encapsulated `Q_ChildPosition`. In general terms:

- After initialization (`INIT`/`INITO`), the block is ready to accept position-change requests.
- Whenever an adapter event occurs, a single request transaction is started on the inner block, which performs the required validation, scaling (if enabled), and position update.
- Upon completion, the inner block issues `CNF`, carrying the new status, the old coordinates, and the result code.

Because both adapter events are connected to the same inner REQ input, simultaneous or rapid successive events are serialized by the inner block's execution model; the system behaves deterministically with respect to the inner state machine.

## Application Scenarios

- **Decoupled HMI input handling** – When the X and Y coordinate values originate from separate UI widgets or external bus nodes, the AI adapters allow those sources to be connected directly to the position command without a central coordinator.
- **Distributed automation architectures** – In networked ISO 11783 installations, position sources can be located on different nodes, while the command logic resides in a central controller.
- **Scaled coordinate transformation** – For applications where the parent object uses a different coordinate scaling (DM/SKM factor), enabling `xScale` adapts the incoming raw values to the parent's coordinate system automatically.
- **Reusable wrapper pattern** – This block demonstrates how to adapt an existing command FB to an adapter-based interface, improving modularity and testability in larger FB networks.

## Comparison with Similar Blocks

Compared to the direct `Q_ChildPosition` block, the following differences apply:

| Aspect                  | Q_ChildPosition                       | Q_ChildPosition_AI                                    |
|-------------------------|---------------------------------------|-------------------------------------------------------|
| Position inputs         | Plain `s16Xposition` / `s16Yposition` data inputs, triggered by a single REQ event. | Position values arrive via two AI adapter sockets; either adapter event triggers the update. |
| Interface style         | Conventional input/output variables.  | Adapter-based input sockets, clean separation of event and data for each coordinate. |
| Scaling control         | Provided as input (xScale).           | Provided as input (xScale); behavior identical.       |
| Outputs                 | STATUS, old positions, result.        | All outputs passthrough unchanged.                    |
| Use case                | Direct command invocation in a fixed network. | Flexible connection of external position sources via adapters. |

The AI wrapper adds architectural flexibility but does not change the service semantics of the underlying command.

## Conclusion

`Q_ChildPosition_AI` is an adapter wrapper around the ISO 11783-6 child-position command, providing a clean, event-driven interface for X and Y coordinates via unidirectional AI sockets. It preserves all functionality of the original `Q_ChildPosition` – including status reporting, old-position feedback, and optional DM/SKM scaling – while enabling a more modular and flexible network design. This makes it well-suited for distributed control systems where position data originates from multiple independent sources.