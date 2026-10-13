# AR_S_TO_ATM

![AR_S_TO_ATM_network](./AR_S_TO_ATM_network.svg)

* * * * * * * * * *

## Introduction

`AR_S_TO_ATM` converts an `AR` value (REAL, seconds) into an `ATM` value (TIME). It is needed for TIME sockets (e.g. `FT_PT1_AR.TM`) that are to be fed by a REAL quantity (AR2) that is editable via VT or web and edited in seconds.

![AR_S_TO_ATM](AR_S_TO_ATM.svg)

## Interface Structure

### **Event Inputs**

The function block has no direct event inputs.

### **Event Outputs**

The function block has no direct event outputs.

### **Data Inputs**

The function block has no direct data inputs.

### **Data Outputs**

The function block has no direct data outputs.

### **Adapters**

- **IN1** (socket, `adapter::types::unidirectional::AR`): time in seconds (REAL)
- **OUT** (plug, `adapter::types::unidirectional::ATM`): `IN1`, converted from seconds (REAL) to TIME

## Functionality

Internally a single instance `Sec_To_Time` of type `adapter::iec61131::arithmetic::AR_MULTIME` is used with the parameter `IN1 = T#1s`. The socket `IN1` is routed to `Sec_To_Time.IN2` and the result `Sec_To_Time.OUT` to the plug `OUT`. Thus `OUT = T#1s * IN1`, i.e. true IEC 61131-3 TIME arithmetic: an input of `2.5` yields `T#2s500ms`.

## Technical Details

- The earlier variant (`AR_R_MUL` × 1000 → `AR_TO_ADI` → `ADI_TO_ATM`) was replaced because it relied on a non-existent function `iec61131::conversion::F_DINT_TO_TIME`
- Stateless; events travel via the adapters (`E1` of `IN1` triggers the calculation)

## Application Scenarios

- Adjustable PT1 filter time of the pulse frequency sensors (see [`logiBUS_PI_Speed_Publish_OPC`](logiBUS_PI_Speed_Publish_OPC.md)): edited in seconds, routed as TIME to `FT_PT1_AR.TM`
- Any place where a quantity entered in seconds has to be passed to a TIME socket

## Conclusion

`AR_S_TO_ATM` is a small adapter SubApp that converts second values to TIME using `T#1s * seconds`.
