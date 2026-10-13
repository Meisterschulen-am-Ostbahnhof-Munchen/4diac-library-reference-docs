# NumericValue_TO_CLIENT_1_0_OPC

![NumericValue_TO_CLIENT_1_0_OPC_network](./NumericValue_TO_CLIENT_1_0_OPC_network.svg)

* * * * * * * * * *

## Introduction

`NumericValue_TO_CLIENT_1_0_OPC` connects a VT input field (`CInputNumber`) to a remote **OPC-UA method**: **only on a genuine input** (`NumericValue_PHYSA` fires on commit) is the method (`CALL_METHOD`, `AR_CLIENT_1_0`) called with the entered `REAL` value as argument. The return path - displaying the actually effective, confirmed value on the same VT field - runs separately and uncritically via a normal subscribe (`Q_NumericValue_PHYSA`). Hence there is no circular reference and no merge/last-writer-wins: on the receiver side there is only one caller per method, serialized by the method call itself.

![NumericValue_TO_CLIENT_1_0_OPC](NumericValue_TO_CLIENT_1_0_OPC.svg)

## Interface Structure

### **Event Inputs**

*No direct event inputs*

### **Event Outputs**

*No direct event outputs*

### **Data Inputs**

- **stObj** (`logiBUS::utils::conversion::phys::NumericObjectPool_S`): Object ID and scale/offset/decimals of the VT input field; used for input and output (the confirmed value is written back here). Default: `(u16ObjId := ID_NULL, r32Scale := 1.0, i32Offset := 0, u8Decimals := 1)`
- **ID_CALL** (`WSTRING`): Remote method address (`ACTION=CALL_METHOD`); called only on a genuine VT input with the entered value as argument
- **ID_M2M_READ** (`WSTRING`): Remote subscribe address (`REAL`, `ACTION=SUBSCRIBE`); returns the actually effective value for display on the same VT field (read only)

### **Data Outputs**

*No data outputs*

### **Adapters**

This function block has no adapter sockets/plugs at its interface; `AR` adapters are used internally.

## Function Blocks (FBs) Used

- **Input**: `isobus::UT::io::NumericValue::NumericValue_PHYSA` (`QI=TRUE`) - provides the entered physical value (`rPhys`) on commit
- **Call**: `adapter::net::AR_CLIENT_1_0_UNGATED` (`QI=TRUE`) - calls the remote method with the value as argument
- **Subscribe**: [`adapter::net::AR_SUBSCRIBE_1`](../../adapter/net/AR_SUBSCRIBE_1.md) (`QI=TRUE`) - subscribes to the confirmed value
- **Display**: `isobus::UT::Q::Q_NumericValue_PHYSA` - writes the value to the VT field

## Program Flow and Connections

1. **Forward path**: `Input.rPhys` → `Call.IN` (input → method call).
2. **Return path**: `Subscribe.OUT` → `Display.rPhys` (confirmed value → VT display).
3. `Subscribe.INITO` → `Call.INIT`: the client is initialized only after the subscribe has been initialized.
4. `stObj` → `Input.stObj` and `Display.stObj`; `ID_CALL` → `Call.ID`; `ID_M2M_READ` → `Subscribe.ID`.

## Technical Details

- Forward and return paths are separate, so no merge is needed.
- `AR_CLIENT_1_0_UNGATED` is the ungated variant of the client.

## Application Scenarios

- Setpoint entry on the VT that takes effect on another station via an OPC-UA method, with feedback of the actually valid value

## Summary

`NumericValue_TO_CLIENT_1_0_OPC` writes VT inputs to the target station via an OPC-UA method call and displays the confirmed value independently via a subscribe.
