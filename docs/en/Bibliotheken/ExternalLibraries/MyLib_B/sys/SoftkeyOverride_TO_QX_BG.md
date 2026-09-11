# SoftkeyOverride_TO_QX_BG


![SoftkeyOverride_TO_QX_BG_network](./SoftkeyOverride_TO_QX_BG_network.svg)

![SoftkeyOverride_TO_QX_BG](./SoftkeyOverride_TO_QX_BG.svg)

* * * * * * * * * *

## Introduction

`SoftkeyOverride_TO_QX_BG` is a reusable IEC 61499 subapplication that combines a program signal with a manual softkey input and writes the result to a digital output of type `logiBUS_QX`. It is designed for digital outputs that need both automatic program control and manual hand operation. The OR combination ensures that the output can be forced active while the softkey is pressed, even if the program signal is inactive. A background visualization subapplication mirrors the output state for HMI purposes.

## Interface Structure

### **Event Inputs**

| Name | Type | Comment |
|------|------|---------|
| `REQ` | Event | Service Request; triggers evaluation and output update. |

### **Event Outputs**

There are no event outputs.

### **Data Inputs**

| Name | Type | Initial Value | Comment |
|------|------|---------------|---------|
| `OUT` | `BOOL` | — | Output data to resource. |
| `u16ObjId` | `UINT` | `ID_NULL` | Object ID Softkey/Button. |
| `Output` | `logiBUS::io::DQ::logiBUS_DO_S` | `logiBUS_DO::Invalid` | Selection of the physical/logical digital output. |

### **Data Outputs**

There are no data outputs.

### **Adapters**

There are no adapters.

## Functionality

The subapplication implements a digital output stage with manual override capability:

- The program signal `OUT` is combined with the softkey input `IX.IN` through an `OR_2` function block.
- The softkey input is read by an internal `Softkey_IX` block, which delivers the current key state and emits an event when the key state changes.
- The external `REQ` event also triggers the OR evaluation, so the output can be updated cyclically or on demand.
- The result of the OR operation is written to the `QX` digital output.
- The same result is sent to an internal `GreenWhiteBackground` subapplication, so the HMI background reflects the current output state.
- The `QX.CNF` event triggers the background update after the output has been written successfully.

The behavior is purely combinatorial with respect to the data values: the output is active if either the program signal `OUT` is true OR the softkey is currently pressed. There is no latching or memory.

## Technical Features

- Generic design: the same subapplication can be reused with different softkey object IDs and different digital output channels.
- Parameterizable softkey address via `u16ObjId`.
- Parameterizable output channel via `Output`.
- Supports event-driven processing through `REQ`.
- The softkey block can trigger processing independently via its internal event output.
- Output and background visualization are updated consistently.
- Uses standard logic block `OR_2` from IEC 61131 bitwise operators.
- The subapplication is packaged as an IEC 61499 subapplication type and can be embedded in larger applications.

## State Overview

The subapplication has no internal state machine, but the output state can be described by the following combinations:

| `OUT` | Softkey pressed | `QX` output | Background |
|-------|-----------------|-------------|------------|
| `FALSE` | `FALSE` | `FALSE` | Inactive |
| `TRUE`  | `FALSE` | `TRUE`  | Active |
| `FALSE` | `TRUE`  | `TRUE`  | Active |
| `TRUE`  | `TRUE`  | `TRUE`  | Active |

The manual softkey therefore acts as an "on" override. It can force the output to be active while pressed, but it cannot force the output off when the program signal is active.

## Application Scenarios

Typical use cases include:

- Digital actuators that must be operable both from a PLC program and from a local HMI softkey.
- Manual test or service functions for machine outputs.
- Lamp, relay, or valve control with operator override.
- HMI background elements that visualize whether an output is active.
- Generic output stages where the same logic must be reused for multiple channels.

## Comparison with Similar Blocks

Compared with a simple direct connection from `OUT` to a digital output, this subapplication adds:

- Manual softkey override capability.
- Event-driven softkey processing.
- HMI background mirroring.

Compared with a flip-flop or latch-based manual override, this subapplication does not store the manual command. The output remains active only while the softkey is pressed. When the softkey is released and `OUT` is false, the output returns to false.

Compared with an AND-based inhibit logic, this subapplication uses an OR combination, so it supports forced activation rather than forced deactivation. If manual off is required, a different logic structure would be needed.

## Conclusion

`SoftkeyOverride_TO_QX_BG` is a compact and reusable subapplication for digital outputs requiring both program control and manual hand operation. It combines a softkey input with a program signal using OR logic, writes the result to a `logiBUS_QX` output, and mirrors the state to an HMI background. Its generic parameters make it suitable for many output channels and softkey configurations in IEC 61499-based systems.
