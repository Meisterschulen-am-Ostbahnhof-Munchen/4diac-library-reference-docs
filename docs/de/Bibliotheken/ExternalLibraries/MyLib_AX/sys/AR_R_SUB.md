# AR_R_SUB

![AR_R_SUB_network](./AR_R_SUB_network.svg)

* * * * * * * * * *

## Einleitung

`AR_R_SUB` ist eine SubApp, die von einem `AR`-Wert einen festen `REAL`-Subtrahenden abzieht (`OUT = IN - SUBTRAHEND`). Der Subtrahend wird als Parameter bei der Instanziierung eingebacken, sodass kein zweiter `AR`-Socket verdrahtet werden muss. Sie ist das Subtraktions-Gegenstück zu [`AR_R_MUL`](./AR_R_MUL.md).

![AR_R_SUB](AR_R_SUB.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Keine.

### **Ereignis-Ausgänge**

Keine.

### **Daten-Eingänge**

- **SUBTRAHEND** (`REAL`): Fester Subtrahend, über Parameter bei der Instanziierung gesetzt. Initialwert `REAL#0.0`.

### **Daten-Ausgänge**

Keine.

### **Adapter**

- **IN** (Socket, `adapter::types::unidirectional::AR`): Minuend-Wert
- **OUT** (Plug, `adapter::types::unidirectional::AR`): `IN - SUBTRAHEND`

## Verwendete Funktionsbausteine (FBs)

- **`initval_AR_SUBTRAHEND`** (`adapter::types::unidirectional::AR::initval::initval_AR`): erzeugt aus `SUBTRAHEND` einen konstanten `AR`-Wert
- **`AR_SUB_2`** (`adapter::iec61131::arithmetic::AR_SUB_2`): subtrahiert zwei `AR`-Werte

## Funktionsweise

- `SUBTRAHEND` → `initval_AR_SUBTRAHEND.INIT_VAL`
- `IN` → `AR_SUB_2.IN1` (Minuend)
- `initval_AR_SUBTRAHEND.OUT` → `AR_SUB_2.IN2` (Subtrahend)
- `AR_SUB_2.OUT` → `OUT`

Der Baustein besitzt keine eigenen Ereignisse; die Berechnung folgt dem Datenfluss des `AR`-Adapters.

## Technische Besonderheiten

- Kein zweiter `AR`-Socket nötig, der Subtrahend ist fest in die Instanz eingebacken
- Keine Laufzeitänderung des Subtrahenden vorgesehen
- Reine Verdrahtung vorhandener Bausteine, keine eigene Logik

## Anwendungsszenarien

- Konstanter Offset-Abzug von einem skalierten oder physikalischen `AR`-Wert

## Zusammenfassung

`AR_R_SUB` subtrahiert einen fest parametrierten `REAL`-Wert von einem `AR`-Signal.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
