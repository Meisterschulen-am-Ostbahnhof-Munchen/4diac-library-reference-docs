# AR_M2M_Subscribe_TO_Q_Publish_OPC

![AR_M2M_Subscribe_TO_Q_Publish_OPC_network](./AR_M2M_Subscribe_TO_Q_Publish_OPC_network.svg)

* * * * * * * * * *

## Introduction

`AR_M2M_Subscribe_TO_Q_Publish_OPC` subscribes to an already converted physical value (REAL) from another station via OPC-UA (M2M), shows it on the VT (`Q_NumericValue_PHYSA`) and republishes it locally so that further local consumers, web mirror or dashboard can read it. In addition, the value is available at the plug `VALUE_OUT` for local adapter consumers. The block is the lean successor of `AR_HZ_TO_Q_Publish_OPC` without its own scaling, because the Hz-to-unit conversion now runs on the sensor station (see [`logiBUS_PI_Speed_Publish_OPC`](logiBUS_PI_Speed_Publish_OPC.md)).

![AR_M2M_Subscribe_TO_Q_Publish_OPC](AR_M2M_Subscribe_TO_Q_Publish_OPC.svg)

## Interface Structure

### **Event Inputs**

- **INIT** (`EInit`): initialization request; passed on to `AR_SUBSCRIBE_M2M.INIT`

### **Event Outputs**

- **INITO** (`EInit`): initialization confirmation; comes from `Publish.INITO`

### **Data Inputs**

- **QI** (BOOL, initial value `TRUE`): quality input, enables the service; passed on to `AR_SUBSCRIBE_M2M.QI` and `Publish.QI`
- **ID_M2M_READ** (WSTRING): remote subscribe address of the finished physical value (REAL), e.g. `FBs::Ventilsteuerung::Ventilsteuerung_STG1::RemoteSubStrings::AUFNAHME_GESCHWINDIGKEIT_KMH_M2M`
- **stObj** (`logiBUS::utils::conversion::phys::NumericObjectPool_S`): object pool properties of the VT number variable (`u16ObjId`, scale, offset, decimals), e.g. `FBs::const::DefaultPool_Numeric::NumberVariable_X_N`
- **ID_WRITE** (WSTRING): local publish address (REAL, physical value) for web mirror, dashboard and self-subscribe of further local consumers

### **Data Outputs**

- **QO** (BOOL): quality output, AND combination (`AND_QO`) of the `QO` outputs of `AR_SUBSCRIBE_M2M` and `Publish`

### **Adapters**

- **VALUE_OUT** (plug, `adapter::types::unidirectional::AR`): read-only mirror of the remotely subscribed value for further local consumers; avoids a duplicate OPC-UA self-subscribe of the same remote node on this station

## Functionality

The network consists of four parts:

- `AR_SUBSCRIBE_M2M` (`adapter::net::AR_SUBSCRIBE_1`, `QI` comes from the input `QI`): subscribes to the address `ID_M2M_READ` (`ID_M2M_READ` → `AR_SUBSCRIBE_M2M.ID`) and outputs the value as an AR adapter.
- `AR_SPLIT_2_Value` (`adapter::events::unidirectional::AR_SPLIT_2`): distributes the subscribed value to two destinations (`AR_SUBSCRIBE_M2M.OUT` → `AR_SPLIT_2_Value.IN`).
- `Publish` (`MyLib::sys::AR_TO_Q_Publish_OPC`): displays the value on the VT and publishes it locally (`AR_SPLIT_2_Value.OUT1` → `Publish.AR_IN`, `stObj` → `Publish.stObj`, `ID_WRITE` → `Publish.ID_WRITE`).

- `AND_QO` (`iec61131::booleanOperators::AND_BOOL_2`): combines `AR_SUBSCRIBE_M2M.QO` and `Publish.QO` into the output `QO`.

`AR_SPLIT_2_Value.OUT2` is routed to the plug `VALUE_OUT`.

Initialization and quality: `INIT` → `AR_SUBSCRIBE_M2M.INIT`, `AR_SUBSCRIBE_M2M.INITO` → `Publish.INIT`, `Publish.INITO` → `INITO` and `AND_QO.REQ`. `QI` is passed on to `AR_SUBSCRIBE_M2M.QI` and `Publish.QI`; `AR_SUBSCRIBE_M2M.QO` → `AND_QO.IN1`, `Publish.QO` → `AND_QO.IN2`, `AND_QO.OUT` → `QO`.

## Technical Details

- No more `SCALE`/`AR_MUL_2`: the subscribed value is already in the physical unit
- The `AR_SPLIT_2` in front of `Publish.AR_IN` was inserted instead of wiring `AR_SUBSCRIBE_M2M.OUT` directly, so that an adapter is never branched without a split
- `VALUE_OUT` supplies the subscribed value to consumers such as automatic filling or filling belt detection without creating a second subscribe on the same remote node

## Application Scenarios

- Displaying a value measured by another station (e.g. speed) on the VT while making it available locally to dashboard and web client
- Passing the remote value on to local control or automation blocks via `VALUE_OUT`

## Conclusion

The block bundles remote subscribe, VT display and local republish of a finished physical value into one SubApp and additionally provides the value as an AR plug.
