# FIFO_UINT_Adapter

![FIFO_UINT_Adapter](./FIFO_UINT_Adapter.svg)

* * * * * * * * * *

## Einleitung

Beschreibung aus dem Kommentar des Typs: FIFO-Warteschlange fuer UINT-Werte mit frei waehlbarer Kapazitaet - wie
FIFO::FIFO_UINT, aber OHNE eigene DATA/PROCESSED-Events auf der
Konsumentenseite: der Konsument haengt stattdessen per Socket am Plug
OUT_DATA (Typ AFIFO_UINT) an. Die Verarbeitungs-Quittung laeuft ueber den
Rueckkanal des Adapters selbst (PROCESSED), kein separater Wrapper noetig.

Paket: `FIFO_adapter`

## Schnittstellenstruktur

### **Ereignis-Eingänge**

-   **INIT** (Typ: `EInit`): Service Initialization (mit Daten: `QI`, `CAPACITY`, `BUF`)
-   **RST** (Typ: `Event`): Queue leeren und Liefer-Sperre aufheben - Reset der ganzen Kette, der Konsument muss mit zurueckgesetzt werden
-   **PUSH** (Typ: `Event`): IN ans Ende der Queue anhaengen, falls Platz vorhanden (mit Daten: `IN`)

### **Ereignis-Ausgänge**

-   **INITO** (Typ: `EInit`): Initialisierung bestaetigt (mit Daten: `QO`, `BUF`)
-   **RSTO** (Typ: `Event`): Reset bestaetigt (mit Daten: `N`)
-   **PUSHO** (Typ: `Event`): PUSH bestaetigt (OK=FALSE wenn Queue voll war oder QO=FALSE) (mit Daten: `N`, `OK`)

### **Daten-Eingänge**

-   **QI** (Typ: `BOOL`): Event Input Qualifier
-   **CAPACITY** (Typ: `UINT`): Kapazitaet - MUSS der Anzahl Elemente von BUF entsprechen, daher immer ueber F_LEN_ARRAY hinter der PROVIDE_ARR_x_UINT-Instanz ermitteln. Ein groesserer Wert schreibt ueber BUF hinaus. 0 ergibt QO=FALSE.
-   **IN** (Typ: `UINT`): Zu pushender Wert

### **Daten-Ausgänge**

-   **QO** (Typ: `BOOL`): Event Output Qualifier
-   **N** (Typ: `UINT`): Anzahl aktuell wartender (noch nicht ausgelieferter) Elemente
-   **OK** (Typ: `BOOL`): TRUE wenn der letzte PUSH erfolgreich war (Queue war nicht voll und QO=TRUE)

### **Adapter (Sockets)**

Keine.

### **Adapter (Plugs)**

-   **OUT_DATA** (Typ: `FIFO_adapter::AFIFO_UINT`): Konsument haengt per Socket hier an - DATA liefert den naechsten Wert, PROCESSED (Rueckkanal) quittiert

## Funktionsweise (aus der Quelle)

Diese Beschreibung ist aus der Typdefinition abgeleitet und gibt Zustände, Übergänge und Algorithmen wieder. Sie wurde nicht auf Hardware getestet.

### Ablaufsteuerung (ECC)

-   **START** (Aktion: keine Aktion)
-   **S_INIT** (Aktion: `INIT_ALG` → sendet Ereignis `INITO`)
-   **S_RST** (Aktion: `RST` → sendet Ereignis `RSTO`)
-   **S_PUSH** (Aktion: `PUSH` → sendet Ereignis `PUSHO`)
-   **S_PROCESSED** (Aktion: `PROCESSED_ALG`)
-   **S_DELIVER** (Aktion: `DELIVER` → sendet Ereignis `OUT_DATA.DATA`)

**Übergänge**

-   START → S_INIT, wenn `INIT`
-   START → S_RST, wenn `RST`
-   START → S_PUSH, wenn `PUSH`
-   START → S_PROCESSED, wenn `OUT_DATA.PROCESSED[DELIVERING]`
-   S_INIT → START, wenn `1`
-   S_RST → START, wenn `1`
-   S_PUSH → S_DELIVER, wenn `OK AND NOT DELIVERING AND N>0`
-   S_PUSH → START, wenn `1`
-   S_PROCESSED → S_DELIVER, wenn `N>0`
-   S_PROCESSED → START, wenn `1`
-   S_DELIVER → START, wenn `1`

### Algorithmen

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

Hinweis: Diese Seite wurde aus der Typdefinition (`FIFO_UINT_Adapter.fbt`) erzeugt und gibt nur die Schnittstelle wieder. Eine ausführliche Beschreibung des Verhaltens fehlt noch.
