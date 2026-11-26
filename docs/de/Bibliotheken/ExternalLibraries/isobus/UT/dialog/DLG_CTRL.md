# DLG_CTRL

![DLG_CTRL](./DLG_CTRL.svg)

* * * * * * * * * *

## Einleitung

Beschreibung aus dem Kommentar des Typs: Ablaufsteuerung eines VT-Dialogs (MessageBox)

Paket: `isobus::UT::dialog`

## Schnittstellenstruktur

### **Ereignis-Eingänge**

-   **INIT** (Typ: `EInit`): Initialisierung (mit Daten: `QI`)
-   **SHOW** (Typ: `Event`): Dialog zeigen (mit Daten: `TEXT_ID`, `TITLE_ID`, `ICON`, `BUTTONS`, `DEFAULT_BUTTON`, `MODAL`, `TIMEOUT`, `AUDIO`)
-   **CLOSE** (Typ: `Event`): Dialog vom Programm schliessen
-   **SHOWN** (Typ: `Event`): Anzeige-Kette fertig
-   **HIDDEN** (Typ: `Event`): Ausblenden fertig
-   **TMO** (Typ: `Event`): Zeit abgelaufen
-   **B_OK** (Typ: `Event`): Button OK losgelassen
-   **B_CANCEL** (Typ: `Event`): Button Cancel losgelassen
-   **B_ABORT** (Typ: `Event`): Button Abort losgelassen
-   **B_RETRY** (Typ: `Event`): Button Retry losgelassen
-   **B_IGNORE** (Typ: `Event`): Button Ignore losgelassen
-   **B_YES** (Typ: `Event`): Button Yes losgelassen
-   **B_NO** (Typ: `Event`): Button No losgelassen

### **Ereignis-Ausgänge**

-   **INITO** (Typ: `EInit`): Initialisierung fertig (mit Daten: `QO`)
-   **SHOWO** (Typ: `Event`): Anzeige-Kette starten (mit Daten: `COVER_ID`, `ICON_ID`, `TITLE_ID_O`, `TEXT_ID_O`, `BUTTONS_ID`, `TITLE_COLOUR`, `FOCUS_K`, `AUDIO_O`)
-   **HIDEO** (Typ: `Event`): Dialog ausblenden
-   **START_T** (Typ: `Event`): Timeout starten (mit Daten: `TIMEOUT_O`)
-   **STOP_T** (Typ: `Event`): Timeout stoppen
-   **CNF** (Typ: `Event`): Dialog beendet (mit Daten: `QO`, `RESULT`)

### **Daten-Eingänge**

-   **QI** (Typ: `BOOL`): Freigabe (Vorgabewert: `TRUE`)
-   **TEXT_ID** (Typ: `UINT`): OutputString Meldungstext (Vorgabewert: `UINT#65535`)
-   **TITLE_ID** (Typ: `UINT`): OutputString Titel, NULL = Standard je Icon (Vorgabewert: `UINT#65535`)
-   **ICON** (Typ: `USINT`): DLG_ICON_* (Vorgabewert: `USINT#1`)
-   **BUTTONS** (Typ: `USINT`): DLG_BTN_* (Vorgabewert: `USINT#1`)
-   **DEFAULT_BUTTON** (Typ: `USINT`): Fokus auf Taste 1..3 (Vorgabewert: `USINT#1`)
-   **MODAL** (Typ: `BOOL`): Maske abdecken (Vorgabewert: `FALSE`)
-   **TIMEOUT** (Typ: `TIME`): T#0s = kein Timeout (Vorgabewert: `T#0s`)
-   **AUDIO** (Typ: `BOOL`): Ton beim Oeffnen (Vorgabewert: `FALSE`)

### **Daten-Ausgänge**

-   **QO** (Typ: `BOOL`): Qualitaet
-   **RESULT** (Typ: `USINT`): DLG_RES_*
-   **COVER_ID** (Typ: `UDINT`): Ziel ObjectPointer_Dlg_Cover
-   **ICON_ID** (Typ: `UDINT`): Ziel ObjectPointer_Dlg_Icon
-   **TITLE_ID_O** (Typ: `UDINT`): Ziel ObjectPointer_Dlg_Title
-   **TEXT_ID_O** (Typ: `UDINT`): Ziel ObjectPointer_Dlg_Text
-   **BUTTONS_ID** (Typ: `UDINT`): Ziel ObjectPointer_Dlg_Buttons
-   **TITLE_COLOUR** (Typ: `USINT`): VT-Farbindex Titelzeile
-   **FOCUS_K** (Typ: `UINT`): Fokus-Button 0..6, 7 = keiner
-   **AUDIO_O** (Typ: `BOOL`): Ton ja/nein
-   **TIMEOUT_O** (Typ: `TIME`): Timeout

### **Adapter (Sockets)**

Keine.

### **Adapter (Plugs)**

Keine.

## Funktionsweise (aus der Quelle)

Diese Beschreibung ist aus der Typdefinition abgeleitet und gibt Zustände, Übergänge und Algorithmen wieder. Sie wurde nicht auf Hardware getestet.

