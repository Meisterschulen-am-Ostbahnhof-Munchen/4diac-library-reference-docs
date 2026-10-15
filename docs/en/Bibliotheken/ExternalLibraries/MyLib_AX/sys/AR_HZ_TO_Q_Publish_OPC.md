# AR_HZ_TO_Q_Publish_OPC

![AR_HZ_TO_Q_Publish_OPC_network](./AR_HZ_TO_Q_Publish_OPC_network.svg)

* * * * * * * * * *

## Introduction

`AR_HZ_TO_Q_Publish_OPC` bundles the chain **remote subscribe of a frequency (Hz) → fixed scaling → VT display (`Q_NumericValue_PHYSA`) and OPC-UA publish** into one reusable SubApp. It combines `AR_SUBSCRIBE_1`, `initval_AR`, `AR_MUL_2` and `AR_TO_Q_Publish_OPC` - one block per pulse speed/rotation sensor (pickup B03, hedgehog belt 1/2 B10/B11, saw blade B08), only the parameters differ. It was extracted from `Inputs_STG1.SUB`, where four nearly identical chains existed.

![AR_HZ_TO_Q_Publish_OPC](AR_HZ_TO_Q_Publish_OPC.svg)

## Interface Structure

### **Event Inputs**

- **INIT** (`EInit`): initialization request; starts the initialization chain (`AR_SUBSCRIBE_Hz` → `Publish`)

### **Event Outputs**

- **INITO** (`EInit`): initialization confirmation; comes from `Publish.INITO` after the chain has completed

### **Data Inputs**

- **QI** (`BOOL`): quality input, service enable (initial value `TRUE`), passed on to `AR_SUBSCRIBE_Hz.QI` and `Publish.QI`
- **ID_HZ_REMOTE** (`WSTRING`): Remote subscribe address of the published frequency (Hz), e.g. `FBs::Ventilsteuerung::Ventilsteuerung_STG1::RemoteSubStrings::AUFNAHME_GESCHWINDIGKEIT_HZ_REMOTE`
- **SCALE** (`REAL`): Scale factor Hz → physical unit (e.g. `3.6/53` for km/h, `60.0` for rpm at 1 pulse/revolution)
- **stObj** (`logiBUS::utils::conversion::phys::NumericObjectPool_S`): Object pool properties of the VT numeric variable (`u16ObjId`, `scale`, `offset`, `decimals`), e.g. `FBs::const::DefaultPool_Numeric::NumberVariable_X_N`
- **ID_WRITE** (`WSTRING`): Local publish address (REAL, physical value) for web mirror/dashboard/self-subscribe

### **Data Outputs**

- **QO** (`BOOL`): quality output, `AND` of the `QO` outputs of `AR_SUBSCRIBE_Hz` and `Publish` (via `AND_QO`)

### **Adapters**

- **HZ_RAW_OUT** (plug): Unidirectional adapter of type `adapter::types::unidirectional::AR` - optional read-only mirror of the still unscaled raw value (Hz) before the `SCALE` multiplication. Leave unconnected if not needed.

## Function Blocks (FBs) Used

- **AR_SUBSCRIBE_Hz**: [`adapter::net::AR_SUBSCRIBE_1`](../../adapter/net/AR_SUBSCRIBE_1.md) (`QI` comes from the SubApp interface), subscribes to the remote address `ID_HZ_REMOTE`
- **LIT_Scale**: `adapter::types::unidirectional::AR::initval::initval_AR`, provides `SCALE` as an AR value
- **AR_SPLIT_2_HzRaw**: [`adapter::events::unidirectional::AR_SPLIT_2`](../../adapter/events/unidirectional/REAL/AR_SPLIT_2.md), branches the raw value
- **AR_MUL**: [`adapter::iec61131::arithmetic::AR_MUL_2`](../../adapter/iec61131-3/arithmetic/AR_MUL_2.md), multiplies the raw value by `SCALE`
- **Publish**: SubApp `MyLib::sys::AR_TO_Q_Publish_OPC`, shows the value on the VT (`stObj`) and publishes it via OPC-UA (`ID_WRITE`)
- **AND_QO**: `iec61131::booleanOperators::AND_BOOL_2`, combines the `QO` outputs of `AR_SUBSCRIBE_Hz` and `Publish` into the SubApp output `QO`

## Program Flow and Connections

1. `ID_HZ_REMOTE` → `AR_SUBSCRIBE_Hz.ID`; the output `AR_SUBSCRIBE_Hz.OUT` leads to `AR_SPLIT_2_HzRaw.IN`.
2. `AR_SPLIT_2_HzRaw.OUT1` → plug `HZ_RAW_OUT` (unscaled raw value, optional).
3. `AR_SPLIT_2_HzRaw.OUT2` → `AR_MUL.IN1`; `SCALE` → `LIT_Scale.INIT_VAL`, `LIT_Scale.OUT` → `AR_MUL.IN2`.
4. `AR_MUL.OUT` → `Publish.AR_IN`.
5. `stObj` → `Publish.stObj`, `ID_WRITE` → `Publish.ID_WRITE`.
6. Initialization: `INIT` → `AR_SUBSCRIBE_Hz.INIT`; `AR_SUBSCRIBE_Hz.INITO` → `Publish.INIT`; `Publish.INITO` → `INITO` and → `AND_QO.REQ`.
7. Quality: `QI` → `AR_SUBSCRIBE_Hz.QI` and `Publish.QI`; `AR_SUBSCRIBE_Hz.QO` → `AND_QO.IN1`, `Publish.QO` → `AND_QO.IN2`; `AND_QO.OUT` → `QO`.

## Technical Details

- **`HZ_RAW_OUT` (version 1.1)**: The optional plug mirrors the raw value (Hz) for a diagnostic display without needing a second, independent remote subscribe of the same node (FORTE reports `Cannot add another one` on a duplicate self-subscribe).
- **INIT/INITO and QI/QO**: initialization is passed through the chain `INIT` → `AR_SUBSCRIBE_Hz` → `Publish` → `INITO`; `QO` is the `AND` of the `QO` outputs of both blocks.
- Pure wiring of existing blocks; apart from the `AND_QO` combination, no logic of its own.

## Application Scenarios

- Displaying and publishing speed or rotation from a pulse sensor frequency value (e.g. pickup, hedgehog belts, saw blade)

## Summary

`AR_HZ_TO_Q_Publish_OPC` replaces the fourfold duplicated chain of subscribe, scaling and publish with a single parameterizable block.
