# Softkey_SRT_RPC_TO_Remote_BG_OPC


![Softkey_SRT_RPC_TO_Remote_BG_OPC_network](./Softkey_SRT_RPC_TO_Remote_BG_OPC_network.svg)

![Softkey_SRT_RPC_TO_Remote_BG_OPC](./Softkey_SRT_RPC_TO_Remote_BG_OPC.svg)

* * * * * * * * * *
## Introduction

The **Softkey_SRT_RPC_TO_Remote_BG_OPC** subapplication is a composite type designed for a two‑device OPC‑UA automation scenario. It runs on **Device A** (Station 11, 192.168.1.11) and controls three softkeys – **Set**, **Reset**, and **Toggle** – each of which triggers its own argument‑free, return‑value‑free OPC‑UA method call on a remote **Device B**. Instead of passing a string parameter to a single generic method, this design (Option A) uses three distinct remote method addresses, providing clearer semantics and easier maintenance. Additionally, a **GreenWhiteBackground** indication on the Toggle softkey reflects a flip‑flop state that is locally monitored on Device B and made available to Device A via an OPC‑UA variable subscription. The entire protocol logic is encapsulated inside the `MyLib::sys` composite, keeping the device resource simple and reusable.

## Interface Structure

The subapplication exposes only data inputs at its interface. All event handling and adapter logic are internal to the composite.

### **Event Inputs**
_None._ The subapplication does not expose any event inputs. Activation is performed internally by the softkey function blocks, which react to hardware events.

### **Event Outputs**
_None._ No events are propagated to the outside world.

### **Data Inputs**

| Name           | Type    | Description                                                                                     |
|----------------|---------|-------------------------------------------------------------------------------------------------|
| `u16ObjId_SET`   | `UINT`  | Object ID of the **Set** softkey. Initial value: `ID_NULL`.                                     |
| `u16ObjId_RESET` | `UINT`  | Object ID of the **Reset** softkey. Initial value: `ID_NULL`.                                   |
| `u16ObjId_TOGGLE`| `UINT`  | Object ID of the **Toggle** softkey (also carries the GreenWhiteBackground indication). Initial value: `ID_NULL`. |
| `ID_SET_CALL`    | `WSTRING` | Remote method address (`ACTION=CALL_METHOD`) for the **Set** method call on Device B.          |
| `ID_RESET_CALL`  | `WSTRING` | Remote method address (`ACTION=CALL_METHOD`) for the **Reset** method call on Device B.        |
| `ID_TOGGLE_CALL` | `WSTRING` | Remote method address (`ACTION=CALL_METHOD`) for the **Toggle** method call on Device B.       |
| `ID_STATE_READ`  | `WSTRING` | Locally monitored OPC‑UA address (`BOOL`, `ACTION=READ`) for the flip‑flop state that is remotely written by Device B. |

### **Data Outputs**
_None._ No data is returned to the caller.

### **Adapters**
_None at the interface level._ Internally, an `AX_SUBSCRIBE_1` adapter is used to establish the subscription for the state variable.

## Functionality

The subapplication implements the following end‑to‑end behavior:

1. **Softkey Detection** – Three `isobus::UT::io::Softkey::Softkey_IE` instances (`SoftKey_SET`, `SoftKey_RESET`, `SoftKey_TOGGLE`) are configured with `InputEvent = SK_RELEASED` (activation on key release) and `QI = TRUE`. Each instance is associated with a distinct Object ID supplied via the `u16ObjId_*` inputs.

2. **Remote Method Invocation** – When a softkey is pressed, the corresponding `IND` event is routed to the `REQ` input of a dedicated `iec61499::net::CLIENT_0` function block (`TRIGGER_SET_CLIENT`, `TRIGGER_RESET_CLIENT`, `TRIGGER_TOGGLE_CLIENT`). Each client is permanently enabled (`QI = TRUE`) and holds the remote method address in its `ID` input, which is wired from the corresponding `ID_*_CALL` data input.

3. **State Subscription** – An `adapter::net::AX_SUBSCRIBE_1` block (`STATE_SUBSCRIBE`) continuously subscribes to the OPC‑UA variable specified in `ID_STATE_READ`. Its output (an adapter interface) is connected to the `DI1` adapter socket of a `MyLib::sys::GreenWhiteBackground1_AX` subapplication.

4. **Visual Indication** – The `GreenWhiteBackground1_AX` subapplication, fed with `u16ObjId_TOGGLE` and the subscribed state value, renders either a green or a white background on the Toggle softkey, reflecting the current flip‑flop state maintained on Device B.

## Technical Features

