# logiBUS_IXA_TO_logiBUS_QXA

![logiBUS_IXA_TO_logiBUS_QXA_network](./logiBUS_IXA_TO_logiBUS_QXA_network.svg)

* * * * * * * * * *

## Einleitung

`logiBUS_IXA_TO_logiBUS_QXA` verdrahtet einen physischen digitalen Eingang (`logiBUS_IXA`) direkt auf einen physischen digitalen Ausgang (`logiBUS_QXA`) — eine reine Hardware-Durchschaltung ohne VT-Beteiligung, adapterbasiert (azyklisch mit Bestätigung, `QI` kommt vom Eingang `QI`). Für die ereignisgesteuerte Variante ohne Adapter siehe [`logiBUS_IX_TO_logiBUS_QX`](./logiBUS_IX_TO_logiBUS_QX.md).

![logiBUS_IXA_TO_logiBUS_QXA](logiBUS_IXA_TO_logiBUS_QXA.svg)

## Verwendete Funktionsbausteine (FBs)

### Sub-Bausteine: logiBUS_IXA_TO_logiBUS_QXA

- **Typ**: SubAppType
- **Verwendete interne FBs**:
    - **logiBUS_IXA**: `logiBUS::io::DI::logiBUS_IXA` — physischer digitaler Eingang, Adapter-Ausgang `IN`, `QI` kommt vom Eingang `QI`.
    - **logiBUS_QXA**: `logiBUS::io::DQ::logiBUS_QXA` — physischer digitaler Ausgang, Adapter-Eingang `OUT`, `QI` kommt vom Eingang `QI`.
    - **AND_QO**: `iec61131::booleanOperators::AND_BOOL_2` — verknüpft `logiBUS_QXA.QO` und `logiBUS_IXA.QO` zum Ausgang `QO`.
- **Funktionsweise**: Der Adapter-Ausgang des Eingangs wird direkt auf den Adapter-Eingang des Ausgangs verdrahtet — keine Zwischenlogik, keine VT-Anbindung. Die SubApp-Schnittstelle besitzt außerdem die Ereignisse `INIT`/`INITO` sowie `QI` (BOOL, Initialwert `TRUE`) und `QO` (BOOL).

## Programmablauf und Verbindungen

1. `Input` → `logiBUS_IXA.Input`; `Output` → `logiBUS_QXA.Output`.
2. `logiBUS_IXA.IN` (Adapter) → `logiBUS_QXA.OUT` (Adapter) — direkte Durchschaltung.
3. Initialisierungskette: `INIT` → `logiBUS_QXA.INIT`, `logiBUS_QXA.INITO` → `logiBUS_IXA.INIT`, `logiBUS_IXA.INITO` → `INITO` und `AND_QO.REQ` (zusätzlich `logiBUS_IXA.INITO` → `logiBUS_IXA.REQ`).
4. Qualität: `QI` → `logiBUS_QXA.QI` und `logiBUS_IXA.QI`; `logiBUS_QXA.QO` → `AND_QO.IN1`; `logiBUS_IXA.QO` → `AND_QO.IN2`; `AND_QO.OUT` → `QO`.

## Anwendungsszenarien

- Reine Hardware-zu-Hardware-Verdrahtung, z. B. für einen physischen Notaus-Kontakt oder eine feste Verriegelung, die unabhängig vom VT funktionieren muss.

## Zusammenfassung

Adapterbasierte Direktverdrahtung eines physischen Eingangs auf einen physischen Ausgang, ohne VT- oder Ereignislogik.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
