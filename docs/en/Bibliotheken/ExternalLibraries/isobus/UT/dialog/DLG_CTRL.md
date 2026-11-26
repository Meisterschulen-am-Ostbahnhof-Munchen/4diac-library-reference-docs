# DLG_CTRL

![DLG_CTRL](./DLG_CTRL.svg)

* * * * * * * * * *

## Introduction

Description from the comment of the type: Ablaufsteuerung eines VT-Dialogs (MessageBox)

Package: `isobus::UT::dialog`

## Interface structure

### **Event inputs**

-   **INIT** (Type: `EInit`): Initialisierung (with data: `QI`)
-   **SHOW** (Type: `Event`): Dialog zeigen (with data: `TEXT_ID`, `TITLE_ID`, `ICON`, `BUTTONS`, `DEFAULT_BUTTON`, `MODAL`, `TIMEOUT`, `AUDIO`)
-   **CLOSE** (Type: `Event`): Dialog vom Programm schliessen
-   **SHOWN** (Type: `Event`): Anzeige-Kette fertig
-   **HIDDEN** (Type: `Event`): Ausblenden fertig
-   **TMO** (Type: `Event`): Zeit abgelaufen
-   **B_OK** (Type: `Event`): Button OK losgelassen
-   **B_CANCEL** (Type: `Event`): Button Cancel losgelassen
-   **B_ABORT** (Type: `Event`): Button Abort losgelassen
-   **B_RETRY** (Type: `Event`): Button Retry losgelassen
-   **B_IGNORE** (Type: `Event`): Button Ignore losgelassen
-   **B_YES** (Type: `Event`): Button Yes losgelassen
-   **B_NO** (Type: `Event`): Button No losgelassen

### **Event outputs**

-   **INITO** (Type: `EInit`): Initialisierung fertig (with data: `QO`)
-   **SHOWO** (Type: `Event`): Anzeige-Kette starten (with data: `COVER_ID`, `ICON_ID`, `TITLE_ID_O`, `TEXT_ID_O`, `BUTTONS_ID`, `TITLE_COLOUR`, `FOCUS_K`, `AUDIO_O`)
-   **HIDEO** (Type: `Event`): Dialog ausblenden
-   **START_T** (Type: `Event`): Timeout starten (with data: `TIMEOUT_O`)
-   **STOP_T** (Type: `Event`): Timeout stoppen
-   **CNF** (Type: `Event`): Dialog beendet (with data: `QO`, `RESULT`)

### **Data inputs**

-   **QI** (Type: `BOOL`): Freigabe (Default: `TRUE`)
-   **TEXT_ID** (Type: `UINT`): OutputString Meldungstext (Default: `UINT#65535`)
-   **TITLE_ID** (Type: `UINT`): OutputString Titel, NULL = Standard je Icon (Default: `UINT#65535`)
-   **ICON** (Type: `USINT`): DLG_ICON_* (Default: `USINT#1`)
-   **BUTTONS** (Type: `USINT`): DLG_BTN_* (Default: `USINT#1`)
-   **DEFAULT_BUTTON** (Type: `USINT`): Fokus auf Taste 1..3 (Default: `USINT#1`)
-   **MODAL** (Type: `BOOL`): Maske abdecken (Default: `FALSE`)
-   **TIMEOUT** (Type: `TIME`): T#0s = kein Timeout (Default: `T#0s`)
-   **AUDIO** (Type: `BOOL`): Ton beim Oeffnen (Default: `FALSE`)

### **Data outputs**

-   **QO** (Type: `BOOL`): Qualitaet
-   **RESULT** (Type: `USINT`): DLG_RES_*
-   **COVER_ID** (Type: `UDINT`): Ziel ObjectPointer_Dlg_Cover
-   **ICON_ID** (Type: `UDINT`): Ziel ObjectPointer_Dlg_Icon
-   **TITLE_ID_O** (Type: `UDINT`): Ziel ObjectPointer_Dlg_Title
-   **TEXT_ID_O** (Type: `UDINT`): Ziel ObjectPointer_Dlg_Text
-   **BUTTONS_ID** (Type: `UDINT`): Ziel ObjectPointer_Dlg_Buttons
-   **TITLE_COLOUR** (Type: `USINT`): VT-Farbindex Titelzeile
-   **FOCUS_K** (Type: `UINT`): Fokus-Button 0..6, 7 = keiner
-   **AUDIO_O** (Type: `BOOL`): Ton ja/nein
-   **TIMEOUT_O** (Type: `TIME`): Timeout

