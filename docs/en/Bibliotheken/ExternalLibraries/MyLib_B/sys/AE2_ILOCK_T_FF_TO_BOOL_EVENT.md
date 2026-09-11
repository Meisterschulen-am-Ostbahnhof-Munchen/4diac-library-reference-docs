# AE2_ILOCK_T_FF_TO_BOOL_EVENT


![AE2_ILOCK_T_FF_TO_BOOL_EVENT_network](./AE2_ILOCK_T_FF_TO_BOOL_EVENT_network.svg)

![AE2_ILOCK_T_FF_TO_BOOL_EVENT](./AE2_ILOCK_T_FF_TO_BOOL_EVENT.svg)

* * * * * * * * * *
## Introduction

AE2_ILOCK_T_FF_TO_BOOL_EVENT is a reusable IEC 61499 subapplication that represents one chain link of a mutually interlocked toggle flip-flop chain. It combines a local toggle state with an AE2 bidirectional adapter chain, allowing many participants to be connected in series. Each instance exchanges interlock information through its AE2 SOCKET/PLUG pair, while the local state is provided as a BOOL output `Q` and a confirmation event `EO`.

The subapplication is designed as a generic chain element: the PLUG of one instance is connected to the SOCKET of the next instance. This makes the interlock chain scalable to any number of participants. The block is the BOOL/event-output variant of the sister block `AE2_ILOCK_T_FF_TO_AX`, which provides an adapter output instead.

## Interface Structure

### **Event Inputs**

| Name | Type | Description |
|------|------|-------------|
| `IND` | `Event` | Input event that triggers the toggle operation and starts the mutual interlock handshake. |

### **Event Outputs**

| Name | Type | Description |
|------|------|-------------|
| `EO` | `Event` | Output event emitted after the internal toggle/interlock operation has been processed. |

### **Data Inputs**

There are no data inputs.

### **Data Outputs**

| Name | Type | Description |
|------|------|-------------|
| `Q` | `BOOL` | Current Boolean state of the toggle chain link. |

### **Adapters**

| Name | Type | Role |
|------|------|------|
| `SOCKET` | `adapter::types::bidirectional::AE2` | Adapter socket that receives interlock/event information from the previous chain element. |
| `PLUG` | `adapter::types::bidirectional::AE2` | Adapter plug that sends interlock/event information to the next chain element. |

## Functionality

The subapplication implements one link of a chain of mutually interlocked toggle flip-flops. The core element is an `E_SR` set/reset flip-flop, whose output `Q` is directly exposed as the subapplication output. An `E_SWITCH` uses this value to route the incoming `IND` event to the appropriate logical path.

When the internal state `Q` is `FALSE`, the `IND` event is directed to the set path. The internal flip-flop is set, the state changes to `TRUE`, and the AE2 conversion FBs are triggered to propagate the interlock information to the adjacent chain elements through the adapter chain.

When the internal state `Q` is `TRUE`, the `IND` event is directed to the reset path. The internal flip-flop is reset, the state changes to `FALSE`, and the chain element releases the interlock.

The two AE2 conversion FBs, `AE2_EVENT_TO_E` and `AE2_E_TO_EVENT`, connect the local event logic to the bidirectional adapter chain. They handle the outgoing and incoming adapter event paths and support the mutual exclusion mechanism between neighbouring chain links.

## Technical Features

- Implements a toggle flip-flop (`T-FF`) with mutual interlock logic.
- Provides the state as a Boolean output `Q` and an event output `EO`.
- Uses the bidirectional AE2 adapter type for chain communication.
- Can be connected in a chain of arbitrary length by connecting `PLUG` to the next element's `SOCKET`.
- Uses standard IEC 61499 function blocks: `E_SR` and `E_SWITCH`.
- Encapsulates the complete interlock logic in a single reusable subapplication.
- Suitable for applications where a Boolean output is required instead of an adapter-based output.

## State Overview

- `Q = FALSE`: The chain link is inactive. A new `IND` event selects the set path, changes the state to `TRUE`, and starts the interlock handshake.
- `Q = TRUE`: The chain link is active. A new `IND` event selects the reset path, changes the state to `FALSE`, and releases the interlock.
- During the interlock handshake, the AE2 adapter chain communicates with neighbouring links to prevent conflicting states in the chain.

## Application Scenarios

- Cascaded mutual exclusion systems where only one participant may be active at a time.
- Toggle-based mode selection in multi-station automation systems.
- Daisy-chained interlock stations that pass control or state information from one element to the next.
- Applications requiring a Boolean output for direct use in visualization, PLC logic, or HMI integration.
- Multi-participant systems where new elements can be added simply by extending the PLUG-to-SOCKET chain.

## Comparison with Similar Blocks

| Block | Output Type | Main Difference |
|-------|-------------|-----------------|
| `AE2_ILOCK_T_FF_TO_BOOL_EVENT` | `BOOL Q` + `EO` event | Provides the state directly as a Boolean value and a confirmation event. |
| `AE2_ILOCK_T_FF_TO_AX` | Adapter output | Uses an AX adapter output instead of a Boolean/event output. |
| Plain Toggle Flip-Flop | `BOOL Q` | Toggles its output, but does not include mutual interlock logic or chain communication. |
| Standard `E_SR` / `E_RS` | `BOOL Q` | Provides set/reset behavior but no toggle chain or interlock coordination. |

## Conclusion

`AE2_ILOCK_T_FF_TO_BOOL_EVENT` is a compact and reusable subapplication for building mutually interlocked toggle chains. It combines standard IEC 61499 event logic with a bidirectional AE2 adapter interface. The Boolean output `Q` and event output `EO` make it easy to integrate into control logic, while the SOCKET/PLUG chain design allows scalable use in systems with many participants.