# SystemTick


![SystemTick_network](./SystemTick_network.svg)

![SystemTick](./SystemTick.svg)

* * * * * * * * * *
## Introduction

The **SystemTick** subapplication is a free-running tick counter designed to serve as a heartbeat indicator for industrial control systems. It independently increments a counter value every 200 milliseconds, cycling back to zero after reaching 100 (i.e., a full cycle lasts 20 seconds). The current counter value is provided via an adapter output, making it easy to monitor the health of the system that hosts this subapplication. As long as the output value keeps changing, the system is alive — regardless of whether the value is displayed locally or distributed via OPC-UA to other modules.

## Interface Structure

This subapplication provides a single adapter output and no direct event or data interfaces. All internal logic is encapsulated within the embedded function block network.

### **Event Inputs**

None.

### **Event Outputs**

None.

### **Data Inputs**

None.

### **Data Outputs**

None.

### **Adapters**

| Name | Type | Description |
|------|------|-------------|
| `ADI_OUT` | `adapter::types::unidirectional::ADI` | Current tick counter value (0–99), updated every 200 ms. |

## Functionality

The subapplication implements a cyclic counter using the following internal function blocks:

- **INIT** – Performs initialization and starts the cycle generation.
- **E_CYCLE** – Generates a periodic event every 200 ms (configured via its `DT` parameter).
- **ADD_2** – Adds 1 to the current counter value on each cycle.
- **F_MOVE** – Stores the current counter value and forwards it to the arithmetic operations.
- **F_MOD** – Applies modulo 100 when returning the visible value to ensure the range 0–99.
- **ADI_DINT_TO_DI** – Converts the internal DINT counter value into the `ADI` adapter format for the `ADI_OUT` plug.

**Working sequence:**

1. The `INIT` function block starts `E_CYCLE` after initialization.
2. Each cycle, `E_CYCLE` triggers `ADD_2`, which increments the counter by 1.
3. The incremented value is stored in `F_MOVE` and also fed to `F_MOD` for modulo 100.
4. `F_MOD` outputs a value between 0 and 99 and passes it to `ADI_DINT_TO_DI`.
5. `ADI_DINT_TO_DI` packages the value and sends it via the `ADI_OUT` adapter output.

## Technical Features

- **Periodicity:** 200 ms (configurable via the `DT` parameter of the `E_CYCLE` block).
- **Value range:** 0 to 99 (modulo 100).
- **Cycle duration:** 20 seconds for a complete wrap-around.
- **Output type:** Unidirectional `ADI` adapter, carrying one event and one 32-bit integer (DINT).
- **Initialization:** Automatic start via the `INIT` block; no external trigger required.
- **Encapsulation:** All logic is self-contained; no direct I/O or interface connections except the adapter output.

## State Overview

The subapplication does not expose an explicit state machine to the outside. Internally, the counter value is held in the `F_MOVE`/`ADD_2` loop, which behaves as a simple infinite cycle:

1. **Startup:** `INIT` runs and starts `E_CYCLE`.
2. **Counting:** Each cycle increments the counter by 1.
3. **Wrap:** When the value reaches 100, `F_MOD` resets it to 0.
4. **Output:** The current value is continuously reflected on `ADI_OUT`.

## Application Scenarios

- **Heartbeat monitoring:** Use the periodic updating counter as a “system alive” signal in distributed control systems.
- **Operational indication:** Display the tick value on a visualization (HMI) to show that the underlying logic is still executing.
- **OPC-UA distribution:** Feed the adapter output to an OPC-UA server (e.g., a gateway) to make the heartbeat available to remote clients.
- **Watchdog functionality:** Detect loss of signal when the output stops changing, indicating a malfunction or offline state.

## Comparison with Similar Blocks

- **SystemTick vs. simple E_CYCLE with counter:** Unlike a raw periodic event, SystemTick provides a standardized adapter output that encapsulates both an event and a value, making it easier to reuse and integrate with other adapters.
- **SystemTick vs. PLC timer blocks:** Traditional IEC 61131 timers require explicit reset and hold logic; SystemTick runs autonomously and includes modulo scaling.
- **SystemTick vs. heartbeat FBs with fixed periods:** The period is adjustable (via `DT`) and the value range can be easily modified by changing the modulo constant, offering flexibility.

## Conclusion

The **SystemTick** subapplication is a compact, self-contained heartbeat generator well suited for monitoring the health of automation systems. Its simple interface (single adapter output) and clear internal structure make it easy to integrate into larger 4diac projects. The periodic, bounded counter provides a reliable liveliness signal that can be consumed locally or remotely, fulfilling a fundamental requirement in distributed control environments.