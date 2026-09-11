# AUDI_AUI_MUX_6

![AUDI_AUI_MUX_6](./AUDI_AUI_MUX_6.svg)

* * * * * * * * * *

## Introduction

The **AUDI_AUI_MUX_6** function block is a generic 6‑to‑1 multiplexer specifically designed for use with unidirectional AUDI/AUI adapter types. It selects one of six input adapters (IN1…IN6) based on an index value provided via the K adapter, and forwards the selected data to a single output adapter (OUT). The block optimises communication by emitting an output event (CNF) only when the value on the selected input actually changes, thus reducing unnecessary network traffic and processing overhead.

The block belongs to the 4diac IDE environment and follows the IEC 61499 standard. It is intended for scenarios where multiple data sources need to be routed to a common destination, with a dynamic selection mechanism.

## Interface Structure

### **Event Inputs**

None.

### **Event Outputs**

- **CNF** – Confirmation event that indicates a successful update of the selected input value on the output adapter. This event is emitted only when the value transferred from the currently selected input differs from the previously forwarded value.

### **Data Inputs**

None.

### **Data Outputs**

None.

### **Adapters**

The block uses exclusively adapter interfaces to exchange data and control signals.

**Plugs (Output):**

- **OUT** (Type: `adapter::types::unidirectional::AUDI`) – The output adapter that carries the selected value. The value on this adapter is updated only when a change occurs on the currently selected input.

**Sockets (Input):**

- **K** (Type: `adapter::types::unidirectional::AUI`) – The index adapter. It supplies the selection index (0 to 5) that determines which of the six input adapters is routed to the output.
- **IN1** (Type: `adapter::types::unidirectional::AUDI`) – Input value 1, selected when K = 0.
- **IN2** (Type: `adapter::types::unidirectional::AUDI`) – Input value 2, selected when K = 1.
- **IN3** (Type: `adapter::types::unidirectional::AUDI`) – Input value 3, selected when K = 2.
- **IN4** (Type: `adapter::types::unidirectional::AUDI`) – Input value 4, selected when K = 3.
- **IN5** (Type: `adapter::types::unidirectional::AUDI`) – Input value 5, selected when K = 4.
- **IN6** (Type: `adapter::types::unidirectional::AUDI`) – Input value 6, selected when K = 5.

All adapters are unidirectional, meaning data flows in one direction: from the input sockets to the output plug.

## Functionality

The AUDI_AUI_MUX_6 acts as a data selector. It continuously reads the value from the input adapter selected by the current index K. If the value on that input differs from the last value forwarded to the output, the block updates the output adapter and triggers a **CNF** event to signal the update. If the value remains unchanged, no output update or event is generated, thereby avoiding unnecessary communication.

The selection index K is provided through a dedicated adapter, allowing the selection to be changed dynamically during operation. The block is designed for use with adapters that carry both data and control information, but the multiplexing logic focuses solely on the data part of the adapter.

## Technical Features

- **Generic Implementation** – The block is defined as a generic FB with the class name `GEN_AUDI_AUI_MUX`. This allows it to be instantiated with different underlying data types while preserving the same multiplexing behaviour.
- **Change‑driven Output** – The output adapter is updated only when the value on the currently selected input changes. This reduces network load and improves efficiency in distributed automation systems.
- **Index‑based Selection** – The selection index K is provided via an adapter interface, facilitating easy integration with other function blocks that can supply an AUI value.
- **No Data I/O** – The block has no dedicated data inputs or outputs; all data exchange occurs through the adapter interfaces.
- **Six Inputs, One Output** – Supports up to six different data sources, which can be selected arbitrarily.

## State Overview

The block does not expose complex internal states. Its behaviour can be described by two main conditions:

1. **Value unchanged** – The value on the selected input matches the value previously forwarded to the output. In this state, no update is performed and no CNF event is emitted.
2. **Value changed** – The value on the selected input differs from the previously forwarded value. The output adapter is updated with the new value and the CNF event is emitted.

The selection index K can change at any time, and the block immediately re‑evaluates the appropriate input.

## Application Scenarios

- **Data Aggregation** – Combining multiple sensor readings (e.g., temperature, pressure, humidity) into a single data stream, where the current sensor is chosen via an index.
- **Mode Switching** – Switching between different parameter sets or control modes in an automation process, where the active set is determined by a selection signal.
- **Output Multiplexing** – Routing different data sources to a common display, communication module, or control output.
- **Redundancy Handling** – Selecting between redundant sensors or data paths based on a health or prioritisation index.

## Comparison with Similar Blocks

Unlike a conventional multiplexer (e.g., a multi‑input FB with an integer index), the AUDI_AUI_MUX_6 uses adapter interfaces for both the selection and the data. This makes it particularly suitable for distributed systems where data exchange occurs over network connections following the AUDI/AUI protocol. The key difference from a simple value multiplexer is the change‑detection mechanism: most standard multiplexers unconditionally copy the selected input to the output, whereas this block suppresses redundant updates and only emits an event on actual change. This behaviour can significantly reduce network traffic in time‑critical or bandwidth‑limited environments.

## Conclusion

The AUDI_AUI_MUX_6 is a specialised function block for multiplexing up to six unidirectional AUDI data streams to a single output, with an index‑based selection via an AUI adapter. Its change‑driven output update mechanism and generic design make it a valuable component in IEC 61499‑based distributed automation systems where efficient data routing and minimal communication overhead are required. The block simplifies the integration of multiple data sources into a single sink, while maintaining high responsiveness and clear event‑based feedback.
