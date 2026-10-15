# logiBUS_QXA_FROM_SUBSCRIBE_OPC

![logiBUS_QXA_FROM_SUBSCRIBE_OPC_network](./logiBUS_QXA_FROM_SUBSCRIBE_OPC_network.svg)

* * * * * * * * * *

## Einleitung

`logiBUS_QXA_FROM_SUBSCRIBE_OPC` steuert einen digitalen Ausgang (`logiBUS_QXA`) von einem lokal ueberwachten OPC-UA-Knoten, der von einem ANDEREN Geraet per `AX_CLIENT_1_0` remote beschrieben wird - "Krauternter-Stil" (laut Quellcode-Kommentar): das Protokoll (`AX_SUBSCRIBE_1`) steckt IM Baustein selbst, nicht in der Resource des Geraets.

![logiBUS_QXA_FROM_SUBSCRIBE_OPC](logiBUS_QXA_FROM_SUBSCRIBE_OPC.svg)

## Verwendete Funktionsbausteine (FBs)

- **AX_SUBSCRIBE_1** (`adapter::net::AX_SUBSCRIBE_1`): abonniert die lokal ueberwachte Adresse `ID` (BOOL, remote beschrieben).
- **logiBUS_QXA** (`logiBUS::io::DQ::logiBUS_QXA`): physischer digitaler Ausgang, identifiziert ueber `Output_Q1..Q8`.
- **AND_QO** (`iec61131::booleanOperators::AND_BOOL_2`): verknuepft `AX_SUBSCRIBE_1.QO` und `logiBUS_QXA.QO` zum SubApp-Ausgang `QO`.

## Initialisierung und Qualitaet

- Ereignisse: `INIT` (EInit, Eingang) und `INITO` (EInit, Ausgang, kommt von `logiBUS_QXA.INITO`).
- Dateneingang `QI` (BOOL, Vorgabe `TRUE`) und Datenausgang `QO` (BOOL); `AX_SUBSCRIBE_1` und `logiBUS_QXA` tragen kein festes `QI=TRUE` mehr.
- Initialisierung: `INIT` → `AX_SUBSCRIBE_1.INIT`, `AX_SUBSCRIBE_1.INITO` → `logiBUS_QXA.INIT`, `logiBUS_QXA.INITO` → `INITO` und → `AND_QO.REQ`.
- Qualitaet: `QI` → `AX_SUBSCRIBE_1.QI` und `logiBUS_QXA.QI`; `AX_SUBSCRIBE_1.QO` → `AND_QO.IN1`, `logiBUS_QXA.QO` → `AND_QO.IN2`; `AND_QO.OUT` → `QO`.

## Zusammenfassung

Ein-Baustein-Bruecke von einem lokal abonnierten OPC-UA-Wert zu einem physischen digitalen Ausgang - Gegenstueck zu [`logiBUS_IXA_TO_CLIENT_OPC`](./logiBUS_IXA_TO_CLIENT_OPC.md) auf der Sendeseite.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
