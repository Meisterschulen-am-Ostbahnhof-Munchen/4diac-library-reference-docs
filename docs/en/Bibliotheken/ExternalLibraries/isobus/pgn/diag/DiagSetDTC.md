# DiagSetDTC

![DiagSetDTC](./DiagSetDTC.svg)

* * * * * * * * * *

## Introduction

Description from the comment of the type: Activate or deactivate a diagnostic trouble code (DTC) in the DM1 message

Package: `isobus::pgn::diag`

## Interface structure

### **Event inputs**

-   **INIT** (Type: `EInit`): Service Initialization
-   **REQ** (Type: `Event`): Set the DTC (activate or deactivate) (with data: `s16CfHandle`, `u32SPN`, `u8FMI`, `bActive`, `u8NumberOfDM1Messages`)

### **Event outputs**

-   **INITO** (Type: `EInit`): Initialization Confirm
-   **CNF** (Type: `Event`): Confirmation (with data: `s16Status`)

### **Data inputs**

-   **s16CfHandle** (Type: `INT`): Handle of the own (internal) CF (Default: `UNVALID`)
-   **u32SPN** (Type: `UDINT`): Suspect Parameter Number of the trouble code (19 bit)
-   **u8FMI** (Type: `USINT`): Failure Mode Identifier, see isobus::pgn::diag::FMI
-   **bActive** (Type: `BOOL`): TRUE = activate, FALSE = deactivate the DTC
-   **u8NumberOfDM1Messages** (Type: `USINT`): 0 or 255: active until deactivated; 1..254: deactivate automatically after this number of DM1 messages (seconds)

### **Data outputs**

-   **s16Status** (Type: `INT`): Return value of iso_BaseDiagSetDM1DTC(): 0 = OK, < 0 = error

### **Adapters (sockets)**

None.

### **Adapters (plugs)**

None.

Note: This page was generated from the type definition (`DiagSetDTC.fbt`) and only shows the interface. A detailed description of the behavior is still missing.
