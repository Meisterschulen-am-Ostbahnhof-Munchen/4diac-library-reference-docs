# ILOCK_SWITCH_2_QXA_OPC


![ILOCK_SWITCH_2_QXA_OPC_network](./ILOCK_SWITCH_2_QXA_OPC_network.svg)

![ILOCK_SWITCH_2_QXA_OPC](./ILOCK_SWITCH_2_QXA_OPC.svg)

* * * * * * * * * *

## Introduction

The ILOCK_SWITCH_2_QXA_OPC subapplication implements a 2-direction double-acting digital output with a "last-wins" interlock and configurable protection time. For each direction, the real function command (remote subscribe) is merged with the existing IO-test command (remote subscribe) using an AX_OR_2 block before entering the interlock logic. This keeps the IO-test fully usable while preventing both directions from being permanently switched simultaneously. The interlocked output drives the physical logiBUS channel and reports its actual state both to the existing IO-test monitoring and to the real function feedback (e.g., GreenWhiteBackground on a SoftKey). It is generic for any "digital DW" actuator operated from a module without its own visualization terminal.

## Interface Structure

### **Event Inputs**

None.

### **Event Outputs**

None.

### **Data Inputs**

- **Output_UP** (logiBUS::io::DQ::logiBUS_DO_S): Physical output channel for the UP/Left direction. Initial value: `logiBUS_DO::Invalid`.
- **Output_DOWN** (logiBUS::io::DQ::logiBUS_DO_S): Physical output channel for the DOWN/Right direction. Initial value: `logiBUS_DO::Invalid`.
- **DT_PROTECT** (TIME): Protection dead time before a direction change is allowed. Default: `T#300ms`.
- **ID_TEST_READ_UP** (WSTRING): Existing IO-test subscribe address for the UP direction (e.g., `STG3_Q0x_READ`).
- **ID_TEST_WRITE_UP** (WSTRING): Existing IO-test publish address for the UP direction (e.g., `STG3_Q0x_WRITE`).
- **ID_TEST_READ_DOWN** (WSTRING): Existing IO-test subscribe address for the DOWN direction.
- **ID_TEST_WRITE_DOWN** (WSTRING): Existing IO-test publish address for the DOWN direction.
- **ID_READ_UP** (WSTRING): Real function command subscribe address for the UP direction.
- **ID_WRITE_UP** (WSTRING): Real function feedback publish address for the UP direction.
- **ID_READ_DOWN** (WSTRING): Real function command subscribe address for the DOWN direction.
- **ID_WRITE_DOWN** (WSTRING): Real function feedback publish address for the DOWN direction.

### **Data Outputs**

None.

### **Adapters**

None.

## Functionality

For each direction (UP and DOWN), the subapplication subscribes to two independent remote command sources via `AX_SUBSCRIBE_1` blocks:

1. The existing IO-test command (`ID_TEST_READ_*`).
2. The real function command (`ID_READ_*`).

Both signals are combined using an `AX_OR_2` block, so that either the IO-test command or the real function command can request the activation of that direction. The combined requests are then passed to an `ILOCK_SWITCH_PROTECT_AX` interlock block, which enforces a "last-wins" policy and a configurable protection time (`DT_PROTECT`). This ensures that both directions can never be active simultaneously and that a minimum dead time elapses before switching directions.

The interlocked output for each direction is distributed via an `AX_SPLIT_3` block into three paths:

- **Physical output**: Drives the logiBUS digital output channel (`DigitalOutput_UP` / `DigitalOutput_DOWN`).
- **IO-test feedback**: Publishes the actual state to the existing IO-test publish address (`ID_TEST_WRITE_*`).
- **Function feedback**: Publishes the actual state to the real function feedback address (`ID_WRITE_*`, e.g., for SoftKey background indication).

The `E_TimeOut` block is connected to the interlock's timeout event to trigger and manage the protection timer logic.

## Technical Features

- Merging of IO-test and real function commands using `AX_OR_2` logic blocks.
- Last-wins interlocking via the `ILOCK_SWITCH_PROTECT_AX` block.
- Configurable protection dead time (`DT_PROTECT`), defaulting to 300 ms.
- Remote subscribe (`AX_SUBSCRIBE_1`) and publish (`AX_PUBLISH_1`) capabilities for integration into distributed control networks.
- Signal fan-out via `AX_SPLIT_3` to drive the physical output, IO-test monitoring, and function feedback simultaneously.
- Fully generic design for any "digital DW" actuator, with all addresses parameterized via WSTRING inputs.
- No event inputs/outputs at the interface level; all logic is driven internally by the subscribed data.

## State Overview

The subapplication relies on the internal state machine of the `ILOCK_SWITCH_PROTECT_AX` interlock block. The relevant operating states are:

- **Idle**: No direction is requested; both outputs are inactive.
- **UP active**: The UP direction is interlocked and active; the DOWN direction is blocked.
- **DOWN active**: The DOWN direction is interlocked and active; the UP direction is blocked.
- **Switching with protection time**: After a direction change request, the protection timer (`DT_PROTECT`) runs before the new direction is applied, preventing rapid toggling.

## Application Scenarios

- Controlling a double-acting digital actuator (e.g., a motorized valve or damper) from a control module without a local visualization terminal.
- Integrating existing IO-test functionality with real remote function commands, ensuring that both can drive the output while maintaining interlock safety.
- Providing feedback to both the legacy IO-test monitoring system and the operator interface (e.g., SoftKey background color) with the actual interlocked output state.
- Generic use in any "digital DW" actuator system where two directional commands must be merged and interlocked.

## Comparison with Similar Blocks

- Compared to a simple `logiBUS_QXA` direct output, this subapplication adds interlock logic and protection timing.
- Compared to an interlock block that only accepts single-command inputs, this block explicitly merges remote subscribe/publish channels for both IO-test and real function commands.
- The use of `AX_OR_2` before the interlock distinguishes it from pure interlock variants, enabling both test and functional commands to share the same interlock protection path.

## Conclusion

ILOCK_SWITCH_2_QXA_OPC provides a robust and generic solution for driving a double-acting digital output with interlocking, protection time, and merged IO-test/function command handling. It preserves the existing IO-test infrastructure while guaranteeing safe operation and consistent feedback through both test and functional channels, making it suitable for a wide range of actuator control applications.
