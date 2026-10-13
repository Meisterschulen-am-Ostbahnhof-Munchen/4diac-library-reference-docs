# SUBSCRIBE_24

* * * * * * * * * *

## Introduction

The function block `SUBSCRIBE_24` subscribes to a [`PUBLISH_24`](PUBLISH_24.md) block and provides the received data (up to 24 values) at its outputs.

![SUBSCRIBE_24](SUBSCRIBE_24.svg)

## Interface Structure

### **Event Inputs**

- **INIT** (`EInit`): initialisation. Associated with `QI`, `ID`.
- **RSP**: response to an indication. Associated with `QI`.

### **Event Outputs**

- **INITO** (`EInit`): initialisation confirm. Associated with `QO`, `STATUS`.
- **IND**: new data available. Associated with `QO`, `STATUS`, `RD_1` to `RD_24`.

### **Data Inputs**

- **QI** (`BOOL`): event input qualifier
- **ID** (`WSTRING`): identification string of the communication channel (must match that of the `PUBLISH_24`)

### **Data Outputs**

- **QO** (`BOOL`): event output qualifier
- **STATUS** (`WSTRING`): status information
- **RD_1** to **RD_24** (`ANY`): received data values

### **Adapters**

No adapter interfaces available.

## Functionality

After initialisation with `INIT`, the block signals `IND` whenever new data is received and provides the values `RD_1` to `RD_24`. The service is implemented as a service interface (`APPLICATION` ↔ `RESOURCE`); `RSP` answers the indication.

## Technical Details

- Generic block (generic class `GEN_SUBSCRIBE`): the number of data ports is 24 here, their types (`ANY`) are fixed at instantiation
- Unicode strings (`WSTRING`) for `ID` and `STATUS`
- Counterpart: `PUBLISH_24` with `SD_1` to `SD_24`

## Application Scenarios

- Receiving many related values that a `PUBLISH_24` sends elsewhere

## ⚖️ Comparison with Similar Building Blocks

[`SUBSCRIBE_10`](SUBSCRIBE_10.md) receives up to 10 data values, `SUBSCRIBE_24` up to 24.

## Conclusion

`SUBSCRIBE_24` is the receiving counterpart of `PUBLISH_24` for up to 24 data values.
