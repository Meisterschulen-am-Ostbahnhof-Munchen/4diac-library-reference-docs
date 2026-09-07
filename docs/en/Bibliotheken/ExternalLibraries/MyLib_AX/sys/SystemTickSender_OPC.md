# SystemTickSender_OPC

![SystemTickSender_OPC_network](./SystemTickSender_OPC_network.svg)

* * * * * * * * * *

## Introduction

`SystemTickSender_OPC` is the most streamlined SystemTick variant for modules without their own VT connection: The running tick counter from [`System_Tick`](./System_Tick.md) is published directly via OPC UA, without a local VT display. Another module with VT can subscribe to and display this value via remote subscribe to monitor the module's activity.


## Function Blocks (FBs) Used

### Sub-Blocks: SystemTickSender_OPC

- **Type**: SubAppType
- **Internal FBs Used**:

- **System_Tick** (SubApp, `MyLib::sys`): returns the counter reading as a `ADI` adapter (DINT).

- **ADI_PUBLISH_1**: `adapter::net::ADI_PUBLISH_1` (`QI=TRUE`) — publishes the counter reading directly via OPC UA (`ID_WRITE`).

- **Functionality**: The counter reading from `System_Tick` is passed directly to `ADI_PUBLISH_1` without conversion.


## Program Flow and Connections

1. `ID_WRITE` → `ADI_PUBLISH_1.ID` (Data connection, hidden).

2. `System_Tick.ADI_OUT` → `ADI_PUBLISH_1.IN`.

## Technical Features

- **No type conversion required**: Since no VT display is involved, the `ADI_TO_AUDI` conversion is omitted—the raw DINT value is published directly.

## Application Scenarios

- Modules without their own VT connection (e.g., pure I/O/field modules) whose liveness is to be monitored by another module with VT.


## Comparison with Similar Modules

If a local VT display is also required, use [`SystemTickSender_ISO_OPC`](./SystemTickSender_ISO_OPC.md) (VT + OPC UA) or [`SystemTickSender_ISO`](./SystemTickSender_ISO.md) (VT only).

## Summary

`SystemTickSender_OPC` publishes the SystemTick heartbeat exclusively via OPC UA — the minimal option for modules without their own VT connection.

---

### 🌐 Related Topic Subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Color Reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