### **Adapters (sockets)**

None.

### **Adapters (plugs)**

None.

## Operation (from the source)

This description is derived from the type definition and shows states, transitions and algorithms. It has not been tested on hardware.

### Execution control chart (ECC)

-   **START** (Action: no action)
-   **S_INIT** (Action: `INIT_ALG` → sends event `INITO`)
-   **IDLE** (Action: no action)
-   **S_PREP** (Action: `PREP` → sends event `SHOWO`)
-   **OPENING** (Action: no action)
-   **S_O_SHOW** (Action: `MARK_SHOW`)
-   **S_O_CLOSE** (Action: `R_NONE`, `MARK_CLOSE`)
-   **S_REPLACE** (Action: `R_NONE` → sends event `CNF`)
-   **S_START_T** (Action: sends event `START_T`)
-   **OPEN** (Action: no action)
-   **S_R_OK** (Action: `R_OK`)
-   **S_R_CANCEL** (Action: `R_CANCEL`)
-   **S_R_ABORT** (Action: `R_ABORT`)
-   **S_R_RETRY** (Action: `R_RETRY`)
-   **S_R_IGNORE** (Action: `R_IGNORE`)
-   **S_R_YES** (Action: `R_YES`)
-   **S_R_NO** (Action: `R_NO`)
-   **S_R_TMO** (Action: `R_TMO`)
-   **S_R_CLOSE** (Action: `R_NONE`)
-   **S_REPLACE_OPEN** (Action: `R_NONE` → sends event `STOP_T`, sends event `CNF`)
-   **S_HIDE** (Action: sends event `STOP_T`, sends event `HIDEO`)
-   **CLOSING** (Action: no action)
-   **S_C_SHOW** (Action: `MARK_SHOW`)
-   **S_DONE_SHOW** (Action: sends event `CNF`)
-   **S_DONE** (Action: sends event `CNF`)

**Transitions**

-   START → S_INIT, when `INIT`
-   S_INIT → IDLE, when `1`
-   IDLE → S_INIT, when `INIT`
-   IDLE → S_PREP, when `SHOW`
-   S_PREP → OPENING, when `1`
-   OPENING → S_HIDE, when `SHOWN[xPendingClose]`
-   OPENING → S_REPLACE, when `SHOWN[xPendingShow]`
-   OPENING → S_START_T, when `SHOWN[TIMEOUT_O > T#0s]`
-   OPENING → OPEN, when `SHOWN`
-   OPENING → S_O_SHOW, when `SHOW`
-   OPENING → S_O_CLOSE, when `CLOSE`
-   S_O_SHOW → OPENING, when `1`
-   S_O_CLOSE → OPENING, when `1`
-   S_REPLACE → S_PREP, when `1`
-   S_START_T → OPEN, when `1`
-   OPEN → S_R_OK, when `B_OK`
-   S_R_OK → S_HIDE, when `1`
-   OPEN → S_R_CANCEL, when `B_CANCEL`
-   S_R_CANCEL → S_HIDE, when `1`
-   OPEN → S_R_ABORT, when `B_ABORT`
-   S_R_ABORT → S_HIDE, when `1`
-   OPEN → S_R_RETRY, when `B_RETRY`
-   S_R_RETRY → S_HIDE, when `1`
-   OPEN → S_R_IGNORE, when `B_IGNORE`
-   S_R_IGNORE → S_HIDE, when `1`
-   OPEN → S_R_YES, when `B_YES`
-   S_R_YES → S_HIDE, when `1`
-   OPEN → S_R_NO, when `B_NO`
-   S_R_NO → S_HIDE, when `1`
-   OPEN → S_R_TMO, when `TMO`
-   S_R_TMO → S_HIDE, when `1`
-   OPEN → S_R_CLOSE, when `CLOSE`
-   S_R_CLOSE → S_HIDE, when `1`
-   OPEN → S_REPLACE_OPEN, when `SHOW`
-   S_REPLACE_OPEN → S_PREP, when `1`
-   S_HIDE → CLOSING, when `1`
-   CLOSING → S_DONE_SHOW, when `HIDDEN[xPendingShow]`
-   CLOSING → S_DONE, when `HIDDEN`
-   CLOSING → S_C_SHOW, when `SHOW`
-   S_C_SHOW → CLOSING, when `1`
-   S_DONE_SHOW → S_PREP, when `1`
-   S_DONE → IDLE, when `1`

