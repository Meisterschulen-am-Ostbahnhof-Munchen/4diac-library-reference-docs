# DLG_MessageBox

![DLG_MessageBox](./DLG_MessageBox.svg)

* * * * * * * * * *

## Introduction

Description from the comment of the type: VT-Dialog wie Windows-MessageBox (eine Instanz je Anwendung)

Package: `isobus::UT::dialog`

## Interface structure

### **Event inputs**

-   **INIT** (Type: `EInit`): Initialisierung (with data: `QI`)
-   **SHOW** (Type: `Event`): Dialog zeigen (with data: `QI`, `TEXT_ID`, `TITLE_ID`, `ICON`, `BUTTONS`, `DEFAULT_BUTTON`, `MODAL`, `TIMEOUT`, `AUDIO`)
-   **CLOSE** (Type: `Event`): Dialog schliessen

### **Event outputs**

-   **INITO** (Type: `EInit`): Initialisierung fertig (with data: `QO`)
-   **CNF** (Type: `Event`): Dialog beendet, RESULT gueltig (with data: `QO`, `RESULT`)

### **Data inputs**

-   **QI** (Type: `BOOL`): Freigabe (Default: `TRUE`)
-   **TEXT_ID** (Type: `UINT`): OutputString Meldungstext (Default: `ID_NULL`)
-   **TITLE_ID** (Type: `UINT`): OutputString Titel, NULL = Standard je Icon (Default: `ID_NULL`)
-   **ICON** (Type: `USINT`): DLG_ICON_* (Default: `USINT#1`)
-   **BUTTONS** (Type: `USINT`): DLG_BTN_* (Default: `USINT#1`)
-   **DEFAULT_BUTTON** (Type: `USINT`): Fokus auf Taste 1..3 (Default: `USINT#1`)
-   **MODAL** (Type: `BOOL`): Maske abdecken (Default: `FALSE`)
-   **TIMEOUT** (Type: `TIME`): T#0s = kein Timeout (Default: `T#0s`)
-   **AUDIO** (Type: `BOOL`): Ton beim Oeffnen (Default: `FALSE`)

### **Data outputs**

-   **QO** (Type: `BOOL`): Qualitaet
-   **RESULT** (Type: `USINT`): DLG_RES_*

### **Adapters (sockets)**

None.

### **Adapters (plugs)**

None.

Note: This page was generated from the type definition (`DLG_MessageBox.fbt`) and only shows the interface. A detailed description of the behavior is still missing.
