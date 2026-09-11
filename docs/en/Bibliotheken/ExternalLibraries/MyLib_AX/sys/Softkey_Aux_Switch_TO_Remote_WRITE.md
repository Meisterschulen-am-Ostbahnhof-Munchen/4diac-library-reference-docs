# Softkey_Aux_Switch_TO_Remote_WRITE


![Softkey_Aux_Switch_TO_Remote_WRITE_network](./Softkey_Aux_Switch_TO_Remote_WRITE_network.svg)

![Softkey_Aux_Switch_TO_Remote_WRITE](./Softkey_Aux_Switch_TO_Remote_WRITE.svg)

* * * * * * * * * *

## Introduction

The `Softkey_Aux_Switch_TO_Remote_WRITE` subapplication is a 4‑source extension of the `Softkey_Aux_IXA_TO_Remote_WRITE` function block. It merges four independent command sources into a single OPC‑UA write request that is sent to a remote target module. The four sources are:

1. **SoftKey** – an ISO‑bus VT SoftKey (identified by `u16ObjId`).
2. **Auxiliary function** – an ISO‑bus Auxiliary Function 2 (identified by `u16ObjIdA`).
3. **Local Web override** – a subscription to a web client (e.g., `vt-ui-mirror`) on the same module (`ID_WEB_READ`).
4. **Remote physical switch** – an additional physical button on a different module, obtained via a remote subscription (`ID_SWITCH_REMOTE`).

All four sources are combined using an OR‑logic into a single command, which is then written to the target module via a generic one‑channel OPC‑UA client (`AX_CLIENT_1_0`) that uses the address provided in `ID_WRITE_REMOTE`. The subapplication is designed for scenarios such as cutting‑height adjustment, where a SoftKey on STG1, a physical button on STG4, and a web override must all trigger the same actuator on STG2 – without relying on multiple independent writers that could cause race conditions or last‑write‑wins errors.

This block does **not** provide status feedback or background colour highlighting; for that purpose, a separate block (e.g., `AX_SUBSCRIBE_BG3_WEB_OPC`) can be placed alongside, reusing the same object IDs.

## Interface Structure

The subapplication has no event inputs, event outputs, data outputs, or exposed adapters. Its interface consists solely of five data inputs, which are described below.

### **Event Inputs**

None.

### **Event Outputs**

None.

### **Data Inputs**

| Name | Type | Initial Value | Description |
|------|------|---------------|-------------|
| `u16ObjId` | `UINT` | `ID_NULL` | Object ID of the SoftKey (VT) | 
| `u16ObjIdA` | `UINT` | `ID_NULL` | Object ID of the AuxFunction2 (VT) |
| `ID_WRITE_REMOTE` | `WSTRING` | – | OPC‑UA address of the remote target module for the write command (ACTION=WRITE, CLIENT) |
| `ID_WEB_READ` | `WSTRING` | – | Local subscribe address for a web client (e.g., `vt-ui-mirror`) on the same module, OR‑combined with SoftKey and AUX |
| `ID_SWITCH_REMOTE` | `WSTRING` | – | Remote subscribe address of an additional physical button on a different module (e.g., `STG4_I2_REMOTE`), OR‑combined with the other sources |

### **Data Outputs**

None.

### **Adapters**

None (all adapter connections are internal to the subapplication).

## Functionality

The subapplication implements a logical OR of four boolean command sources, followed by a single OPC‑UA write to a remote target. Internally, it uses the following function blocks:

- `Softkey_IXA` – reads the SoftKey state via an ISO‑bus adapter and outputs a boolean.
- `Aux_IXA` – reads the Auxiliary Function 2 state and outputs a boolean.
- `SUBSCRIBE_WEB` – subscribes to the `ID_WEB_READ` address to receive a boolean from a web client.
- `SUBSCRIBE_SWITCH` – subscribes to the `ID_SWITCH_REMOTE` address to receive a boolean from a remote physical button.
- `OR_MERGE` (type `AX_OR_4`) – combines the four boolean inputs into a single output.
- `AX_CLIENT_1_0` – writes the merged boolean to the remote target using the `ID_WRITE_REMOTE` address.

