# Button_IXA_TO_logiBUS_QXA

![Button_IXA_TO_logiBUS_QXA_network](./Button_IXA_TO_logiBUS_QXA_network.svg)

* * * * * * * * * *

## Einleitung

`Button_IXA_TO_logiBUS_QXA` verbindet einen VT-Taster (`Button_IXA`) direkt mit einem physischen digitalen Ausgang (`logiBUS_QXA`) — die einfachste Form eines VT-schaltbaren Ausgangs, ohne Statusanzeige und ohne OPC-UA. Für die Variante mit VT-Statusfarbe siehe [`Button_IXA_TO_logiBUS_QXA_BG`](./Button_IXA_TO_logiBUS_QXA_BG.md).

![Button_IXA_TO_logiBUS_QXA](Button_IXA_TO_logiBUS_QXA.svg)

## Verwendete Funktionsbausteine (FBs)

### Sub-Bausteine: Button_IXA_TO_logiBUS_QXA

- **Typ**: SubAppType
- **Verwendete interne FBs**:
    - **Button_IXA**: `isobus::UT::io::Button::Button_IXA` — VT-Taster-Adapter, `QI` kommt vom Eingang `QI`, `u16ObjId` identifiziert die VT-Taste.
    - **logiBUS_QXA**: `logiBUS::io::DQ::logiBUS_QXA` — physischer digitaler Ausgang, `QI` kommt vom Eingang `QI`.
    - **AND_QO**: `iec61131::booleanOperators::AND_BOOL_2` — verknüpft `logiBUS_QXA.QO` und `Button_IXA.QO` zum Ausgang `QO`.
- **Funktionsweise**: Der Adapter-Ausgang des Tasters wird direkt auf den Adapter-Eingang des physischen Ausgangs verdrahtet — keine Zwischenlogik. Zusätzlich hat die SubApp die Schnittstelle `INIT`/`INITO` (Ereignisse) und `QI`/`QO` (BOOL): `INIT` startet die Init-Kette, `QI` (Initialwert `TRUE`) wird an beide FBs durchgereicht, `QO` ist die UND-Verknüpfung ihrer `QO`-Ausgänge.

## Programmablauf und Verbindungen

1. `u16ObjId` → `Button_IXA.u16ObjId`; `Output` → `logiBUS_QXA.Output`.
2. `Button_IXA.IN` (Adapter) → `logiBUS_QXA.OUT` (Adapter) — direkte Durchschaltung.
3. `INIT` → `logiBUS_QXA.INIT`; `logiBUS_QXA.INITO` → `Button_IXA.INIT`; `Button_IXA.INITO` → `INITO` und `AND_QO.REQ`.
4. `QI` → `logiBUS_QXA.QI` und `Button_IXA.QI`.
5. `logiBUS_QXA.QO` → `AND_QO.IN1`; `Button_IXA.QO` → `AND_QO.IN2`; `AND_QO.OUT` → `QO`.

## Anwendungsszenarien

- Minimaler VT-Taster-zu-Ausgang-Baustein für Übungen, die noch keine Statusanzeige oder Fernbedienung benötigen.

## Zusammenfassung

Die einfachste Bausteinvariante dieser Familie: ein VT-Taster direkt auf einen physischen Ausgang, ohne Zusatzfunktion.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
