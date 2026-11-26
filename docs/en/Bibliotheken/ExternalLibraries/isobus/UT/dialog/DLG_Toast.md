# DLG_Toast

![DLG_Toast](./DLG_Toast.svg)

* * * * * * * * * *

## Introduction

Description from the comment of the type: Hinweis ohne Tasten, verschwindet nach TIMEOUT

Package: `isobus::UT::dialog`

## Interface structure

### **Event inputs**

-   **INIT** (Type: `EInit`): Initialisierung (with data: `QI`)
-   **SHOW** (Type: `Event`): Hinweis zeigen (with data: `QI`, `TEXT_ID`, `TITLE_ID`, `ICON`, `TIMEOUT`)
-   **CLOSE** (Type: `Event`): Hinweis schliessen

### **Event outputs**

-   **INITO** (Type: `EInit`): Initialisierung fertig (with data: `QO`)
-   **CNF** (Type: `Event`): Hinweis beendet (with data: `QO`)

### **Data inputs**

-   **QI** (Type: `BOOL`): Freigabe (Default: `TRUE`)
-   **TEXT_ID** (Type: `UINT`): OutputString Meldungstext (Default: `ID_NULL`)
-   **TITLE_ID** (Type: `UINT`): OutputString Titel, NULL = Standard (Default: `ID_NULL`)
-   **ICON** (Type: `USINT`): DLG_ICON_* (Default: `USINT#1`)
-   **TIMEOUT** (Type: `TIME`): Anzeigedauer (Default: `T#3s`)

### **Data outputs**

-   **QO** (Type: `BOOL`): Qualitaet

### **Adapters (sockets)**

None.

### **Adapters (plugs)**

None.

Note: This page was generated from the type definition (`DLG_Toast.fbt`) and only shows the interface. A detailed description of the behavior is still missing.
