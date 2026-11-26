# STRING_TO_CAN_MSG

![STRING_TO_CAN_MSG](./STRING_TO_CAN_MSG.svg)

* * * * * * * * * *

## Introduction

Description from the comment of the type: STRING (bis 8 Zeichen) in eine CAN_MSG fuer AlPgnTxNew8B packen

Package: `isobus::pgn`

## Interface structure

### **Event inputs**

-   **REQ** (Type: `Event`): Text packen (with data: `IN`, `PRIO`)

### **Event outputs**

-   **CNF** (Type: `Event`): MSG ist gefuellt (with data: `MSG`, `TRUNCATED`)

### **Data inputs**

-   **IN** (Type: `STRING`): Text, je Zeichen 1 Byte
-   **PRIO** (Type: `USINT`): Prioritaet (0..7), groesser 7 wird 7; AlPgnTxNew8B nimmt die Prioritaet vom install, nicht aus MSG (Default: `6`)

### **Data outputs**

-   **MSG** (Type: `isobus::pgn::CAN_MSG`): data = Zeichen, dann 16#00 (falls Platz), Rest 16#FF; u16DaSize = 8
-   **TRUNCATED** (Type: `BOOL`): TRUE, wenn IN laenger als 8 Zeichen war (abgeschnitten)

### **Adapters (sockets)**

None.

### **Adapters (plugs)**

None.

Note: This page was generated from the type definition (`STRING_TO_CAN_MSG.fbt`) and only shows the interface. A detailed description of the behavior is still missing.
