# Softkey_IX_TO_logiBUS_QX

![Softkey_IX_TO_logiBUS_QX_network](./Softkey_IX_TO_logiBUS_QX_network.svg)

* * * * * * * * * *

## Einleitung

`Softkey_IX_TO_logiBUS_QX` ist das test_B-Gegenstück zu [`Softkey_IXA_TO_logiBUS_QXA`](../../MyLib_AX/sys/Softkey_IXA_TO_logiBUS_QXA.md) — ein VT-Softkey (`Softkey_IX`) schaltet direkt einen physischen Ausgang (`logiBUS_QX`), ohne Adapter.

![Softkey_IX_TO_logiBUS_QX](Softkey_IX_TO_logiBUS_QX.svg)

## Verwendete Funktionsbausteine (FBs)

### Sub-Bausteine: Softkey_IX_TO_logiBUS_QX

- **Typ**: SubAppType
- **Verwendete interne FBs**:
    - **Softkey_IX**: `isobus::UT::io::Softkey::Softkey_IX` — VT-Softkey (Nicht-Adapter-Variante).
    - **logiBUS_QX**: `logiBUS::io::DQ::logiBUS_QX` — physischer digitaler Ausgang.
    - **AND_QO**: `iec61131::booleanOperators::AND_BOOL_2` — verknüpft `logiBUS_QX.QO` und `Softkey_IX.QO` zum Ausgang `QO`.
- **Funktionsweise**: `Softkey_IX.IND` löst `logiBUS_QX.REQ` aus; `Softkey_IX.IN` wird direkt auf `logiBUS_QX.OUT` verdrahtet. Die SubApp-Schnittstelle besitzt außerdem die Ereignisse `INIT`/`INITO` sowie `QI` (BOOL, Initialwert `TRUE`) und `QO` (BOOL).

## Programmablauf und Verbindungen

1. `u16ObjId` → `Softkey_IX.u16ObjId`; `Output` → `logiBUS_QX.Output`.
2. `Softkey_IX.IND` → `logiBUS_QX.REQ`; `Softkey_IX.IN` → `logiBUS_QX.OUT`.
3. Initialisierungskette: `INIT` → `logiBUS_QX.INIT`, `logiBUS_QX.INITO` → `Softkey_IX.INIT`, `Softkey_IX.INITO` → `INITO` und `AND_QO.REQ`.
4. Qualität: `QI` → `logiBUS_QX.QI` und `Softkey_IX.QI`; `logiBUS_QX.QO` → `AND_QO.IN1`; `Softkey_IX.QO` → `AND_QO.IN2`; `AND_QO.OUT` → `QO`.

## Anwendungsszenarien

- Minimaler Softkey-zu-Ausgang-Baustein für test_B.

## Zusammenfassung

test_B-Pendant zu `Softkey_IXA_TO_logiBUS_QXA`.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
