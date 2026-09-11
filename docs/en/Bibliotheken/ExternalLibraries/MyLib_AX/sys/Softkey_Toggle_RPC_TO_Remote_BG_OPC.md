# Softkey_Toggle_RPC_TO_Remote_BG_OPC


![Softkey_Toggle_RPC_TO_Remote_BG_OPC_network](./Softkey_Toggle_RPC_TO_Remote_BG_OPC_network.svg)

![Softkey_Toggle_RPC_TO_Remote_BG_OPC](./Softkey_Toggle_RPC_TO_Remote_BG_OPC.svg)

* * * * * * * * * *

## Introduction

Softkey_Toggle_RPC_TO_Remote_BG_OPC is a composite subapplication used on device A, e.g. Station 11 at IP address 192.168.1.11. It combines a softkey input, an OPC UA client request, and a state-monitoring adapter to provide a remote toggle control with visual feedback.

When the F1 softkey is released, represented by `SK_RELEASED`, the subapplication sends an argument-less and return-value-less OPC UA method call to a remote device B through a `CLIENT_0` function block. This is a pure remote procedure call trigger. No value-change trick and no additional flip-flop block is required on device A. The actual flip-flop state is monitored on device B and displayed locally by a `GreenWhiteBackground` composite, which shows either green or white depending on the remote state.

The protocol logic is encapsulated inside the `MyLib::sys` composite subapplication, not directly in the device resource.

## Interface Structure

The subapplication exposes only data inputs. It has no externally visible event inputs, event outputs, data outputs, or adapter interfaces.

### **Event Inputs**

None.  
All event generation is handled internally by the softkey input block.

### **Event Outputs**

None.

### **Data Inputs**

| Name | Type | Initial Value | Description |
|------|------|---------------|-------------|
| `u16ObjId` | `UINT` | `ID_NULL` | Object ID used for the softkey and the background element. |
| `ID_TRIGGER_CALL` | `WSTRING` | – | Remote OPC UA method address with `ACTION=CALL_METHOD`. Used for the argument-less trigger method call on device B. |
| `ID_STATE_READ` | `WSTRING` | – | Locally monitored OPC UA address for the flip-flop state. The address is of type `BOOL` and uses `ACTION=READ`. It is written remotely by device B. |

### **Data Outputs**

None.

### **Adapters**

None in the external interface.  
Internally, the adapter output `STATE_SUBSCRIBE.OUT` is connected to the adapter input `GreenWhiteBackground_AX.DI1`.

## Functionality

The subapplication performs two main tasks: triggering a remote OPC UA method call and visualizing the remote flip-flop state.

1. `SoftKey_UP_F1`, an instance of `isobus::UT::io::Softkey::Softkey_IE`, is configured with `QI = TRUE` and `InputEvent = SK_RELEASED`.  
2. When the F1 softkey is released, the internal event output `IND` of `SoftKey_UP_F1` is emitted.  
3. This event is connected to the `REQ` input of `TRIGGER_CLIENT`, an instance of `iec61499::net::CLIENT_0`.  
4. `TRIGGER_CLIENT` uses the `ID_TRIGGER_CALL` address to execute the remote OPC UA method on device B. The method has no arguments and no return value, so a single event-triggered request is sufficient.  
5. In parallel, `STATE_SUBSCRIBE`, an instance of `adapter::net::AX_SUBSCRIBE_1`, monitors the `ID_STATE_READ` address and provides the current flip-flop state through its adapter output `OUT`.  
6. This adapter output is connected to `GreenWhiteBackground_AX.DI1`.  
7. `GreenWhiteBackground_AX` uses `u16ObjId` to address the correct background element and displays the state as green or white.

The result is a clean remote toggle operation: the user presses F1, device B toggles its internal flip-flop, and device A displays the new state.

## Technical Features

- Composite subapplication type belonging to the `MyLib::sys` package.
- Uses `isobus::UT::io::Softkey::Softkey_IE` for softkey event detection.
- Uses `iec61499::net::CLIENT_0` for the OPC UA remote method call.
- Uses `adapter::net::AX_SUBSCRIBE_1` for adapter-based state subscription.
- Uses `MyLib::sys::GreenWhiteBackground1_AX` for green/white state visualization.
- Configuration is done through data inputs only; no external event wiring is necessary.
- No internal state machine is required.
- All internal blocks are enabled with `QI = TRUE`.
- The remote method call is a pure RPC trigger and does not require writing alternating values to a tag.
- The protocol and communication details are contained inside the composite subapplication, making it reusable across resources.

## State Overview

The subapplication does not contain an explicit ECC state machine. Its behavior is event-driven and dataflow-oriented.

Conceptually, the following states can be identified:

| State | Description |
|-------|-------------|
| Waiting for F1 release | The softkey block is armed and waits for `SK_RELEASED`. The background displays the current remote state. |
| Issuing remote call | On `SK_RELEASED`, the event is forwarded to `CLIENT_0`, which sends the OPC UA method call to device B. |
| Monitoring and visualization | `STATE_SUBSCRIBE` reads the remote BOOL state and updates `GreenWhiteBackground_AX`. |

No flip-flop state is stored locally on device A. The authoritative state remains on device B.

## Application Scenarios

- **Remote operator panel**: Device A acts as an operator interface for device B. Pressing F1 triggers a remote action on device B.
- **RPC-based toggle**: Device B contains a flip-flop that toggles as side effect of an OPC UA method call. Device A only sends the trigger and does not need to simulate a value change.
- **Visual state feedback**: The green/white background gives the operator immediate feedback about the remote flip-flop state.
- **Reusable protocol encapsulation**: The complete OPC UA trigger and state-read logic is bundled in a library composite, so the resource on device A remains simple and maintainable.

## Comparison with Similar Blocks

| Approach | Characteristics |
|----------|-----------------|
| Direct softkey to local output | Simple, but cannot trigger remote OPC UA methods. |
| Raw `CLIENT_0` in the resource | Requires manual event and data wiring in every resource. No integrated state visualization. |
| Value-change trigger with `AX_T_FF` | Requires an extra flip-flop or alternating write values on device A and can produce unnecessary network traffic. |
| This subapplication | Uses an explicit OPC UA `CALL_METHOD` trigger and an adapter-based state subscription. The complete protocol logic is encapsulated in the composite subapplication. |

## Conclusion

Softkey_Toggle_RPC_TO_Remote_BG_OPC provides a compact and reusable solution for remote OPC UA triggering with local visual feedback. By combining a softkey event source, a `CLIENT_0` RPC trigger, an adapter-based state subscription, and a green/white background display, it achieves a clean separation between HMI input, communication logic, and visualization. The design follows a "subapplication style" where the complexity of the remote protocol is hidden inside the composite, making it suitable for reusable library-based automation components.
