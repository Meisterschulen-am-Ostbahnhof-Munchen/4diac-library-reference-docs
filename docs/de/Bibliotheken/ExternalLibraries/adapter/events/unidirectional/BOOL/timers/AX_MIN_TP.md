# AX_MIN_TP

![AX_MIN_TP_network](./AX_MIN_TP_network.svg)

* * * * * * * * * *

## Einleitung

`AX_MIN_TP` ist ein Mindest-Pulsbreiten-Baustein: `AX_Q` folgt `AX_IN`, ist aber **mindestens `PT` lang**. Kürzere Pulse werden auf `PT` gestreckt, längere Pulse unverändert durchgereicht. Er ist das Gegenstück zu [`AX_TP`](./AX_TP.md), das immer exakt `PT` ausgibt, und zu [`AX_TLIM`](./AX_TLIM.md), das auf maximal `PT` kappt. Anwendungsfall: Der Bunkerboden-Vorlauf im Automatikbetrieb braucht eine Mindestzeit.

**Sicherheitshinweis (Martin Schuder):** Der Baustein selbst hat **keinen Hard-Abbruch**. Ein Endschalter, der den Puls jederzeit sofort unterbrechen soll, muss extern per `AX_AND(AX_Q, NOT Endschalter)` **nach** diesem Baustein verknüpft werden.

![AX_MIN_TP](AX_MIN_TP.svg)

## Verwendete Funktionsbausteine (FBs)

### Sub-Bausteine: AX_MIN_TP

- **Typ**: SubAppType
- **Verwendete interne FBs**:
    - **AX_SPLIT_2_In**: `adapter::events::unidirectional::AX_SPLIT_2` – `AX_IN` wird für `AX_TP_MinZeit.IN` **und** direkt für `AX_OR_Halten` gebraucht
    - **AX_TP_MinZeit**: `adapter::events::unidirectional::timers::AX_TP` – non-retriggerable: läuft nach der steigenden Flanke von `AX_IN` immer exakt `PT` durch, unabhängig davon, ob `AX_IN` vorher schon wieder abfällt
    - **AX_OR_Halten**: `adapter::booleanOperators::AX_OR_2` – `AX_IN OR AX_TP_MinZeit.Q`: hält den Ausgang, solange entweder der rohe Puls noch da ist (längere Pulse) oder die Mindestzeit noch läuft (kürzere Pulse)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **R**: Reset (an `AX_TP_MinZeit.R` durchgereicht)

### **Ereignis-Ausgänge**

Keine – Ereignisse laufen über die Adapter.

### **Daten-Eingänge**

- **PT** (`TIME`): Mindest-Pulsbreite

### **Daten-Ausgänge**

Keine.

### **Adapter**

- **AX_IN** (Socket, `adapter::types::unidirectional::AX`): Roher Eingangspuls
- **AX_Q** (Plug, `adapter::types::unidirectional::AX`): Auf mindestens `PT` gestreckter Ausgang

## Programmablauf und Verbindungen

- `AX_IN` → `AX_SPLIT_2_In.IN`
- `AX_SPLIT_2_In.OUT1` → `AX_TP_MinZeit.IN`
- `AX_SPLIT_2_In.OUT2` → `AX_OR_Halten.IN2`
- `AX_TP_MinZeit.Q` → `AX_OR_Halten.IN1`
- `AX_OR_Halten.OUT` → `AX_Q`
- `PT` → `AX_TP_MinZeit.PT`; `R` → `AX_TP_MinZeit.R`

## Technische Besonderheiten

- Kein Latch und kein `AX_TON` nötig: `AX_TP` ist non-retriggerable, daher gilt `AX_Q = AX_IN OR AX_TP_MinZeit.Q`
- Kürzere Pulse werden gestreckt (`AX_TP.Q` hält bis `PT`), längere bleiben unverändert (`AX_IN` hält `Q` hoch, auch nachdem `AX_TP.Q` abgelaufen ist)
- Version 1.2 (2026-09-29): Vereinfachung gegenüber v1.1 (`ACTIVE`/`AX_TON`/`AX_R_TRIG`-Latch-Konstruktion, verworfen); neuer Ereigniseingang `R`

## Anwendungsszenarien

- Mindestlaufzeit für einen Antrieb (z. B. Bunkerboden-Vorlauf im Automatikbetrieb)
- Entprellen/Verlängern zu kurzer Impulse

## Zusammenfassung

`AX_MIN_TP` gibt einen `AX`-Puls mit mindestens `PT` Länge aus, indem es den Eingang mit einem non-retriggerable `AX_TP` ODER-verknüpft.
