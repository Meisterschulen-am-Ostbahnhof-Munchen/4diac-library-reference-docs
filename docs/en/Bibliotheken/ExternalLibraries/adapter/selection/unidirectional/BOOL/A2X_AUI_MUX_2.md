# A2X_AUI_MUX_2

![A2X_AUI_MUX_2](./A2X_AUI_MUX_2.svg)

* * * * * * * * * *

## Introduction

A2X_AUI_MUX_2 is a generic, adapter-based multiplexer for unidirectional IEC 61499 adapter connections. It selects one of two A2X input adapters using an AUI index adapter and forwards the selected value to an A2X output adapter. The output is only updated when the selected value actually changes, which helps to suppress unnecessary event traffic.

## Interface Structure

### **Event Inputs**

The FB does not declare any top-level event inputs. It is triggered through its connected adapter sockets.

### **Event Outputs**

| Name | Type | Comment |
|------|------|---------|
| CNF | Event | Confirmation of Set Index K. Emitted when the selected value has actually been changed. |

### **Data Inputs**

There are no direct data inputs. Input data is transported through the adapter sockets.

### **Data Outputs**

There are no direct data outputs. Output data is transported through the output adapter plug.

### **Adapters**

| Kind | Name | Adapter Type | Comment |
|------|------|--------------|---------|
| Plug | OUT | `adapter::types::unidirectional::A2X` | IN1 for K = 0, IN2 for K = 1 |
| Socket | K | `adapter::types::unidirectional::AUI` | Index |
| Socket | IN1 | `adapter::types::unidirectional::A2X` | Input value 1 (selected when K = 0) |
| Socket | IN2 | `adapter::types::unidirectional::A2X` | Input value 2 (selected when K = 1) |

## Functionality

A2X_AUI_MUX_2 selects one of two A2X input streams based on the index supplied by the AUI adapter:

- K = 0 selects IN1.
- K = 1 selects IN2.

The selected value is written to the output adapter OUT. The generic runtime implementation `GEN_A2X_AUI_MUX` compares the newly selected value with the value currently present on OUT. If the values are the same, the output is not updated and no event is emitted. If the value differs, OUT is updated and the CNF event confirms the operation.

## Technical Features

- Generic FB using the runtime class `GEN_A2X_AUI_MUX`.
- Fully adapter-based interface, no direct data I/O.
- Unidirectional data flow from the input adapters to the output adapter.
- Value-change detection avoids unnecessary output updates and events.
- Single confirmation event output: CNF.
- Part of the package `adapter::selection::unidirectional`.
- Compliant with the IEC 61499-2 type specification.

## State Overview

A2X_AUI_MUX_2 does not define an explicit ECC state machine. Instead, its behavior is implemented by the generic runtime class. Internally, the FB retains the last value written to OUT. When a new index is applied, the generic implementation checks whether the newly selected input differs from the stored value. If a change is detected, the output and internal state are updated, and CNF is raised. If no change is detected, the FB remains in its current output state and suppresses the confirmation event.

## Application Scenarios

- Selecting between two A2X data sources in an adapter-based automation system.
- Redundancy or failover switching based on an AUI index.
- Routing one of two adapter-based signal paths to a downstream consumer.
- Reducing event load by suppressing repeated identical values.
- Reusable selection logic in modular IEC 61499 applications.

## Comparison with Similar Blocks

Compared with `AX_AUI_MUX_2`, this FB is the A2X variant. It uses A2X adapter types for the data paths and an AUI adapter for the index.

Compared with a conventional MUX function block, A2X_AUI_MUX_2 has no direct data inputs or outputs. All data exchange is encapsulated in adapters, which improves reusability and type decoupling.

Compared with a simple gate or switch, this FB provides real selection logic and integrated change detection, making it suitable for event-driven selection scenarios without redundant updates.

## Conclusion

A2X_AUI_MUX_2 is a compact, generic, unidirectional multiplexer for IEC 61499 applications. It selects one of two A2X input adapters via an AUI index and forwards the selected value to an A2X output. The integrated value-change detection prevents unnecessary output events, making it well suited for event-driven automation and adapter-based system designs.
