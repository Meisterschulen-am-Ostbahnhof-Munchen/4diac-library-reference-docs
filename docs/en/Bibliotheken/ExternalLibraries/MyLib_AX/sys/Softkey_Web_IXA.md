# Softkey_Web_IXA

![Softkey_Web_IXA_network](./Softkey_Web_IXA_network.svg)

* * * * * * * * * *

## Introduction

`Softkey_Web_IXA` merges a **SoftKey (VT)** OR a local **web override** (vt-ui-mirror, continuous state) into **one** common continuous `AX` state. It is intended for keys without an AUX assignment (e.g. undercarriage centre point) that should nevertheless be pressable or holdable via the web client just like physically. Analogous to [`Softkey_Aux_IXA_TO_Remote_WRITE`](./Softkey_Aux_IXA_TO_Remote_WRITE.md), which additionally has AUX and remote write; here only the pure merge without AUX and without writing anywhere. The caller processes the merged state further itself (e.g. with [`SoftKey_Kurz_Lang`](./SoftKey_Kurz_Lang.md)).

![Softkey_Web_IXA](Softkey_Web_IXA.svg)

## Interface Structure

### **Event Inputs**

- **INIT** (`EInit`): initialization request; starts the chain `Softkey` → `WebSub`

### **Event Outputs**

- **INITO** (`EInit`): initialization confirmation; comes from `WebSub.INITO`

### **Data Inputs**

- **QI** (`BOOL`, default `TRUE`): quality input, service enable; passed on to `Softkey` and `WebSub`
- **u16ObjId** (`UINT`): object ID of the SoftKey
- **ID_WEB_READ** (`WSTRING`): local subscribe address for the web client (vt-ui-mirror): down/up like a held physical key

### **Data Outputs**

- **QO** (`BOOL`): quality output, `AND` of the `QO` outputs of `Softkey` and `WebSub` (via `AND_QO`)

### **Adapters**

- **OUT** (plug, `adapter::types::unidirectional::AX`): SoftKey OR web override as one common continuous state

## Function Blocks (FBs) Used

- **`Softkey`** (`isobus::UT::io::Softkey::Softkey_IXA`): reads the SoftKey state
- **`WebSub`** (`adapter::net::AX_SUBSCRIBE_1`): subscribes to the web override
- **`OR_MERGE`** (`adapter::booleanOperators::AX_OR_2`): OR combination of both states
- **`AND_QO`** (`iec61131::booleanOperators::AND_BOOL_2`): combines `Softkey.QO` and `WebSub.QO` into the SubApp output `QO`

## Functionality

- `u16ObjId` → `Softkey.u16ObjId`; `ID_WEB_READ` → `WebSub.ID`
- `Softkey.IN` → `OR_MERGE.IN1`; `WebSub.OUT` → `OR_MERGE.IN2`
- `OR_MERGE.OUT` → `OUT`
- Initialization: `INIT` → `Softkey.INIT`; `Softkey.INITO` → `WebSub.INIT`; `WebSub.INITO` → `INITO` and → `AND_QO.REQ`
- Quality: `QI` → `Softkey.QI` and `WebSub.QI`; `Softkey.QO` → `AND_QO.IN1`, `WebSub.QO` → `AND_QO.IN2`; `AND_QO.OUT` → `QO`

## Technical Details

- Continuous state (not an event): the web client can hold the key like a physical one
- No AUX, no remote write

## Application Scenarios

- Keys without AUX assignment that should also be operable via the web client and whose state is processed further e.g. by `SoftKey_Kurz_Lang`

## Summary

`Softkey_Web_IXA` ORs SoftKey and web override into one continuous `AX` state.

---

### 🌐 Related topic pages on ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & color reference on ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