### Algorithms

**INIT_ALG**

```iecst
QO := QI;
RESULT := 0;
xPendingShow := FALSE;
xPendingClose := FALSE;
```

**PREP**

```iecst
(* Pointer-Ziele aus der Vorlage Workspace_Dialog, IDs siehe DialogIDs.gcf *)
IF MODAL THEN COVER_ID := 14400; ELSE COVER_ID := 65535; END_IF;
CASE ICON OF
  1: ICON_ID := 3402; TITLE_COLOUR := 2;      (* gruen *)
  2: ICON_ID := 3403; TITLE_COLOUR := 15; (* navy *)
  3: ICON_ID := 3404; TITLE_COLOUR := 6;   (* oliv *)
  4: ICON_ID := 3405; TITLE_COLOUR := 12;    (* rot *)
ELSE
  ICON_ID := 65535; TITLE_COLOUR := 2;
END_CASE;
IF TITLE_ID <> 65535 THEN
  TITLE_ID_O := TITLE_ID;
ELSIF ICON >= 1 AND ICON <= 4 THEN
  TITLE_ID_O := UDINT#11409 + ICON;
ELSE
  TITLE_ID_O := 11410;
END_IF;
TEXT_ID_O := TEXT_ID;
(* FOCUS_K: 0 OK, 1 Cancel, 2 Abort, 3 Retry, 4 Ignore, 5 Yes, 6 No, 7 kein Fokus *)
FOCUS_K := 7;
CASE BUTTONS OF
  1: BUTTONS_ID := 3410; FOCUS_K := 0;
  2: BUTTONS_ID := 3411;
     IF DEFAULT_BUTTON = 2 THEN FOCUS_K := 1; ELSE FOCUS_K := 0; END_IF;
  3: BUTTONS_ID := 3412;
     IF DEFAULT_BUTTON = 2 THEN FOCUS_K := 3; ELSIF DEFAULT_BUTTON = 3 THEN FOCUS_K := 4; ELSE FOCUS_K := 2; END_IF;
  4: BUTTONS_ID := 3413;
     IF DEFAULT_BUTTON = 2 THEN FOCUS_K := 6; ELSIF DEFAULT_BUTTON = 3 THEN FOCUS_K := 1; ELSE FOCUS_K := 5; END_IF;
  5: BUTTONS_ID := 3414;
     IF DEFAULT_BUTTON = 2 THEN FOCUS_K := 6; ELSE FOCUS_K := 5; END_IF;
  6: BUTTONS_ID := 3415;
     IF DEFAULT_BUTTON = 2 THEN FOCUS_K := 1; ELSE FOCUS_K := 3; END_IF;
ELSE
  BUTTONS_ID := 65535;
END_CASE;
TIMEOUT_O := TIMEOUT;
AUDIO_O := AUDIO;
xPendingShow := FALSE;
xPendingClose := FALSE;
```

**R_NONE**

```iecst
RESULT := 0;
```

**R_TMO**

```iecst
RESULT := 8;
```

**MARK_SHOW**

```iecst
xPendingShow := TRUE;
```

**MARK_CLOSE**

```iecst
xPendingClose := TRUE;
```

**R_OK**

```iecst
RESULT := 1;
```

**R_CANCEL**

```iecst
RESULT := 2;
```

**R_ABORT**

```iecst
RESULT := 3;
```

**R_RETRY**

```iecst
RESULT := 4;
```

**R_IGNORE**

```iecst
RESULT := 5;
```

**R_YES**

```iecst
RESULT := 6;
```

**R_NO**

```iecst
RESULT := 7;
```

Note: This page was generated from the type definition (`DLG_CTRL.fbt`) and only shows the interface. A detailed description of the behavior is still missing.
