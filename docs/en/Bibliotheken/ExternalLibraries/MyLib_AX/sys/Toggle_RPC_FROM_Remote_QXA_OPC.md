# Toggle_RPC_FROM_Remote_QXA_OPC


![Toggle_RPC_FROM_Remote_QXA_OPC_network](./Toggle_RPC_FROM_Remote_QXA_OPC_network.svg)

![Toggle_RPC_FROM_Remote_QXA_OPC](./Toggle_RPC_FROM_Remote_QXA_OPC.svg)

* * * * * * * * * *

## Introduction

`Toggle_RPC_FROM_Remote_QXA_OPC` is a composite SubApplication designed for a distributed OPC UA control scenario. It runs on Device B and waits for a pure RPC trigger method call coming from Device A. Each received trigger toggles a real flip-flop logic block, updates a local digital output, and actively writes the new state back to Device A. The communication protocol is encapsulated in the composite itself, so the device resource does not need any protocol-specific wiring.

## Interface Structure

The SubApplication exposes no event inputs, no event outputs, no data outputs, and no adapter plugs/sockets. All external information enters through three data inputs.

### **Event Inputs**

None.

### **Event Outputs**

None.

### **Data Inputs**

| Name                | Type                            | Initial Value          | Description                                                                                     |
|---------------------|---------------------------------|------------------------|-------------------------------------------------------------------------------------------------|
| `Output`            | `logiBUS::io::DQ::logiBUS_DO_S` | `logiBUS_DO::Invalid`  | Selects the digital output channel, e.g. Q1..Q8, that shall be toggled.                         |
| `ID_TRIGGER_METHOD` | `WSTRING`                       |                        | Local method address for the argumentless RPC trigger. Device A calls this method using `CALL_METHOD`. |
| `ID_STATE_WRITE`    | `WSTRING`                       |                        | Remote address on Device A where the new flip-flop state is written back as a BOOL.             |

### **Data Outputs**

None.

### **Adapters**

No adapter sockets or plugs are exposed on the SubApplication boundary. Internally, adapter connections are used to forward the toggle event and state to the digital output and to the write-back client.

## Functionality

The SubApplication implements a remote toggle request/response cycle:

1. Device A invokes the configured OPC UA method.
2. `TRIGGER_SERVER`, an instance of `iec61499::net::SERVER_0`, receives the request and raises its `IND` event.
3. The `IND` event directly clocks the toggle flip-flop `AX_T_FF`.
4. The same `IND` event is immediately connected back to `TRIGGER_SERVER.RSP`, releasing the OPC UA server thread and avoiding blocking delays.
5. The flip-flop output `Q` is fed into `AX_SPLIT_2`, which duplicates the adapter event/data stream.
6. The first split output drives `DigitalOutput_Q1`, a `logiBUS::io::DQ::logiBUS_QXA` instance, to set or clear the selected physical digital output.
7. The second split output feeds `STATE_CLIENT`, an `adapter::net::AX_CLIENT_1_0` instance, which writes the new state back to Device A using the address given by `ID_STATE_WRITE`.

The result is a complete remote toggle operation: Device A triggers, Device B toggles, Device B updates its output, and Device A receives the new state.

## Technical Features

- Pure RPC trigger mechanism using `SERVER_0`; no value-change trick or intermediate bridge is required.
- `TRIGGER_SERVER.IND` is wired directly to `TRIGGER_SERVER.RSP`, preventing the OPC UA server thread from being held and avoiding project-wide communication delays.
- Real toggle logic is implemented with the `AX_T_FF` flip-flop adapter.
- `AX_SPLIT_2` distributes the same adapter event/data stream to two consumers.
- Digital output handling is done with `logiBUS_QXA`, supporting channel selection via the `Output` data input.
- Active state write-back is performed by `AX_CLIENT_1_0`.
- The whole protocol stack lives inside the `MyLib::sys` composite, keeping the device resource clean.

## State Overview

The SubApplication does not contain an explicit ECC state machine. Its stateful behavior is fully encapsulated in the internal `AX_T_FF` flip-flop:

- On each `CLK` event, the internal state toggles.
- The current state is available on the `Q` adapter output.
- The toggled state is then propagated to the digital output and to the write-back client.

Because the toggle logic is a true flip-flop, every remote method call changes the output state alternately between true and false.

## Application Scenarios

This SubApplication is useful in distributed automation systems where one controller must remotely toggle an output on another controller and keep both devices synchronized. A typical scenario is:

- Device A acts as the master or HMI/SCADA gateway.
- Device B is a remote station, e.g. Station 12 at IP `192.168.1.12`.
- Device A sends an OPC UA method call to Device B.
- Device B toggles its local digital output.
- Device B writes the new output state back to Device A for consistent visualization or further logic.

This pattern is especially valuable when no continuous cyclic data exchange is required and a simple event-driven RPC is preferred.

## Comparison with Similar Blocks

Compared to simpler or alternative implementations:

- **Plain `SERVER_0` without `RSP` wiring**: Can block the OPC UA server thread and delay unrelated OPC UA requests. This SubApplication avoids that by directly connecting `IND` to `RSP`.
- **Value-change trick**: Many implementations use a data value change instead of a real method call. This SubApplication uses a real RPC method trigger, which is cleaner and avoids unintended triggers.
- **Bridge-based communication**: Some architectures require a separate bridge block in the device resource. This SubApplication encapsulates the bridge/protocol logic inside the composite, so the resource remains simple and reusable.
- **FB with internal ECC**: A basic function block would implement the toggle in an ECC; this composite uses a dedicated `AX_T_FF` adapter, making the logic explicit and reusable.

## Conclusion

`Toggle_RPC_FROM_Remote_QXA_OPC` provides a robust, self-contained way to implement remote toggling over OPC UA. It combines a responsive RPC server, true flip-flop behavior, digital output control, and active state write-back in one composite SubApplication. The direct `IND` to `RSP` wiring ensures that the OPC UA stack remains responsive, while the clean separation of protocol and resource logic makes the SubApplication easy to reuse and maintain.
