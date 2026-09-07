# Softkey_Aux_IXA_TO_Remote_WRITE

![Softkey_Aux_IXA_TO_Remote_WRITE_network](./Softkey_Aux_IXA_TO_Remote_WRITE_network.svg)

* * * * * * * * * *

## Introduction

`Softkey_Aux_IXA_TO_Remote_WRITE` is the command half of `Softkey_Aux_IXA_TO_Remote_WRITE_BG_OPC`, extracted as an independent component: VT SoftKey, AUX assignment (joystick), and a local web override (e.g., vt-ui-mirror on the same module) are combined using an OR operator and written to a target module via remote write—without status feedback.



## Function Blocks (FBs) Used

### Sub-Blocks: Softkey_Aux_IXA_TO_Remote_WRITE

- **Type**: SubAppType
- **Internal FBs Used**:

- **Softkey_IXA**: `isobus::UT::io::Softkey::Softkey_IXA` (`QI=TRUE`) — VT SoftKey Adapter (`u16ObjId`).

- **Aux_IXA**: `isobus::UT::io::Auxiliary::IN::Aux_IXA` (`QI=TRUE`) — AUX Assignment/Joystick Adapter (`u16ObjIdA`).

- **SUBSCRIBE_WEB**: `adapter::net::AX_SUBSCRIBE_1` (`QI=TRUE`) — Local web subscribe (`ID_WEB_READ`), e.g., from a vt-ui-mirror on the same module.

- **OR_MERGE**: `adapter::booleanOperators::AX_OR_3` — Combines SoftKey, Aux, and Web Override using OR.

- **AX_CLIENT_1_0**: `adapter::net::AX_CLIENT_1_0` (`QI=TRUE`) — Writes the combined signal to the target module using remote write (`ID_WRITE_REMOTE`).


- **How it Works**: Three independent command sources (SoftKey, Aux, local web override) are combined using `AX_OR_3` and transmitted to the target module as a single remote write command.

## Program Flow and Connections

1. `u16ObjId` → `Softkey_IXA.u16ObjId`; `u16ObjIdA` → `Aux_IXA.u16ObjId`; `ID_WEB_READ` → `SUBSCRIBE_WEB.ID`; `ID_WRITE_REMOTE` → `AX_CLIENT_1_0.ID` (Data connections, hidden).

2. `Softkey_IXA.IN` → `OR_MERGE.IN1`; `Aux_IXA.IN` → `OR_MERGE.IN2`; `SUBSCRIBE_WEB.OUT` → `OR_MERGE.IN3`.

3. `OR_MERGE.OUT` → `AX_CLIENT_1_0.IN`.

## Technical Features

- **Three Equal Command Sources**: SoftKey, AUX Assignment, and Local Web Override function equally via the OR operator—any of the three sources can trigger the command.

- **No Status Feedback**: Unlike `Softkey_Aux_IXA_TO_Remote_WRITE_BG_OPC`, this block does not contain any feedback/background color; For this purpose, `AX_SUBSCRIBE_BG3_WEB_OPC` should be placed alongside it with the same values as `u16ObjId`/`u16ObjIdA`.

## Application Scenarios

- Remote control of a target module via soft key, AUX joystick assignment, or a local web client, without requiring a status display on the same module.

## Comparison with Similar Modules

For the complete, pre-wired combination of both sides (command + status), `Softkey_Aux_IXA_TO_Remote_WRITE_BG_OPC` should still be used, which contains this module as a command sub-module.

## Summary

`Softkey_Aux_IXA_TO_Remote_WRITE` encapsulates the pure command side of a remote channel: three OR-linked sources (soft key, aux, local web override), written to a target module via remote write.


---

### 🌐 Related topic subpages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Color Reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
