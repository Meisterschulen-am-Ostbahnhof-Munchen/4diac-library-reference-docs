# AE_DELAY

![AE_DELAY](./AE_DELAY.svg)

* * * * * * * * * *

## Introduction

The **AE_DELAY** function block is a wrapper for the standard IEC 61499 block `E_DELAY`, designed for use with **event adapters (AE)**. Delay time is supplied via the `PT` adapter socket (type `ATM`).

## Interface Structure

### **Event Inputs**

None.

### **Event Outputs**

None. Output events are emitted via plug `EO`.

### **Data Inputs**

None.

### **Data Outputs**

None.

### **Adapters**

| Name      | Type                               | Direction        | Comment                                                           |
| :-------- | :--------------------------------- | :--------------- | :---------------------------------------------------------------- |
| **START** | adapter::types::unidirectional::AE | Socket (Input)   | Starts the delay.                                                 |
| **STOP**  | adapter::types::unidirectional::AE | Socket (Input)   | Cancels the running delay.                                        |
| **PT**    | adapter::types::unidirectional::ATM| Socket (Input)   | Preset Time (ATM adapter socket for delay time).                  |
| **EO**    | adapter::types::unidirectional::AE | Plug (Output)    | Event Output after delay expiration.                              |
