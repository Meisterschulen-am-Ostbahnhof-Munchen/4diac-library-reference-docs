# INI_OPC_PARAM_ATM

![INI_OPC_PARAM_ATM_network](./INI_OPC_PARAM_ATM_network.svg)

* * * * * * * * * *

## Introduction

`INI_OPC_PARAM_ATM` is the time parameter variant of [`INI_OPC_PARAM_AR`](./INI_OPC_PARAM_AR.md): The persistently stored REAL value is additionally converted into a `ATM` adapter (`TIME`) — `DEFAULT_VALUE` and the stored value are interpreted as seconds (`T#1s * Sekunden`).



## Function Blocks (FBs) Used

### Sub-Blocks: INI_OPC_PARAM_ATM

- **Type**: SubAppType
- **Internal FBs Used**:

- **AX_SUBSCRIBE_1**: `adapter::net::AR_SUBSCRIBE_1` (`QI=TRUE`) — subscribes to new values from the OPC UA client (`ID_READ`).

- **INI_AR**: `eclipse4diac::storage::INI_AR` (`QI=TRUE`, `SETM=FALSE`) — persistently stores the value in the INI file under `SECTION`/`KEY`, with `DEFAULT_VALUE` (in seconds) as the starting value.

- **AX_PUBLISH_1**: `adapter::net::AR_PUBLISH_1` (`QI=TRUE`) — publishes the current REAL value via OPC UA (`ID_WRITE`).

- **Sec_To_Time**: `adapter::iec61131::arithmetic::AR_MULTIME` (`IN1=T#1s`) — multiplies the stored REAL value (seconds) by `T#1s` to provide it as the value `TIME`.

- **How it works**: Like `INI_OPC_PARAM_AR`, except that `INI_AR.AR_OUT` is converted via `Sec_To_Time` (`AR_MULTIME`) to `TIME` and provided as the `ATM` plug `OUT`.


## Program Flow and Connections

1. **Initialization Chain**: `AX_SUBSCRIBE_1.INITO` → `INI_AR.INIT` → `INI_AR.INITO` → `AX_PUBLISH_1.INIT`

2. **Parameters**: `SECTION` → `INI_AR.SECTION`; `KEY` → `INI_AR.KEY`; `DEFAULT_VALUE` (seconds) → `INI_AR.DEFAULT_VALUE`; `ID_READ` → `AX_SUBSCRIBE_1.ID`; `ID_WRITE` → `AX_PUBLISH_1.ID`.

3. **Adapter Chain**: `AX_SUBSCRIBE_1.OUT` → `INI_AR.AR_IN` → `INI_AR.AR_OUT` → `AX_PUBLISH_1.IN` (REAL, published unchanged via OPC UA) **and** → `Sec_To_Time.IN2` → `Sec_To_Time.OUT` → `OUT` (as `TIME`).


## Technical Features

- **`AR_MULTIME` with fixed `IN1=T#1s`**: The multiplication `T#1s * REAL-Sekunden` directly converts the stored numerical value into a `TIME` value, without requiring any conversion by the caller.

- **OPC UA Publish Remains REAL**: `ID_WRITE` continues to publish the original REAL value (seconds) — only the additional `OUT` plug delivers `TIME`.

- Otherwise identical to `INI_OPC_PARAM_AR`](./INI_OPC_PARAM_AR.md).



## Application Scenarios

- Retentive time parameters (e.g., delay or timeout times in seconds) that should be editable as a simple number via OPC UA, but are used internally as a `TIME` value (e.g., for a `E_DELAY` or `timeOut` adapter).

## Comparison with Similar Function Blocks

Compared to [`INI_OPC_PARAM_AR`](./INI_OPC_PARAM_AR.md)], only the additional `Sec_To_Time` conversion (`AR_MULTIME`) is added; The `OUT` plugin is of type `ATM` instead of `AR`. `INI_OPC_PARAM_AX` and ](./INI_OPC_PARAM_AX.md) are the BOOL variants.

## Summary

`INI_OPC_PARAM_ATM` provides a persistently stored, OPC UA editable time parameter as a ready-made `TIME` adapter, calculated from a real value stored in seconds.

---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Color Reference on ms-muc-docs.de ](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
