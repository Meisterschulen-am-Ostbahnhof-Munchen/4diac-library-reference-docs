# FIFO_UINT_Adapter

![FIFO_UINT_Adapter](./FIFO_UINT_Adapter.svg)

* * * * * * * * * *

## Introduction

Description from the comment of the type: FIFO-Warteschlange fuer UINT-Werte mit frei waehlbarer Kapazitaet - wie
FIFO::FIFO_UINT, aber OHNE eigene DATA/PROCESSED-Events auf der
Konsumentenseite: der Konsument haengt stattdessen per Socket am Plug
OUT_DATA (Typ AFIFO_UINT) an. Die Verarbeitungs-Quittung laeuft ueber den
Rueckkanal des Adapters selbst (PROCESSED), kein separater Wrapper noetig.

Package: `FIFO_adapter`

## Interface structure

### **Event inputs**

-   **INIT** (Type: `EInit`): Service Initialization (with data: `QI`, `CAPACITY`, `BUF`)
-   **RST** (Type: `Event`): Queue leeren und Liefer-Sperre aufheben - Reset der ganzen Kette, der Konsument muss mit zurueckgesetzt werden
-   **PUSH** (Type: `Event`): IN ans Ende der Queue anhaengen, falls Platz vorhanden (with data: `IN`)

### **Event outputs**

-   **INITO** (Type: `EInit`): Initialisierung bestaetigt (with data: `QO`, `BUF`)
-   **RSTO** (Type: `Event`): Reset bestaetigt (with data: `N`)
-   **PUSHO** (Type: `Event`): PUSH bestaetigt (OK=FALSE wenn Queue voll war oder QO=FALSE) (with data: `N`, `OK`)

### **Data inputs**

-   **QI** (Type: `BOOL`): Event Input Qualifier
-   **CAPACITY** (Type: `UINT`): Kapazitaet - MUSS der Anzahl Elemente von BUF entsprechen, daher immer ueber F_LEN_ARRAY hinter der PROVIDE_ARR_x_UINT-Instanz ermitteln. Ein groesserer Wert schreibt ueber BUF hinaus. 0 ergibt QO=FALSE.
-   **IN** (Type: `UINT`): Zu pushender Wert

### **Data outputs**

-   **QO** (Type: `BOOL`): Event Output Qualifier
-   **N** (Type: `UINT`): Anzahl aktuell wartender (noch nicht ausgelieferter) Elemente
-   **OK** (Type: `BOOL`): TRUE wenn der letzte PUSH erfolgreich war (Queue war nicht voll und QO=TRUE)

### **Adapters (sockets)**

None.

### **Adapters (plugs)**

-   **OUT_DATA** (Type: `FIFO_adapter::AFIFO_UINT`): Konsument haengt per Socket hier an - DATA liefert den naechsten Wert, PROCESSED (Rueckkanal) quittiert

## Operation (from the source)

This description is derived from the type definition and shows states, transitions and algorithms. It has not been tested on hardware.

### Execution control chart (ECC)

-   **START** (Action: no action)
-   **S_INIT** (Action: `INIT_ALG` → sends event `INITO`)
-   **S_RST** (Action: `RST` → sends event `RSTO`)
-   **S_PUSH** (Action: `PUSH` → sends event `PUSHO`)
-   **S_PROCESSED** (Action: `PROCESSED_ALG`)
-   **S_DELIVER** (Action: `DELIVER` → sends event `OUT_DATA.DATA`)

**Transitions**

-   START → S_INIT, when `INIT`
-   START → S_RST, when `RST`
-   START → S_PUSH, when `PUSH`
-   START → S_PROCESSED, when `OUT_DATA.PROCESSED[DELIVERING]`
-   S_INIT → START, when `1`
-   S_RST → START, when `1`
-   S_PUSH → S_DELIVER, when `OK AND NOT DELIVERING AND N>0`
-   S_PUSH → START, when `1`
-   S_PROCESSED → S_DELIVER, when `N>0`
-   S_PROCESSED → START, when `1`
-   S_DELIVER → START, when `1`

### Algorithms

**INIT_ALG**: Queue leeren und QI/QO durchreichen

```iecst
PTR := 0;
OUTPTR := 0;
N := 0;
DELIVERING := FALSE;
OK := FALSE;
QO := QI AND CAPACITY > 0;
```

**RST**: Queue und Liefer-Sperre leeren - RST setzt die ganze Kette zurueck, der Konsument muss mit zurueckgesetzt werden

```iecst
PTR := 0;
OUTPTR := 0;
N := 0;
DELIVERING := FALSE;
OK := FALSE;
```

**PUSH**: Wert anhaengen, falls Platz vorhanden

```iecst
IF NOT QO OR N >= CAPACITY THEN
    OK := FALSE;
ELSE
    OK := TRUE;
    BUF[PTR] := IN;
    PTR := (PTR + 1) MOD CAPACITY;
    N := N + 1;
END_IF;
```

**PROCESSED_ALG**: Liefer-Sperre aufheben - das naechste DATA folgt ueber die Transition nach S_DELIVER, falls die Queue noch Elemente enthaelt

```iecst
DELIVERING := FALSE;
```

**DELIVER**: Vordersten Wert entnehmen und ueber den Plug liefern

```iecst
OUT_DATA.OUT := BUF[OUTPTR];
OUTPTR := (OUTPTR + 1) MOD CAPACITY;
N := N - 1;
OUT_DATA.N := N;
DELIVERING := TRUE;
```

Note: This page was generated from the type definition (`FIFO_UINT_Adapter.fbt`) and only shows the interface. A detailed description of the behavior is still missing.