The data flow is as follows:

1. The four sources are read continuously via their respective adapters/subscriptions.
2. The four boolean signals are fed into `AX_OR_4`, which produces a `TRUE` if any of the four is active.
3. The OR result is passed to `AX_CLIENT_1_0`, which sends an OPC‑UA write request to the configured remote address.

This design ensures that all commands from different inputs are combined into a single write operation, avoiding multiple writers that might conflict with each other. The subapplication is intended for use cases where a command must be triggered from multiple physical or logical locations but only one data point is written to the target.

## Technical Features

- **Generic OPC‑UA Client** – Uses a single `AX_CLIENT_1_0` adapter to write a boolean value to a remote node. The node address is provided via `ID_WRITE_REMOTE`.
- **Four‑input OR Combination** – Internal `AX_OR_4` merges SoftKey, AuxFunction, Web override, and remote switch signals.
- **Remote Subscription** – Uses `AX_SUBSCRIBE_1` adapters for both the web override and the remote switch, enabling cross‑module communication.
- **No Status Feedback** – The subapplication only sends commands; it does not receive or process status information from the target. For feedback, a separate subscription block must be used.
- **Configurable Object IDs** – Both VT object IDs (`u16ObjId` and `u16ObjIdA`) are passed through to the internal SoftKey and Aux blocks, allowing flexible mapping to ISO‑bus objects.
- **Initialisation** – The internal FB instances are initialised with `QI = TRUE` by default.

## State Overview

The subapplication does not implement an explicit state machine. Its behaviour is purely combinatorial: the logic continuously evaluates the four input sources and writes the OR result to the remote target. No internal states are stored, and no sequence or timing logic is applied. This makes it suitable for simple command‑forwarding tasks where immediate response is required.

## Application Scenarios

- **Cutting‑Height Adjustment** – A SoftKey on module STG1, a physical push‑button S06/S07 on module STG4, and a web override (via `vt-ui-mirror`) on STG1 all control an actuator on STG2. The subapplication combines these inputs and sends a single write command to STG2.
- **Multi‑Location Emergency Stop** – When a stop command can be issued from a panel, a remote button, or a web interface, this block forwards the first activated command to a safety‑relevant controller.
- **Generic Command Forwarding** – Any scenario where a boolean command must be generated from multiple sources and sent to a remote node with exactly one OPC‑UA write per transition.

## Comparison with Similar Blocks

- **vs. `Softkey_Aux_IXA_TO_Remote_WRITE`** – The present block adds a fourth input (`ID_SWITCH_REMOTE`), allowing a physical button on a third module to be included. The internal logic is identical except for the extended OR gate (4 inputs instead of 3).
- **vs. a network of separate `AX_CLIENT_1_0` blocks** – Using multiple writers to the same target would create race conditions and last‑write‑wins issues. This subapplication aggregates all sources into a single client, guaranteeing deterministic behaviour.
- **vs. blocks with status feedback** – This block is deliberately simple and does not provide feedback; a companion block (e.g., `AX_SUBSCRIBE_BG3_WEB_OPC`) must be used if status highlighting is required.

## Conclusion

`Softkey_Aux_Switch_TO_Remote_WRITE` provides a clean, robust solution for merging four command sources into one remote OPC‑UA write. Its architecture avoids common pitfalls of distributed control systems, such as conflicting writes, and keeps the interface minimal – only the necessary object IDs and communication addresses are exposed. It is well‑suited for applications where multiple physical or virtual controls must trigger a single actuator or function on a remote module, especially in agricultural and industrial machinery. The block is intentionally simple, offering no status feedback and requiring an external subscription block for full UI integration.