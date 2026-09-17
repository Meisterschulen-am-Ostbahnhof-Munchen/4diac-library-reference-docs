# AX_AND_6

## Introduction

The `AX_AND_6` function block is a generic block for calculating a logical AND operation across six unidirectional AX adapter inputs. It is used to aggregate and monitor up to 6 input signals (e.g., emergency stop chains, enable or status signals) into a single adapter output signal.

## Interface Structure

### **Event Inputs**

None.

### **Event Outputs**

None.

### **Data Inputs**

None.

### **Data Outputs**

None.

### **Adapters**

**Plug Adapter (Output):**

- **OUT** – AND result (Adapter type: `adapter::types::unidirectional::AX`)

**Socket Adapters (Inputs):**

- **IN1** – AND input 1 (Adapter type: `adapter::types::unidirectional::AX`)
- **IN2** – AND input 2 (Adapter type: `adapter::types::unidirectional::AX`)
- **IN3** – AND input 3 (Adapter type: `adapter::types::unidirectional::AX`)
- **IN4** – AND input 4 (Adapter type: `adapter::types::unidirectional::AX`)
- **IN5** – AND input 5 (Adapter type: `adapter::types::unidirectional::AX`)
- **IN6** – AND input 6 (Adapter type: `adapter::types::unidirectional::AX`)

## How It Works

The function block evaluates the logical AND operation of all 6 AX inputs. The output `OUT.D1` is `TRUE` only when all six sockets `IN1.D1` through `IN6.D1` are simultaneously `TRUE`. If at least one input is `FALSE`, the output switches to `FALSE`.

## Change Filtering

The result is written to output plug `OUT` and its adapter event `OUT.E1` is fired only when the newly calculated value differs from the currently held value. If the logical result remains unchanged, no adapter event is generated – preventing event flooding in downstream SubApps.

## Technical Features

- **Generic Function Block**: Utilizes system class name `GEN_AX_AND` within the `adapter::booleanOperators` package.
- **Pure Adapter Interface**: Enables direct connection without manual event/data extraction.

## Application Scenarios

- Aggregated monitoring of up to 6 emergency stop or safety channels (e.g., STG1 I1–I6 emergency stop monitoring).
- Grouping multiple safety and interlock criteria.

## See Also

- [`AX_AND_2`](AX_AND_2.md) – 2-input AND block.
- [`AX_AND_3`](AX_AND_3.md) – 3-input AND block.
- [`AX_AND_4`](AX_AND_4.md) – 4-input AND block.
