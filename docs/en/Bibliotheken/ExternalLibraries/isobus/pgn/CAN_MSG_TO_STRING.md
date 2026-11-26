# CAN_MSG_TO_STRING

![CAN_MSG_TO_STRING](./CAN_MSG_TO_STRING.svg)

* * * * * * * * * *

## Introduction

Description from the comment of the type: CAN_MSG von AlPgnRxNew8B (bis 8 Byte) als STRING auslesen

Package: `isobus::pgn`

## Interface structure

### **Event inputs**

-   **REQ** (Type: `Event`): Text auslesen (with data: `MSG`, `STOP_AT_NUL`)

### **Event outputs**

-   **CNF** (Type: `Event`): OUT ist gefuellt (with data: `OUT`)

### **Data inputs**

-   **MSG** (Type: `isobus::pgn::CAN_MSG`): empfangene Nachricht, gelesen werden u16DaSize Bytes (hoechstens 8)
-   **STOP_AT_NUL** (Type: `BOOL`): TRUE: Text endet vor dem ersten 16#00 (Default: `TRUE`)

### **Data outputs**

-   **OUT** (Type: `STRING`): Text, je Byte 1 Zeichen

### **Adapters (sockets)**

None.

### **Adapters (plugs)**

None.

Note: This page was generated from the type definition (`CAN_MSG_TO_STRING.fbt`) and only shows the interface. A detailed description of the behavior is still missing.
