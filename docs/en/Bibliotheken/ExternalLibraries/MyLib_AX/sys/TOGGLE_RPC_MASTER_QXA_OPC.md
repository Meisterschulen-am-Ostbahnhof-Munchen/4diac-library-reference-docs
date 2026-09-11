# TOGGLE_RPC_MASTER_QXA_OPC


![TOGGLE_RPC_MASTER_QXA_OPC_network](./TOGGLE_RPC_MASTER_QXA_OPC_network.svg)

![TOGGLE_RPC_MASTER_QXA_OPC](./TOGGLE_RPC_MASTER_QXA_OPC.svg)

* * * * * * * * * *
## Introduction
The `TOGGLE_RPC_MASTER_QXA_OPC` is a subapplication that implements a remote-controlled toggle "head" for a group of up to six simultaneously switched channels without having its own physical output. It is intended for OPC UA based operator panels: a remote client triggers a toggle operation by calling a local OPC UA method, and the resulting state is distributed through six adapter plugs to sibling subapplications. The same state is also published locally so that an operator panel can display it, for example as a background colour.

This subapplication is designed for multi-channel use cases where one softkey should toggle several outputs at the same time, but each physical channel may still have its own delayed start behaviour.

## Interface Structure
The subapplication exposes two WSTRING data inputs and six unidirectional AX adapter plugs. It has no explicit event inputs, event outputs, data outputs, or sockets.

### **Event Inputs**
None.

### **Event Outputs**
None.

### **Data Inputs**
| Name | Type | Description |
|---|---|---|
| `ID_TRIGGER_METHOD` | WSTRING | Local OPC UA method address (`ACTION=CREATE_METHOD`) used as the argumentless toggle trigger. It is called remotely by the operator panel via `CALL_METHOD`. |
| `ID_STATE_WRITE` | WSTRING | Local OPC UA publish address (`ACTION=WRITE`) for the actual toggle state. It can be subscribed by remote operator panels, e.g. for a GreenWhite background indication. |

### **Data Outputs**
None.

### **Adapters**
| Name | Type | Direction / Kind | Description |
|---|---|---|---|
| `OUT1` | `adapter::types::unidirectional::AX` | Plug | Toggle state for channel 1, typically connected to an `AX_TON_MERGE_QXA_OPC.MASTER`. |
| `OUT2` | `adapter::types::unidirectional::AX` | Plug | Toggle state for channel 2. |
| `OUT3` | `adapter::types::unidirectional::AX` | Plug | Toggle state for channel 3. |
| `OUT4` | `adapter::types::unidirectional::AX` | Plug | Toggle state for channel 4. |
| `OUT5` | `adapter::types::unidirectional::AX` | Plug | Toggle state for channel 5. |
| `OUT6` | `adapter::types::unidirectional::AX` | Plug | Toggle state for channel 6. |

Unused `OUTn` plugs can simply remain unconnected.

## Functionality
The subapplication provides a click-toggle behaviour:

1. The internal OPC UA server FB `TRIGGER_SERVER` receives an incoming method call or event at its `IND` input.
2. The same event clocks the internal `AX_T_FF` flip-flop.
3. The flip-flop output `Q` toggles between `FALSE` and `TRUE` on every trigger.
4. The state is forwarded to the `AX_SPLIT_7` block, which distributes it to seven branches.
5. Six of the branches are connected to the `OUT1` .. `OUT6` adapter plugs and can be wired to sibling subapplications.
6. The seventh branch is connected to `PUBLISH_STATE`, which publishes the state under the configured OPC UA address `ID_STATE_WRITE`.
7. `TRIGGER_SERVER.IND` is directly connected to `TRIGGER_SERVER.RSP`, ensuring that the OPC UA server response is sent immediately and that the server thread is not blocked.

This allows the toggle state to be used both locally in the automation network and remotely for HMI display purposes.

## Technical Features
- Uses `iec61499::net::SERVER_0` as the OPC UA method and event server.
- Uses `adapter::events::unidirectional::AX_T_FF` as the central toggle flip-flop.
- Uses `adapter::events::unidirectional::AX_SPLIT_7` to distribute one toggle state to seven outputs.
- Uses `adapter::net::AX_PUBLISH_1` to publish the current toggle state to OPC UA.
- `TRIGGER_SERVER.IND` is wired directly to `TRIGGER_SERVER.RSP` to avoid blocking the OPC UA server thread.
- Supports up to six 1:1 adapter connections to sibling subapplications.
- The data input connections are marked as not visible in the internal network to keep the design compact.
- Licensed under the Eclipse Public License 2.0.

## State Overview
The central state is the boolean output `Q` of the internal `AX_T_FF` block. It toggles on every rising edge of the trigger event from `TRIGGER_SERVER.IND`.

| State | Meaning |
|---|---|
| `FALSE` | Toggle state is off. All connected `OUTn` adapter values and the published OPC UA state are `FALSE`. |
| `TRUE` | Toggle state is on. All connected `OUTn` adapter values and the published OPC UA state are `TRUE`. |
| Transition | A trigger event is received, `Q` is inverted, and the new state is distributed to all branches. |

There is no additional internal state machine; the behaviour is that of a simple T flip-flop.

## Application Scenarios
- **Lighting groups**: A bank of six spotlights can be switched with a single softkey while each channel starts with its own delay to avoid capacitive inrush current peaks.
- **Multi-channel remote control**: An operator panel calls one OPC UA method to toggle a group of drives, valves, or light channels.
- **HMI background indication**: The published state is subscribed by the operator panel and used for colour feedback, e.g. Green/White background.
- **Distributed systems**: The `OUTn` plugs can be connected to sibling subapplications such as `AX_TON_MERGE_QXA_OPC`, one per physical channel, allowing shared toggle control with individual channel timing.

## Comparison with Similar Blocks
- `TOGGLE_RPC_MASTER_QXA_OPC` has no physical output of its own. Instead, it provides six adapter plugs to distribute the toggle state to multiple sibling subapplications.
- `TOGGLE_RPC_MERGE_QXA_OPC` is the single-output variant. It includes the physical output path and is recommended when only one output is required.
- Other softkey/RPC toggle blocks may support remote background toggling, but they do not provide the same multi-channel fan-out through six separate plugs.

## Conclusion
`TOGGLE_RPC_MASTER_QXA_OPC` is a flexible solution for remote click-toggle control of up to six channels via OPC UA. It combines a method-triggered flip-flop, local adapter-based fan-out, and OPC UA state publication. The direct `IND` to `RSP` connection keeps the OPC UA server responsive, and unused outputs can be left unconnected, making the subapplication suitable for both small and medium-sized multi-channel HMI-controlled applications.