### Ablaufsteuerung (ECC)

-   **START** (Aktion: keine Aktion)
-   **S_INIT** (Aktion: `INIT_ALG` → sendet Ereignis `INITO`)
-   **IDLE** (Aktion: keine Aktion)
-   **S_PREP** (Aktion: `PREP` → sendet Ereignis `SHOWO`)
-   **OPENING** (Aktion: keine Aktion)
-   **S_O_SHOW** (Aktion: `MARK_SHOW`)
-   **S_O_CLOSE** (Aktion: `R_NONE`, `MARK_CLOSE`)
-   **S_REPLACE** (Aktion: `R_NONE` → sendet Ereignis `CNF`)
-   **S_START_T** (Aktion: sendet Ereignis `START_T`)
-   **OPEN** (Aktion: keine Aktion)
-   **S_R_OK** (Aktion: `R_OK`)
-   **S_R_CANCEL** (Aktion: `R_CANCEL`)
-   **S_R_ABORT** (Aktion: `R_ABORT`)
-   **S_R_RETRY** (Aktion: `R_RETRY`)
-   **S_R_IGNORE** (Aktion: `R_IGNORE`)
-   **S_R_YES** (Aktion: `R_YES`)
-   **S_R_NO** (Aktion: `R_NO`)
-   **S_R_TMO** (Aktion: `R_TMO`)
-   **S_R_CLOSE** (Aktion: `R_NONE`)
-   **S_REPLACE_OPEN** (Aktion: `R_NONE` → sendet Ereignis `STOP_T`, sendet Ereignis `CNF`)
-   **S_HIDE** (Aktion: sendet Ereignis `STOP_T`, sendet Ereignis `HIDEO`)
-   **CLOSING** (Aktion: keine Aktion)
-   **S_C_SHOW** (Aktion: `MARK_SHOW`)
-   **S_DONE_SHOW** (Aktion: sendet Ereignis `CNF`)
-   **S_DONE** (Aktion: sendet Ereignis `CNF`)

**Übergänge**

-   START → S_INIT, wenn `INIT`
-   S_INIT → IDLE, wenn `1`
-   IDLE → S_INIT, wenn `INIT`
-   IDLE → S_PREP, wenn `SHOW`
-   S_PREP → OPENING, wenn `1`
-   OPENING → S_HIDE, wenn `SHOWN[xPendingClose]`
-   OPENING → S_REPLACE, wenn `SHOWN[xPendingShow]`
-   OPENING → S_START_T, wenn `SHOWN[TIMEOUT_O > T#0s]`
-   OPENING → OPEN, wenn `SHOWN`
-   OPENING → S_O_SHOW, wenn `SHOW`
-   OPENING → S_O_CLOSE, wenn `CLOSE`
-   S_O_SHOW → OPENING, wenn `1`
-   S_O_CLOSE → OPENING, wenn `1`
-   S_REPLACE → S_PREP, wenn `1`
-   S_START_T → OPEN, wenn `1`
-   OPEN → S_R_OK, wenn `B_OK`
-   S_R_OK → S_HIDE, wenn `1`
-   OPEN → S_R_CANCEL, wenn `B_CANCEL`
-   S_R_CANCEL → S_HIDE, wenn `1`
-   OPEN → S_R_ABORT, wenn `B_ABORT`
-   S_R_ABORT → S_HIDE, wenn `1`
-   OPEN → S_R_RETRY, wenn `B_RETRY`
-   S_R_RETRY → S_HIDE, wenn `1`
-   OPEN → S_R_IGNORE, wenn `B_IGNORE`
-   S_R_IGNORE → S_HIDE, wenn `1`
-   OPEN → S_R_YES, wenn `B_YES`
-   S_R_YES → S_HIDE, wenn `1`
-   OPEN → S_R_NO, wenn `B_NO`
-   S_R_NO → S_HIDE, wenn `1`
-   OPEN → S_R_TMO, wenn `TMO`
-   S_R_TMO → S_HIDE, wenn `1`
-   OPEN → S_R_CLOSE, wenn `CLOSE`
-   S_R_CLOSE → S_HIDE, wenn `1`
-   OPEN → S_REPLACE_OPEN, wenn `SHOW`
-   S_REPLACE_OPEN → S_PREP, wenn `1`
-   S_HIDE → CLOSING, wenn `1`
-   CLOSING → S_DONE_SHOW, wenn `HIDDEN[xPendingShow]`
-   CLOSING → S_DONE, wenn `HIDDEN`
-   CLOSING → S_C_SHOW, wenn `SHOW`
-   S_C_SHOW → CLOSING, wenn `1`
-   S_DONE_SHOW → S_PREP, wenn `1`
-   S_DONE → IDLE, wenn `1`

### Algorithmen

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

Hinweis: Diese Seite wurde aus der Typdefinition (`DLG_CTRL.fbt`) erzeugt und gibt nur die Schnittstelle wieder. Eine ausführliche Beschreibung des Verhaltens fehlt noch.
