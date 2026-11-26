# E_MUX_FIFO_2_Adapter

![E_MUX_FIFO_2_Adapter](./E_MUX_FIFO_2_Adapter.svg)

* * * * * * * * * *

## Introduction

Description from the comment of the type: Adapter-Variante von FIFO::E_MUX_FIFO_2: merkt sich bei EI1/EI2 den Kanal
(0/1) in einer FIFO und liefert ihn ueber den Plug OUT_DATA (AFIFO_UINT, Kanal in
OUT) einzeln aus. PROCESSED kommt ueber den Rueckkanal. Gegenstueck: E_DEMUX_FIFO_2.

Package: `FIFO_adapter`

## Interface structure

### **Event inputs**

-   **INIT** (Type: `EInit`): Service Initialization (with data: `QI`, `CAPACITY`, `BUF`)
-   **RST** (Type: `Event`): Queue leeren und Liefer-Sperre aufheben - Reset der ganzen Kette, der Konsument muss mit zurueckgesetzt werden
-   **EI1** (Type: `Event`): Kanal 1 (0) ans Ende der Queue anhaengen, falls Platz vorhanden
-   **EI2** (Type: `Event`): Kanal 2 (1) ans Ende der Queue anhaengen, falls Platz vorhanden

### **Event outputs**

-   **INITO** (Type: `EInit`): Initialisierung bestaetigt (with data: `QO`, `BUF`)
-   **RSTO** (Type: `Event`): Reset bestaetigt (with data: `N`)
-   **PUSHO** (Type: `Event`): EI1/EI2 bestaetigt (OK=FALSE wenn Queue voll war oder QO=FALSE) (with data: `N`, `OK`)

### **Data inputs**

-   **QI** (Type: `BOOL`): Event Input Qualifier
-   **CAPACITY** (Type: `UINT`): Kapazitaet - MUSS der Anzahl Elemente von BUF entsprechen, daher immer ueber F_LEN_ARRAY hinter der PROVIDE_ARR_x_UINT-Instanz ermitteln. Ein groesserer Wert schreibt ueber BUF hinaus. 0 ergibt QO=FALSE.

### **Data outputs**

-   **QO** (Type: `BOOL`): Event Output Qualifier
-   **N** (Type: `UINT`): Anzahl aktuell wartender (noch nicht ausgelieferter) Elemente
-   **OK** (Type: `BOOL`): TRUE wenn das letzte EI1/EI2 erfolgreich war (Queue war nicht voll und QO=TRUE)

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
-   **S_EI1** (Action: `SEL_K0`, `PUSH` → sends event `PUSHO`)
-   **S_EI2** (Action: `SEL_K1`, `PUSH` → sends event `PUSHO`)
-   **S_PROCESSED** (Action: `PROCESSED_ALG`)
-   **S_DELIVER** (Action: `DELIVER` → sends event `OUT_DATA.DATA`)

**Transitions**

-   START → S_INIT, when `INIT`
-   START → S_RST, when `RST`
-   START → S_EI1, when `EI1`
-   START → S_EI2, when `EI2`
-   START → S_PROCESSED, when `OUT_DATA.PROCESSED[DELIVERING]`
-   S_INIT → START, when `1`
-   S_RST → START, when `1`
-   S_EI1 → S_DELIVER, when `OK AND NOT DELIVERING AND N>0`
-   S_EI1 → START, when `1`
-   S_EI2 → S_DELIVER, when `OK AND NOT DELIVERING AND N>0`
-   S_EI2 → START, when `1`
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

**SEL_K0**: Kanal 1

```iecst
KIN := 0;
```

**SEL_K1**: Kanal 2

```iecst
KIN := 1;
```

**PUSH**: Kanal anhaengen, falls Platz vorhanden

```iecst
IF NOT QO OR N >= CAPACITY THEN
    OK := FALSE;
ELSE
    OK := TRUE;
    BUF[PTR] := KIN;
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

Note: This page was generated from the type definition (`E_MUX_FIFO_2_Adapter.fbt`) and only shows the interface. A detailed description of the behavior is still missing.
