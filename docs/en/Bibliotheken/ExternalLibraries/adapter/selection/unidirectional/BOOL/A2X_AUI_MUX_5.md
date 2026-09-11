# A2X_AUI_MUX_5

![A2X_AUI_MUX_5](./A2X_AUI_MUX_5.svg)

* * * * * * * * * *

## Introduction

A2X_AUI_MUX_5 is an adapter-based unidirectional multiplexer for IEC 61499 applications. It selects one of five A2X input adapters, **IN1** to **IN5**, based on the index value supplied by the **K** adapter. The selected value is forwarded to the **OUT** A2X output adapter.

The function block is implemented as a generic FB and uses change detection to minimize unnecessary adapter traffic. The output adapter **OUT** is only updated when the selected value actually changes. In this case, the **CNF** event is emitted.

## Interface Structure

The block communicates mainly through adapters. There are no conventional event inputs, data inputs, or data outputs. All value data is exchanged through the A2X and AUI adapter connections.

### **Event Inputs**

There are no event inputs defined for this function block.

### **Event Outputs**

| Name | Type | Comment |
|------|------|---------|
| CNF | Event | Confirmation that the index K has been applied and that the output value has actually changed. |

### **Data Inputs**

There are no data inputs defined for this function block.

### **Data Outputs**

There are no data outputs defined for this function block.

### **Adapters**

| Direction | Name | Adapter Type | Comment |
|-----------|------|--------------|---------|
| Socket | K | adapter::types::unidirectional::AUI | Selection index. |
| Socket | IN1 | adapter::types::unidirectional::A2X | Input value 1, selected when K = 0. |
| Socket | IN2 | adapter::types::unidirectional::A2X | Input value 2, selected when K = 1. |
| Socket | IN3 | adapter::types::unidirectional::A2X | Input value 3, selected when K = 2. |
| Socket | IN4 | adapter::types::unidirectional::A2X | Input value 4, selected when K = 3. |
| Socket | IN5 | adapter::types::unidirectional::A2X | Input value 5, selected when K = 4. |
| Plug | OUT | adapter::types::unidirectional::A2X | Selected A2X value forwarded to the output. |

## Functionality

A2X_AUI_MUX_5 behaves as a 5-to-1 adapter multiplexer:

1. The index value is received through the **K** AUI adapter.
2. The corresponding input adapter **IN1** to **IN5** is selected.
3. The value of the selected input is forwarded to the **OUT** adapter.
4. The **CNF** event is emitted only when the output value actually changes.

If the index changes to an input that carries the same value as the current output, the output is not updated and **CNF** is not emitted. This avoids unnecessary event and data propagation in the connected IEC 61499 application.

Since the FB is a generic function block, the actual behavior is delegated to the generic class **GEN_A2X_AUI_MUX**. The runtime environment must provide this generic backend.

## Technical Features

- 5-to-1 adapter-based multiplexing.
- Unidirectional data flow through A2X and AUI adapters.
- Change-triggered output update.
- Event output **CNF** is only generated on actual value changes.
- No scalar data inputs or outputs are required.
- Generic FB design using `eclipse4diac::core::GenericClassName`.
- Generic backend: `GEN_A2X_AUI_MUX`.
- Located in the adapter package `adapter::selection::unidirectional`.
- Designed according to the IEC 61499-2 FB type identification structure.

## State Overview

The FBType XML does not define an explicit ECC. Instead, the behavior is provided by the generic backend. Conceptually, the function block can be described using two states:

| State | Description |
|-------|-------------|
| Idle | Waiting for a change on the K index or on the currently selected input adapter. |
| Updating | The selected value has changed; OUT is updated and CNF is emitted. |

After an update, the block returns to the idle state. If an incoming value or a new index selection does not produce a different output value, no state transition to updating occurs and no event is emitted.

## Application Scenarios

- **Source selection:** Connect up to five A2X data sources and dynamically select the active source using an AUI index.
- **Event-efficient data forwarding:** Use **CNF** to trigger downstream logic only when the output value really changes.
- **Redundancy handling:** Switch between redundant input channels without propagating duplicate values.
- **Adapter-based system integration:** Integrate the block into unidirectional adapter chains where typed adapter connections are preferred over scalar data wiring.

## Comparison with Similar Blocks

Compared to the **AX_AUI_MUX_5** block, this FB is the A2X variant and uses the A2X adapter type for its data interfaces. The main improvement is the change-detection behavior: **CNF** is suppressed when the selected value has not actually changed.

Compared to a conventional multiplexer FB with scalar data inputs and a numeric selector, A2X_AUI_MUX_5 uses typed adapters and provides a unidirectional connection style. This makes it more suitable for modular, adapter-oriented IEC 61499 architectures.

## Conclusion

A2X_AUI_MUX_5 is a compact and event-efficient adapter multiplexer for selecting one of five A2X input channels. Its unidirectional adapter interface, generic implementation, and change-triggered confirmation event make it suitable for modern IEC 61499 applications where adapter-based communication and reduced event traffic are important.
