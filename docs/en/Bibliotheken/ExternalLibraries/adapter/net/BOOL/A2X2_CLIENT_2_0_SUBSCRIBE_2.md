# A2X2_CLIENT_2_0_SUBSCRIBE_2

![A2X2_CLIENT_2_0_SUBSCRIBE_2](./A2X2_CLIENT_2_0_SUBSCRIBE_2.svg)

* * * * * * * * * *

## Introduction

The function block **A2X2_CLIENT_2_0_SUBSCRIBE_2** is a composite FB that bridges between a bidirectional A2X2 adapter (with two BOOL channels, UP and DOWN) and an OPC-UA server. It uses the standard `CLIENT_2_0` and `SUBSCRIBE_2` function blocks to write two BOOL values to a remote OPC-UA node and read two BOOL values from a local state node. Each input/output BOOL is individually buffered using `E_D_FF` flip-flops to ensure stable signal transfer and to decouple event sequences.

## Interface Structure

### **Event Inputs**

| Event    | Type   | Comment                                     |
|----------|--------|---------------------------------------------|
| `INIT`   | EInit  | Initialization event. Resets the FB and starts the data flow. |

### **Event Outputs**

| Event   | Type   | Comment                                                                  |
|---------|--------|--------------------------------------------------------------------------|
| `INITO` | EInit  | Initialization confirm. Emitted after both internal FB initializations complete. |
| `CNF`   | Event  | Confirmation event. Emitted when `QO`, `STATUS_WRITE`, and `STATUS_READ` are updated. |

### **Data Inputs**

| Name      | Type    | Comment                                                               |
|-----------|---------|-----------------------------------------------------------------------|
| `QI`      | BOOL    | Enable input. When `FALSE`, the FB is inactive and outputs are reset. |
| `ID_WRITE`| WSTRING | Remote target address for the write operation (both BOOLs).           |
| `ID_READ` | WSTRING | Local state node address to monitor (both BOOLs).                     |

### **Data Outputs**

| Name           | Type    | Comment                                                                  |
|----------------|---------|--------------------------------------------------------------------------|
| `QO`           | BOOL    | `TRUE` only when both the write client and the read subscriber report `QO = TRUE`. |
| `STATUS_WRITE` | WSTRING | Status string from the internal `WRITE_CLIENT` FB.                       |
| `STATUS_READ`  | WSTRING | Status string from the internal `READ_SUBSCRIBE` FB.                     |

### **Adapters**

| Adapter | Type                               | Comment                                                                 |
|---------|------------------------------------|-------------------------------------------------------------------------|
| `IO`    | `adapter::types::bidirectional::A2X2` | Bidirectional BOOL channels: `DO_UP`/`DO_DOWN` (send) and `DI_UP`/`DI_DOWN` (receive). |

## Functionality

The FB operates in two independent data paths:

1. **Write Path**: BOOL values received from the adapter (`IO.DO_UP`, `IO.DO_DOWN`) are captured in the corresponding `E_D_FF_TX_UP` and `E_D_FF_TX_DOWN` flip-flops. Each event at `IO.EO_UP` or `IO.EO_DOWN` triggers a `REQ` on the `WRITE_CLIENT` FB, which sends the buffered values to the remote OPC-UA node via `SD_1` and `SD_2`.

2. **Read Path**: The `READ_SUBSCRIBE` FB continuously monitors the configured `ID_READ` node. Upon each `IND` event, its `RD_1` and `RD_2` data outputs are fed into the `E_D_FF_RX_UP` and `E_D_FF_RX_DOWN` flip-flops. Their stored values are then presented on `IO.DI_UP` and `IO.DI_DOWN` and emitted via the corresponding event outputs (`IO.EI_UP`, `IO.EI_DOWN`).

The internal `AND_QO` block combines the `QO` signals from both the write client and the read subscriber, producing the overall `QO` output. The `CNF` event is generated whenever either the write client finishes a write operation (`WRITE_CLIENT.CNF`) or the read subscriber receives new data (`READ_SUBSCRIBE.IND`), ensuring that the status outputs are always current.

## Technical Features

- Uses standard IEC 61499 `CLIENT_2_0` and `SUBSCRIBE_2` blocks for OPC-UA communication.
- Four `E_D_FF` flip-flops provide edge‑triggered buffering, preventing data loss when events occur faster than the communication cycle.
- Initialization is chained: `INIT` → `READ_SUBSCRIBE.INIT` → `WRITE_CLIENT.INIT` → `INITO`.
- The output `QO` is only `TRUE` when both write and read sides are operational, giving a combined health indicator.
- Status strings are propagated separately for write and read operations, aiding diagnostics.

## State Overview

The FB does not maintain a complex internal state machine. Its behaviour is driven by the internal FB instances:

- **`READ_SUBSCRIBE`** operates in the standard subscribe‑mode, generating `IND` events whenever subscribed data changes.
- **`WRITE_CLIENT`** operates in client mode, generating `REQ` events to send data and `CNF` events to confirm completion.
- The `E_D_FF` flip‑flops act as single‑bit storage elements, toggling their `Q` outputs on each clock edge from the corresponding event inputs.
- The `AND_QO` block simply performs a logical AND of the two `QO` signals.

Overall, the FB is in an active state only when `QI = TRUE`. When `QI` is `FALSE`, all internal blocks are disabled and outputs become `FALSE`/empty strings.

## Application Scenarios

This FB is suitable for applications requiring:

- Reliable bidirectional exchange of two BOOL values between an industrial controller and an OPC-UA server.
- Separation of write and read data paths with individual status reporting.
- Edge-triggered buffering to handle high-frequency updates without loss.
- Use cases where an A2X2 adapter is present, such as connecting to remote I/O, interlocking logic, or display of single‑bit state information.

## Comparison with Similar Blocks

- **A2X2_CLIENT_2_0_SUBSCRIBE_1** (hypothetical) might handle only one BOOL channel; this FB processes two channels (UP and DOWN) with independent buffering.
- Using two separate `CLIENT_2_0` and `SUBSCRIBE_2` blocks for each channel would double the network connections and complicate status aggregation; this FB encapsulates both in one unit.
- Compared to a simple direct coupling without flip‑flops, the `E_D_FF` buffers prevent race conditions and ensure that only stable, latched values are transmitted or displayed.

## Conclusion

**A2X2_CLIENT_2_0_SUBSCRIBE_2** provides a compact, reliable solution for bidirectional BOOL exchange with an OPC-UA server. It combines standard communication blocks with local buffering, yielding clear status outputs and a robust initialization sequence. Its interface is tailored to the A2X2 adapter, making it a ready‑to‑use component in distributed automation systems.
