# AX_TON_MERGE_QXA_OPC


![AX_TON_MERGE_QXA_OPC_network](./AX_TON_MERGE_QXA_OPC_network.svg)

![AX_TON_MERGE_QXA_OPC](./AX_TON_MERGE_QXA_OPC.svg)

* * * * * * * * * *
## Introduction

The `AX_TON_MERGE_QXA_OPC` function block (subapplication) implements a single channel of a switched group (e.g., a spotlight bank). It receives the toggle state from a common master via an adapter, delays the switch‑on by a configurable time, and merges the result with the existing IO‑test command for that channel. The combined signal drives the physical output and optionally publishes the state back for diagnostics. This design distributes capacitive inrush currents over time when multiple channels are switched together.

## Interface Structure

### **Event Inputs**

None – the block is data‑driven and uses internal event connections only.

### **Event Outputs**

None.

### **Data Inputs**

| Name             | Type                        | Description                                                               |
|------------------|-----------------------------|---------------------------------------------------------------------------|
| `Output`         | `logiBUS::io::DQ::logiBUS_DO_S` | Physical output for this channel (initial value: `logiBUS_DO::Invalid`). |
| `PT`             | `TIME`                      | Switch‑on delay relative to the master toggle. Set per instance (e.g., `T#0ms`, `T#800ms`, `T#1600ms`). Switch‑off is always immediate. |
| `ID_TEST_READ`   | `WSTRING`                   | Existing IO‑test subscribe address for this channel (e.g., `STG5_Q0x_READ`). |
| `ID_TEST_WRITE`  | `WSTRING`                   | Existing IO‑test publish address for this channel (e.g., `STG5_Q0x_WRITE`). **Must be on its own OPC UA node** (not the same as `ID_TEST_READ`) to avoid feedback/latching issues. |

### **Data Outputs**

None.

### **Adapters**

| Socket   | Type                            | Description                                                               |
|----------|---------------------------------|---------------------------------------------------------------------------|
| `MASTER` | `adapter::types::unidirectional::AX` | Receives the toggle state from a shared `TOGGLE_RPC_MASTER_QXA_OPC` block. |

## Functionality

1. The `MASTER` adapter supplies a toggle state (typically a boolean).  
2. An internal `AX_TON` timer delays only the *activation* of this state by the configured `PT`. Deactivation (switch‑off) propagates immediately.  
3. The delayed signal is combined with the IO‑test command via an `AX_OR_2` gate. The IO‑test command is obtained by subscribing to the OPC UA address `ID_TEST_READ`.  
4. The merged output is then split (`AX_SPLIT_2`) into two paths:  
   - **Path 1** drives the physical output via `logiBUS_QXA`.  
   - **Path 2** publishes the state back to the OPC UA address `ID_TEST_WRITE` for monitoring or test feedback.  

The complete signal flow is internal and appears as a single logical unit to the application.

## Technical Features

- **Configurable switch‑on delay**: Each instance can have a distinct `PT` value, allowing a staggered startup of multiple channels.  
- **Immediate switch‑off**: No delay is applied to the falling edge, ensuring rapid deactivation.  
- **Integration with existing IO‑test infrastructure**: Reuses subscribe/publish addresses to merge manual test commands with the master toggle.  
- **Decoupled OPC UA nodes**: Separate read and write addresses prevent self‑feedback and unintended latching.  
- **Physical output handling**: The block maps directly to a `logiBUS_QXA` digital output, abstracting low‑level hardware access.

## State Overview

This subapplication does not define an explicit state machine; its behavior is deterministic and solely based on the input data and the internal timer logic. The `AX_TON` component internally manages the delay timing, but no user‑visible states are exposed.

## Application Scenarios

**Staggered activation of a spotlight bank**  
Consider a bank of six LED channels, each connected to the same master toggle via a shared `TOGGLE_RPC_MASTER_QXA_OPC`. By assigning increasing `PT` values (e.g., 0 ms, 800 ms, 1600 ms, …), the capacitive inrush currents of the LED drivers are spread over time instead of being switched simultaneously. This prevents overloads and reduces electrical stress.

**Merging IO‑test commands**  
When the IO‑test system issues a command for a channel (via `ID_TEST_READ`), the `AX_OR_2` gate combines it with the master‑derived toggle. The physical output is driven by either source, and the resulting state is published back on `ID_TEST_WRITE` for verification.

## Comparison with Similar Blocks

- **`AX_TON_MERGE_QXA_OPC`** (this block) – for channels that share a common master toggle and require a staggered switch‑on. It adds a delay and merges with the IO‑test input.  
- **`TOGGLE_RPC_MERGE_QXA_OPC`** – for completely independent channels with their own soft‑key, no shared master.  
- **`MERGE_SWITCH_1_QXA_OPC`** – a simpler variant without the delayed‑on feature, suitable when inrush spreading is not required.

Choose `AX_TON_MERGE_QXA_OPC` when you need to synchronize multiple channels from one master while avoiding simultaneous switching peaks.

## Conclusion

`AX_TON_MERGE_QXA_OPC` provides a clean, reusable solution for controlling a switched group with controlled switch‑on timing. Its integration with existing IO‑test mechanisms and support for per‑channel delays make it ideal for applications with capacitive loads and multiple outputs. The block encapsulates the required logic, simplifying application design and maintenance.