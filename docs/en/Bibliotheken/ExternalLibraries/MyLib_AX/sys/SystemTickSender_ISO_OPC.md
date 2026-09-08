# SystemTickSender_ISO_OPC

![SystemTickSender_ISO_OPC_network](./SystemTickSender_ISO_OPC_network.svg)

* * * * * * * * * *

## Introduction

`SystemTickSender_ISO_OPC` combines [`SystemTickSender_ISO`](./SystemTickSender_ISO.md) (display on a local VT number field) with [`SystemTickSender_OPC`](./SystemTickSender_OPC.md) (remote publishing via OPC UA) so that the same status indicator is visible both on the local screen and can be monitored by other modules via remote subscribe.


## Function Blocks (FBs) Used

### Sub-Blocks: SystemTickSender_ISO_OPC

- **Type**: SubAppType
- **Internal FBs Used**:

- **System_Tick** (SubApp, `MyLib::sys`): Returns the counter reading as a `ADI` adapter (DINT).

- **ADI_SPLIT_2**: `adapter::events::unidirectional::ADI_SPLIT_2` — Fans `System_Tick.ADI_OUT` out to two destinations, as an adapter socket can only accept one source: one output feeds the VT display (via `ADI_TO_AUDI`), the other feeds `ADI_PUBLISH_1` for OPC UA publishing.

- **ADI_TO_AUDI**: `adapter::conversion::unidirectional::ADI_TO_AUDI` — Converts DINT to UDINT for VT display.

- **Q_NumericValue_AUDI**: `isobus::UT::Q::Q_NumericValue_AUDI` — displays the value locally on the VT (`u16ObjId`).

- **ADI_PUBLISH_1**: `adapter::net::ADI_PUBLISH_1` (`QI=TRUE`) — publishes the same counter value via OPC UA (`ID_WRITE`).

- **Functionality**: The counter value from `System_Tick` is branched via `ADI_SPLIT_2`: one branch goes to the VT display via `ADI_TO_AUDI`, as with `SystemTickSender_ISO`, the other goes directly to `ADI_PUBLISH_1` for OPC UA publishing — as with `SystemTickSender_OPC`.

## Program Flow and Connections

1. `System_Tick.ADI_OUT` → `ADI_SPLIT_2.IN`.


2. `ADI_SPLIT_2.OUT1` → `ADI_TO_AUDI.ADI_IN` → `ADI_TO_AUDI.AUDI_OUT` → `Q_NumericValue_AUDI.u32NewValue` (VT display).

3. `ADI_SPLIT_2.OUT2` → `ADI_PUBLISH_1.IN` (OPC UA publish).

4. Parameters: `u16ObjId` → `Q_NumericValue_AUDI.u16ObjId`; `ID_WRITE` → `ADI_PUBLISH_1.ID`.


## Technical Features

- **One counter, two uses**: `ADI_SPLIT_2` allows the parallel use of the same `System_Tick` counter for VT display and OPC UA publishing without duplicating the counter.

## Application Scenarios

- Modules with their own VT connection whose heartbeat should also be monitored by other modules via remote subscribe.

## Comparison with similar function blocks

If only one of the two outputs is needed, the simpler [`SystemTickSender_ISO`](./SystemTickSender_ISO.md) (VT only) or [`SystemTickSender_OPC`](./SystemTickSender_OPC.md) (OPC UA only) should be used. This pattern (Split → VT Display + OPC UA Publish) is structurally equivalent to the older [`SystemTickSender`](./SystemTickSender.md).

## Summary

`SystemTickSender_ISO_OPC` displays the SystemTick Heartbeat simultaneously locally on the VT and remotely via OPC UA by combining the two leaner individual variants into a single component.

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Color Reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
