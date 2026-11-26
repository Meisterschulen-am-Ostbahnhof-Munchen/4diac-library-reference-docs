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

## Operation (from the source)

This description is derived from the type definition and shows states, transitions and algorithms. It has not been tested on hardware.

### Internal blocks

-   **CTRL**: instance of `isobus::UT::dialog::DLG_CTRL`
-   **QN_COVER**: instance of `isobus::UT::Q::Q_NumericValue`
-   **QN_ICON**: instance of `isobus::UT::Q::Q_NumericValue`
-   **QN_TITLE**: instance of `isobus::UT::Q::Q_NumericValue`
-   **QN_TEXT**: instance of `isobus::UT::Q::Q_NumericValue`
-   **QN_BUTTONS**: instance of `isobus::UT::Q::Q_NumericValue`
-   **QF_TITLE**: instance of `isobus::UT::Q::Q_FillAttributes`
-   **QH_SHOW**: instance of `isobus::UT::Q::Q_ObjHideShow`
-   **QH_HIDE**: instance of `isobus::UT::Q::Q_ObjHideShow`
-   **FOCUS**: instance of `iec61499::events::E_DEMUX_8`
-   **QS_OK**: instance of `isobus::UT::Q::Q_ObjSelectInput`
-   **QS_CANCEL**: instance of `isobus::UT::Q::Q_ObjSelectInput`
-   **QS_ABORT**: instance of `isobus::UT::Q::Q_ObjSelectInput`
-   **QS_RETRY**: instance of `isobus::UT::Q::Q_ObjSelectInput`
-   **QS_IGNORE**: instance of `isobus::UT::Q::Q_ObjSelectInput`
-   **QS_YES**: instance of `isobus::UT::Q::Q_ObjSelectInput`
-   **QS_NO**: instance of `isobus::UT::Q::Q_ObjSelectInput`
-   **FOCUS_DONE**: instance of `iec61499::events::E_MUX_8`
-   **AUDIO_SW**: instance of `iec61499::events::E_SWITCH`
-   **Q_AUDIO**: instance of `isobus::UT::Q::Q_CtrlAudioSignal`
-   **AUDIO_DONE**: instance of `iec61499::events::E_MERGE`
-   **TIMER**: instance of `iec61499::events::E_DELAY`
-   **BTN_OK**: instance of `isobus::UT::io::Button::Button_IE`
-   **BTN_CANCEL**: instance of `isobus::UT::io::Button::Button_IE`
-   **BTN_ABORT**: instance of `isobus::UT::io::Button::Button_IE`
-   **BTN_RETRY**: instance of `isobus::UT::io::Button::Button_IE`
-   **BTN_IGNORE**: instance of `isobus::UT::io::Button::Button_IE`
-   **BTN_YES**: instance of `isobus::UT::io::Button::Button_IE`
-   **BTN_NO**: instance of `isobus::UT::io::Button::Button_IE`

Note: This page was generated from the type definition (`DLG_MessageBox.fbt`) and only shows the interface. A detailed description of the behavior is still missing.
