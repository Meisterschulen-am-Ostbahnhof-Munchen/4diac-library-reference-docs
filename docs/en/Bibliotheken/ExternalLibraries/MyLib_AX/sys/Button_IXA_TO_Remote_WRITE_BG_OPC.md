# Button_IXA_TO_Remote_WRITE_BG_OPC


![Button_IXA_TO_Remote_WRITE_BG_OPC_network](./Button_IXA_TO_Remote_WRITE_BG_OPC_network.svg)

![Button_IXA_TO_Remote_WRITE_BG_OPC](./Button_IXA_TO_Remote_WRITE_BG_OPC.svg)

* * * * * * * * * *
## Introduction
The **Button_IXA_TO_Remote_WRITE_BG_OPC** subapplication is a generic single‑channel solution that combines three core functions:  
- Reading a visual terminal (VT) button state via the `isobus::UT::io::Button::Button_IXA` function block.  
- Writing the button state to a remote target module using an OPC‑UA client adapter (`adapter::net::AX_CLIENT_1_0`).  
- Updating a VT background colour based on a remote subscription (`adapter::net::AX_SUBSCRIBE_1`) that feeds a background‑processing subapplication (`GreenWhiteBackground1_AX`).  

It is intended for outputs on modules that do not have their own VT connection, allowing the VT button to both send commands and reflect remote status.

## Interface Structure
The subapplication exposes only data inputs; it has no event inputs, event outputs, or external adapters. All internal communication is handled through data and adapter connections inside the subapplication.

### **Event Inputs**
None.

### **Event Outputs**
None.

### **Data Inputs**
| Name           | Type    | Initial Value | Comment                                                                 |
|----------------|---------|---------------|-------------------------------------------------------------------------|
| `u16ObjId`     | UINT    | `ID_NULL`     | Object ID of the button and background (VT)                         |
| `ID_SUBSCRIBE` | WSTRING | –             | Remote‑subscribe address for status/colour (ACTION=SUBSCRIBE)        |
| `ID_WRITE_REMOTE` | WSTRING | –          | Remote‑write address to the target module (ACTION=WRITE, CLIENT)    |

### **Data Outputs**
None.

### **Adapters**
None at the subapplication interface level. Adapters are used internally for OPC‑UA communication and subscription handling.

## Functionality
The subapplication works as follows:

1. **Button Reading** – The `Button_IXA` block reads the state of a VT‑associated button identified by `u16ObjId`. Its output is connected through an adapter to the OPC‑UA client block.
2. **Remote Write** – The `AX_CLIENT_1_0` block acts as an OPC‑UA client. It receives the button state via an adapter connection from `Button_IXA.IN` and writes it to the remote address specified by `ID_WRITE_REMOTE`. This enables the button to control an output on a module without a direct VT link.
3. **Remote Subscribe** – The `AX_SUBSCRIBE_1` block subscribes to a remote status/colour signal using the address given in `ID_SUBSCRIBE`. The subscribed data is passed via an adapter connection to the `GreenWhiteBackground1_AX` subapplication, which determines the appropriate VT background colour.
4. **Background Update** – The `GreenWhiteBackground1_AX` subapplication uses the received status (e.g., on/off, alarm) and the object ID to set the background colour of the same VT object that contains the button.

All three data inputs (`u16ObjId`, `ID_SUBSCRIBE`, `ID_WRITE_REMOTE`) are distributed internally to the respective blocks, ensuring full decoupling between the button input, remote write, and remote subscribe paths.

## Technical Features
- **Generic Design** – The subapplication is parameterised by three inputs, making it usable for any VT object, remote write target, and remote subscription source.
- **Integrated OPC‑UA Client and Subscriber** – Employs `AX_CLIENT_1_0` and `AX_SUBSCRIBE_1` adapters, which provide standardised OPC‑UA connectivity.
- **Built‑in Button Handling** – Uses the `Button_IXA` block, which encapsulates VT button state detection.
- **Background Colour Logic** – Delegates colour computation to the reusable `GreenWhiteBackground1_AX` subapplication, promoting modularity.
- **Fixed Operational Parameters** – The internal function blocks are initialised with `QI = TRUE` to enable continuous operation.
- **Input Types** – `u16ObjId` is a 16‑bit unsigned integer, while the two address inputs are wide strings, accommodating full OPC‑UA address strings.

## State Overview
The subapplication does not maintain its own explicit state machine. Instead, its behaviour is driven by the states of the internal components:

- **Button State** – Determined by the `Button_IXA` block (pressed/released).
- **Remote Communication State** – The `AX_CLIENT_1_0` and `AX_SUBSCRIBE_1` blocks manage connection establishment, data transfer, and error handling.
- **Background Colour State** – Based on the subscribed value, the `GreenWhiteBackground1_AX` subapplication outputs a colour (e.g., green for running, white for idle, or red for alarm).

Because the logic is entirely distributed, the subapplication can be considered a stateless composition that reacts to its inputs and the current network conditions.

## Application Scenarios
- **Remote Output Control without Local VT** – Use a VT pushbutton on a display panel to toggle an output on a remote I/O module that has no visual terminal of its own.
- **Status Indication via Colour** – Simultaneously, the same VT object’s background changes colour (e.g., green = active, white = inactive) based on a subscription to the remote output’s status.
- **Generic Single‑Channel Expansion** – The subapplication can be duplicated for multiple channels, each with its own object ID, write address, and subscribe address, providing a scalable control panel.
- **Legacy Module Integration** – Modules that lack native VT binding can be controlled and monitored through OPC‑UA while still being represented on a central VT screen.

## Comparison with Similar Blocks
Compared to simpler button‑to‑remote‑write blocks that only send the button state to a remote output, **Button_IXA_TO_Remote_WRITE_BG_OPC** adds the crucial background‑colour subscription. This extra capability provides immediate visual feedback on the VT, eliminating the need for separate status polling.  

In contrast to blocks that handle local output directly, this subapplication is specifically designed for remote destinations, offering OPC‑UA connectivity out of the box. The inclusion of the `GreenWhiteBackground1_AX` subapplication also separates colour logic from communication logic, making the solution more maintainable than a monolithic block with hard‑coded colour transitions.

## Conclusion
The **Button_IXA_TO_Remote_WRITE_BG_OPC** subapplication provides a compact, reusable, and generic solution for controlling remote outputs via a VT button while visualising their status through a background colour. By combining an OPC‑UA client for write operations, an OPC‑UA subscriber for status updates, and a dedicated background‑colour subapplication, it delivers a complete remote‑control and status‑indication package. Its simple interface (three data inputs) and clearly separated internal components make it easy to integrate into larger automation projects.