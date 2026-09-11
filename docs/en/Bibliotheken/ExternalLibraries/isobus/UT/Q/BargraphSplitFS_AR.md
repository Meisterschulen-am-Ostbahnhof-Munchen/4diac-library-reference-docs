# BargraphSplitFS_AR

![BargraphSplitFS_AR](./BargraphSplitFS_AR.svg)

* * * * * * * * * *

## Introduction

`BargraphSplitFS_AR` is an adapter‑based wrapper around the function block `BargraphSplitFS`. Instead of requiring an explicit `REQ` event and a plain `rValue` data input, this block accepts the value through a unidirectional `AR` (adapter‑request) socket. This design simplifies integration in systems where the value is already provided via an adapter interface, such as those defined for ISO 11783‑6 (ISOBUS) applications. The wrapper internally maps the adapter’s event and data to the inner block’s `REQ` and `rValue` pins, and passes through the results (status codes and numerical values) to the external interface. Over‑range conditions on the left and right sides are re‑exposed as `AX` adapter plugs, following the same pattern as `PositionMarkerFSA`.

## Interface Structure

### **Event Inputs**

| Event   | Type   | Comment                                      | With Var |
|---------|--------|----------------------------------------------|----------|
| `INIT`  | `EInit` | Service Initialization                      | `stObj`  |

### **Event Outputs**

| Event  | Type    | Comment                                          | With Var                               |
|--------|---------|--------------------------------------------------|----------------------------------------|
| `INITO`| `EInit` | Initialization Confirm                          | –                                      |
| `CNF`  | `Event` | Confirmation of Requested Service               | `STATUSRight`, `s16resultRight`, `STATUSLeft`, `s16resultLeft` |

### **Data Inputs**

| Name   | Type                                    | Comment                                                                          |
|--------|-----------------------------------------|----------------------------------------------------------------------------------|
| `stObj`| `isobus::utils::bargraph::BargraphSplit_S` | Split‑bargraph object pool properties (left/right Bargraph refs, shared magnitude bounds), snapshotted at INIT |

### **Data Outputs**

| Name             | Type   | Comment                                          |
|------------------|--------|--------------------------------------------------|
| `STATUSRight`    | STRING | Service Status – passthrough from the internal BargraphSplitFS |
| `s16resultRight` | INT    | Return value – passthrough from the internal BargraphSplitFS |
| `STATUSLeft`     | STRING | Service Status – passthrough from the internal BargraphSplitFS |
| `s16resultLeft`  | INT    | Return value – passthrough from the internal BargraphSplitFS |

### **Adapters**

| Type       | Name         | Direction | Comment                                             |
|------------|--------------|-----------|-----------------------------------------------------|
| `AX`       | `xOverRight` | Plug      | rValue exceeded `stObj.r32MaxMagnitude`, right side clamped |
| `AX`       | `xOverLeft`  | Plug      | −rValue exceeded `stObj.r32MaxMagnitude`, left side clamped  |
| `AR`       | `rPhys`      | Socket    | Signed physical value input (adapter request)               |

## Functionality

The block is a pure wrapper that delegates all processing to an internal instance of `BargraphSplitFS`. Upon receiving an `INIT` event, the block snapshots the provided `stObj` structure and passes it to the inner block. When a value arrives through the `rPhys` adapter socket (its event `E1` and data `D1`), the internal block’s `REQ` and `rValue` inputs are activated and supplied accordingly. The inner block computes the two‑sided bar graph and issues a `CNF` event together with the status and result values for both the right and left sides. These outputs are directly forwarded to the parent block’s corresponding pins. Any over‑range conditions (`xOverRight`, `xOverLeft`) are also propagated via the adapter plugs, using the same event/data connection mechanism.

The internal network connections confirm this behaviour:

- `INIT` → `Inner.INIT`
- `rPhys.E1` → `Inner.REQ`
- `rPhys.D1` → `Inner.rValue`
- `Inner.INITO` → `INITO`
- `Inner.CNF` → `CNF`, `xOverRight.E1`, `xOverLeft.E1`
- Data: `stObj` → `Inner.stObj`; all four status/result outputs are directly mapped from the inner block; `Inner.xOverRight` → `xOverRight.D1` and `Inner.xOverLeft` → `xOverLeft.D1`.

## Technical Features

- **Adapter‑Based Input** – The signed physical value is supplied via a unidirectional `AR` socket, eliminating the need for a separate `REQ` event and `rValue` data pin.
- **Passthrough Outputs** – All status and return values of the internal block are directly forwarded without additional processing.
- **Over‑Range Indication** – Over‑range conditions are exposed as `AX` plugs in the same style as `PositionMarkerFSA`.
- **Single‑Shot Initialization** – The `INIT` event transfers the configuration structure `stObj` to the internal block.
- **Complete Encapsulation** – The wrapper hides the internal event/data protocol, presenting a clean adapter interface to the surrounding application.

## State Overview

The block itself does not maintain a state machine; it acts as a transparent bridge. Its operational behaviour is fully defined by the internal `BargraphSplitFS` block, which manages the actual split‑bargraph calculation. From the perspective of an external caller, the block is ready to accept a value via `rPhys` after initialization (`INITO` has been issued). The `CNF` event signals that the processing of the current request has finished and that the output data are valid.

## Application Scenarios

This block is suitable for ISOBUS (ISO 11783‑6) applications where a split bar graph must be displayed for a signed physical value, and where the value is already delivered through an adapter interface. Typical use cases include:

- Tractor and implement displays that receive sensor data via a CAN‑based adapter.
- Scenarios where the value source is reused across multiple logic parts and both left and right over‑range indicators need to be forwarded to other visualisation components.
- Systems that prefer a uniform adapter‑based interface for all value inputs, reducing wiring complexity and improving modularity.

## Comparison with Similar Blocks

The primary difference to the base block `BargraphSplitFS` is the input mechanism. `BargraphSplitFS` requires an explicit `REQ` event and a separate `rValue` data input, while `BargraphSplitFS_AR` encapsulates these into a single `AR` adapter socket. This makes the block more convenient in adapter‑centric architectures, but adds the overhead of a wrapper – the internal block still performs the same calculations.

Compared to `PositionMarkerFSA`, which wraps a position marker block in a similar fashion, this block follows the same design pattern: it uses `AR` for the value input and `AX` for the over‑range indications. The naming and structure are consistent, promoting a uniform look and feel across a suite of adapter‑wrapped ISOBUS widgets.

## Conclusion

`BargraphSplitFS_AR` provides a clean, adapter‑based interface to the `BargraphSplitFS` functionality. It simplifies integration in systems that use adapter sockets for value input, while preserving all the features and outputs of the underlying block. The block is well‑suited for ISOBUS applications that require a split bar graph with over‑range indication and prefer a modular, adapter‑oriented design.
