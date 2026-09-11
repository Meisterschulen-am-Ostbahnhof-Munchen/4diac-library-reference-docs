# AUI_AUI_MUX_8

![AUI_AUI_MUX_8](./AUI_AUI_MUX_8.svg)

* * * * * * * * * *
## Introduction

The **AUI_AUI_MUX_8** is a generic unidirectional multiplexer function block designed for selecting one of eight input adapters and forwarding its value to a single output adapter. It is part of the AUI (Agrartechnik Universal Interface) adapter family and provides a compact, event-driven solution for dynamic signal routing in automation applications.

This block acts as a selector: based on the index value provided on the **K** adapter, exactly one of the eight input channels (**IN1** to **IN8**) is connected to the output adapter **OUT**. The output is only refreshed when the selected index actually changes, reducing unnecessary network traffic and event load in distributed control systems.

The block is implemented as a generic function block (generic class name `GEN_AUI_AUI_MUX`), allowing reuse across different data types supported by the underlying AUI adapter type.

## Interface Structure

### **Event Inputs**

The block has **no event inputs**. All triggering is performed implicitly through value changes on the **K** index adapter.

### **Event Outputs**

| Event   | Description                                             |
|---------|---------------------------------------------------------|
| **CNF** | Confirmation event indicating that the index **K** has been successfully applied and the output **OUT** has been updated. |

### **Data Inputs**

The block has **no direct data inputs**. All input values are carried via adapter sockets.

### **Data Outputs**

The block has **no direct data outputs**. All output values are carried via the adapter plug.

### **Adapters**

#### Output Plug (Plugs)

| Adapter | Type                                 | Description                                              |
|---------|--------------------------------------|----------------------------------------------------------|
| **OUT** | `adapter::types::unidirectional::AUI` | Active output carrying the value of the selected input channel (IN1 for K = 0 … IN8 for K = 7). |

#### Input Sockets (Sockets)

| Adapter | Type                                 | Description                                              |
|---------|--------------------------------------|----------------------------------------------------------|
| **K**   | `adapter::types::unidirectional::AUI` | Index selector. Determines which input channel is forwarded to **OUT** (valid range: 0–7). |
| **IN1** | `adapter::types::unidirectional::AUI` | Input value 1, selected when K = 0.                      |
| **IN2** | `adapter::types::unidirectional::AUI` | Input value 2, selected when K = 1.                      |
| **IN3** | `adapter::types::unidirectional::AUI` | Input value 3, selected when K = 2.                      |
| **IN4** | `adapter::types::unidirectional::AUI` | Input value 4, selected when K = 3.                      |
| **IN5** | `adapter::types::unidirectional::AUI` | Input value 5, selected when K = 4.                      |
| **IN6** | `adapter::types::unidirectional::AUI` | Input value 6, selected when K = 5.                      |
| **IN7** | `adapter::types::unidirectional::AUI` | Input value 7, selected when K = 6.                      |
| **IN8** | `adapter::types::unidirectional::AUI` | Input value 8, selected when K = 7.                      |

## Functionality

The multiplexer operates on an event-driven basis:

1. The index value **K** is continuously monitored through its adapter socket.
2. Whenever **K** changes to a new value within the valid range (0–7), the block connects the corresponding input adapter (**IN1** through **IN8**) to the output adapter **OUT**.
3. The connected input's current value is copied to **OUT**.
4. After successful propagation, the `CNF` event is emitted to signal that the selection has been applied.

Key design characteristic: **the output is only updated when the index value actually changes**. If **K** is set repeatedly to the same index, no update occurs and no confirmation event is generated. This behavior minimizes unnecessary communication on the output adapter and reduces processing overhead in the connected downstream logic.

The block is declared as a generic FB (`eclipse4diac::core::GenericClassName = 'GEN_AUI_AUI_MUX'`), meaning it can be instantiated with type parameters to match the concrete AUI adapter variant required by the application.

## Technical Features

- **Eight-to-one multiplexing** with unidirectional AUI adapter type.
- **Event-based confirmation** – the `CNF` event confirms a successful index change and value propagation.
- **Change-only update strategy** – output is refreshed exclusively when the selected index changes, avoiding redundant transmissions.
- **Generic implementation** – can be specialized via the generic class name for different adapter data types.
- **Adapter-based interface** – no direct data pins; all data traffic flows through standardised AUI adapters, enabling easy connection to other AUI-compatible blocks.
- **Standard compliance** – conforms to IEC 61499-2 for distributed automation function blocks.
- **License** – distributed under the Eclipse Public License 2.0.

## State Overview

The block maintains an internal state representing the currently active index. The following state transitions occur:

| Current State | Trigger Condition                  | Action                                          | New State |
|---------------|------------------------------------|-------------------------------------------------|-----------|
| **Idle**      | No change on **K**                 | None (no output update, no event)               | Idle      |
| **Idle**      | **K** changes to valid index n     | Select **IN_(n+1)**, copy value to **OUT**, raise `CNF` | Active (index n) |
| **Active**    | **K** changes to a different valid index m | Select **IN_(m+1)**, copy value to **OUT**, raise `CNF` | Active (index m) |
| **Active**    | **K** set to same value again      | No action (no update, no event)                | Active (unchanged) |
| **Any**       | **K** out of valid range (0–7)     | No valid selection; output remains unchanged    | Last valid state |

The internal state is only the last applied index; no additional sequencing or timeout logic is required.

## Application Scenarios

- **Sensor selection in agricultural machinery**: Switch between multiple sensors (e.g., temperature, pressure, fill level) connected to a single processing unit, using an index signal to select the active measurement source.
- **Mode-dependent data routing**: Redirect different data sources to a common consumer depending on the current operating mode (e.g., automatic, manual, service).
- **Redundant channel handling**: Select between redundant or alternative data paths (e.g., primary vs. backup sensor) based on a health-check index.
- **Parameter multiplexing in distributed control**: Combine multiple configuration values arriving on separate AUI adapters into one output stream for a downstream controller.
- **Test and commissioning**: Dynamically switch between different test signal generators feeding a single measurement or logging unit.

## Comparison with Similar Blocks

| Block            | Type     | Number of Inputs | Update Behaviour                                      | Output Event          |
|------------------|----------|------------------|-------------------------------------------------------|-----------------------|
| **AUI_AUI_MUX_8** | Adapter-based | 8              | Only on index change                                  | Yes (`CNF`)           |
| Typical MUX (data pin) | Direct data | 2–16      | On every input change or on selector event            | Usually none or REQ/CNF |
| AUI_AUI_MUX_2     | Adapter-based | 2              | Only on index change                                  | Yes (`CNF`)           |
| DEMUX variants    | Adapter-based | 1 input, multiple outputs | Selects one of several outputs | Optional confirmation |

Compared to classic data-pin multiplexers, this block offers the advantage of a fully adapter-based interface, making it seamlessly interoperable with other AUI blocks in the 4diac environment. Its change-only update strategy distinguishes it from conventional multiplexers that propagate updates on every input variation, which can be beneficial in bandwidth-constrained or event-intensive systems.

## Conclusion

The **AUI_AUI_MUX_8** is a versatile, generic multiplexer block for the AUI adapter family, providing reliable eight-channel selection with an event-driven confirmation mechanism. Its adapter-only interface and change-detection logic make it well suited for modern distributed automation applications where communication efficiency and modularity are essential. By integrating seamlessly with the Eclipse 4diac suite and adhering to the IEC 61499 standard, it offers a robust solution for signal routing tasks in agricultural, industrial, and process automation environments.