# R_AR_SUB

![R_AR_SUB_network](./R_AR_SUB_network.svg)

* * * * * * * * * *

## Einleitung

`R_AR_SUB` subtrahiert einen `AR`-Wert von einem festen `REAL`-Minuenden: `OUT = MINUEND - IN`. Ein zweiter `AR`-Socket ist nicht nötig, da der Minuend per Parameter bei der Instanziierung eingebacken wird.

![R_AR_SUB](R_AR_SUB.svg)

## Verwendete Funktionsbausteine (FBs)

### Sub-Bausteine: R_AR_SUB

- **Typ**: SubAppType
- **Verwendete interne FBs**:
    - **initval_AR_MINUEND**: `adapter::types::unidirectional::AR::initval::initval_AR` – wandelt den Parameter `MINUEND` in ein `AR`-Signal
    - **AR_SUB_2**: `adapter::iec61131::arithmetic::AR_SUB_2` – bildet die Differenz `IN1 - IN2`

## Schnittstellenstruktur

### **Ereignis-Eingänge / Ereignis-Ausgänge**

Keine – Ereignisse laufen über die Adapter.

### **Daten-Eingänge**

- **MINUEND** (`REAL`, Initialwert `0.0`): Fester Minuend, über Parameter bei der Instanziierung gesetzt

### **Daten-Ausgänge**

Keine.

### **Adapter**

- **IN** (Socket): `adapter::types::unidirectional::AR` – zu subtrahierender Subtrahend
- **OUT** (Plug): `adapter::types::unidirectional::AR` – `MINUEND - IN`

## Programmablauf und Verbindungen

- `MINUEND` → `initval_AR_MINUEND.INIT_VAL`
- `initval_AR_MINUEND.OUT` → `AR_SUB_2.IN1`
- `IN` → `AR_SUB_2.IN2`
- `AR_SUB_2.OUT` → `OUT`

## Technische Besonderheiten

- Der Minuend steht fest als Parameter; es wird kein zweiter `AR`-Eingang benötigt
- Version 1.0 (2026-09-27)

## Anwendungsszenarien

- Invertierung relativ zu einer festen Größe, z. B. `100 - Wert`

## Zusammenfassung

`R_AR_SUB` liefert `MINUEND - IN` für einen `AR`-Wert mit einem per Parameter festgelegten Minuenden.