- **Fully composite design** – The protocol logic is contained within a `MyLib::sys` composite, keeping the device resource lean and making the subapplication portable across different resources or devices.
- **Dedicated OPC‑UA method clients** – Three separate `CLIENT_0` instances, one per softkey, allow independent configuration and fault isolation for each remote call.
- **No parameter passing** – The remote methods are intentionally argument‑free, simplifying the OPC‑UA server implementation on Device B and avoiding string‑parsing overhead.
- **Event‑driven activation** – Softkey events are captured on release (`SK_RELEASED`), providing deterministic triggering.
- **Subscription‑based state feedback** – The flip‑flop state is obtained via OPC‑UA subscription (not polling), ensuring timely and bandwidth‑efficient updates.
- **Configurable identifiers** – Object IDs and remote addresses are injected at instantiation time, allowing the same subapplication to be reused with different hardware layouts or OPC‑UA server configurations.
- **EPL‑2.0 licensing** – The subapplication is published under the Eclipse Public License 2.0.

## State Overview

The subapplication itself does not contain an explicit state machine. However, its constituent elements exhibit the following state‑oriented behavior:

- **Softkey FBs** – Each `Softkey_IE` is in an *idle* state until a hardware‑level key‑release event occurs. Upon detection, the FB generates an `IND` event and returns to the idle state.
- **Client FBs** – Each `CLIENT_0` transitions through *request sent* → *response received* → *idle* for every triggered call. With `QI = TRUE`, the clients remain operationally ready.
- **State variable (remote)** – The flip‑flop state on Device B is a single BOOL (binary) value. It transitions between `0` (white background) and `1` (green background) each time the Toggle method is invoked. The `STATE_SUBSCRIBE` adapter continuously tracks changes and forwards them to the background rendering component.

The overall system thus implements a classic **set / reset / toggle** control paradigm with visual feedback, where the state source of truth resides on Device B.

## Application Scenarios

This subapplication is well suited for:

- **Industrial control panels** – Physical or virtual softkeys on a local operator panel (Device A) that remotely operate actuators, valves, or machine functions on a PLC or controller (Device B).
- **Multi‑device OPC‑UA architectures** – Where one device needs to invoke discrete, side‑effect‑free methods on another device, and the current state must be displayed locally.
- **Toggle‑with‑feedback user interfaces** – For example, enabling/disabling a production line parameter, where the background color of the key gives the operator an immediate visual confirmation of the actual remote state.
- **Modular and portable HMI components** – Because all protocol logic is embedded in the composite, the same subapplication can be dropped into different 4diac projects without modifying the resource.

## Comparison with Similar Blocks

| Feature / Aspect                     | **Softkey_SRT_RPC_TO_Remote_BG_OPC** (Option A)              | Alternative: Single‑Method‑With‑String‑Parameter (Option B)        | Generic OPC‑UA Client FB (non‑dedicated)               |
|--------------------------------------|--------------------------------------------------------------|--------------------------------------------------------------------|--------------------------------------------------------|
| **Remote method granularity**        | Three dedicated methods (Set / Reset / Toggle)               | One method that accepts a string parameter to select the action    | User must supply address and parameters per call        |
| **Parameter handling**               | No arguments; minimal server complexity                      | Server must parse the string, incurring overhead and error risk    | Flexible but requires manual mapping                    |
| **State feedback**                   | Built‑in subscription and GreenWhiteBackground visualization | Typically requires an additional subscription block                | Not included – must be added externally                 |
| **Error isolation**                  | High – failure of one method call does not affect the others | Lower – a parsing error in the string affects all calls            | Depends on configuration, often shared                  |
| **Configuration effort**             | Moderate – three addresses must be provided                  | Low – only one address, but the string must be constructed         | High – full OPC‑UA node configuration required          |
| **Clarity / maintainability**        | Excellent – each call is explicit and self‑documenting       | Acceptable – but string contracts are error‑prone                  | Depends on the engineer’s discipline                    |
| **Suitability for industrial HMI**   | Very high – designed for operator panels with visual feedback | Medium – more suited for ad‑hoc remote calls                        | Medium – versatile but not tailored to softkey use cases |

## Conclusion

The **Softkey_SRT_RPC_TO_Remote_BG_OPC** subapplication provides a clean, reusable, and industrially oriented solution for controlling three softkey‑driven remote method invocations over OPC‑UA, combined with live state visualization. Its dedicated‑method approach (Option A) eliminates string‑parsing pitfalls and improves failure isolation, while the subscription‑based GreenWhiteBackground feedback gives operators immediate and reliable confirmation of the remote flip‑flop state. By encapsulating all protocol details within a `MyLib::sys` composite, the subapplication promotes modularity, portability, and maintainability across different 4diac projects. This makes it an excellent building block for modern HMI and SCADA‑style applications built on the Eclipse 4diac framework.