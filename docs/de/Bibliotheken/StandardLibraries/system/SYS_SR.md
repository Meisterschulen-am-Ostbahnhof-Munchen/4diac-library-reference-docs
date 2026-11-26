# SYS_SR

![SYS_SR](./SYS_SR.svg)

* * * * * * * * * *

## Einleitung

Beschreibung aus dem Kommentar des Typs: Event-driven bistable

Paket: `iec61499::system`

## Schnittstellenstruktur

### **Ereignis-Eingänge**

-   **S** (Typ: `Event`): Set output Q
-   **R** (Typ: `Event`): Reset output Q

### **Ereignis-Ausgänge**

-   **EO** (Typ: `Event`): Output Q has changed (mit Daten: `Q`)

### **Daten-Eingänge**

Keine.

### **Daten-Ausgänge**

-   **Q** (Typ: `BOOL`): value of flip flop

### **Adapter (Sockets)**

Keine.

### **Adapter (Plugs)**

Keine.

## Funktionsweise (aus der Quelle)

Diese Beschreibung ist aus der Typdefinition abgeleitet und gibt Zustände, Übergänge und Algorithmen wieder. Sie wurde nicht auf Hardware getestet.

### Ablaufsteuerung (ECC)

-   **START** (Aktion: keine Aktion)
-   **SET** (Aktion: `SET` → sendet Ereignis `EO`)
-   **RESET** (Aktion: `RESET` → sendet Ereignis `EO`)

**Übergänge**

-   START → SET, wenn `S`
-   SET → RESET, wenn `R`
-   RESET → SET, wenn `S`

### Algorithmen

**SET**: SET algorithm

```iecst
Q := TRUE;
```

**RESET**: RESET algorithm

```iecst
Q := FALSE;
```

Hinweis: Diese Seite wurde aus der Typdefinition (`SYS_SR.fbt`) erzeugt und gibt nur die Schnittstelle wieder. Eine ausführliche Beschreibung des Verhaltens fehlt noch.
