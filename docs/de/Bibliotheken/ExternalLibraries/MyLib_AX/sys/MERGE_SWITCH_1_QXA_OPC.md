# MERGE_SWITCH_1_QXA_OPC

![MERGE_SWITCH_1_QXA_OPC_network](./MERGE_SWITCH_1_QXA_OPC_network.svg)

* * * * * * * * * *

## Einleitung

`MERGE_SWITCH_1_QXA_OPC` treibt einen einzelnen (nicht doppelwirkenden) logiBUS-Ausgang aus zwei OPC-UA-Quellen: dem bestehenden IO-Test-Kommando (Remote-Subscribe) und dem echten Funktions-Kommando (ebenfalls Remote-Subscribe, z. B. von einem SoftKey/AUX über `Softkey_Aux_IXA_TO_Remote_WRITE_BG_OPC`). Beide werden per Pegel-ODER zusammengeführt, sodass der bestehende IO-Test weiterhin nutzbar bleibt, während gleichzeitig die echte Funktion denselben Ausgang ansteuern kann. Für doppelwirkende Aktoren (Links/Rechts, Auf/Ab) ist stattdessen `ILOCK_SWITCH_2_QXA_OPC` zu verwenden.

## Verwendete Funktionsbausteine (FBs)

### Sub-Bausteine: MERGE_SWITCH_1_QXA_OPC

- **Typ**: SubAppType
- **Verwendete interne FBs**:
    - **SUBSCRIBE_TEST** / **SUBSCRIBE_CMD**: je `adapter::net::AX_SUBSCRIBE_1` (`QI=TRUE`) — abonnieren das bestehende IO-Test-Kommando (`ID_TEST_READ`) bzw. das echte Funktions-Kommando (`ID_READ`).
    - **OR_MERGE**: `adapter::booleanOperators::AX_OR_2` — führt beide Subscribe-Quellen per Pegel-ODER zusammen.
    - **SPLIT**: `adapter::events::unidirectional::AX_SPLIT_3` — verteilt das zusammengeführte Signal auf den physischen Ausgang und zwei Rückmelde-Kanäle.
    - **DigitalOutput**: `logiBUS::io::DQ::logiBUS_QXA` (`QI=TRUE`) — physischer digitaler Ausgang.
    - **PUBLISH_TEST** / **PUBLISH_STATE**: je `adapter::net::AX_PUBLISH_1` (`QI=TRUE`) — melden den resultierenden Zustand an die bestehende IO-Test-Überwachung (`ID_TEST_WRITE`) bzw. an die echte Funktions-Rückmeldung (`ID_WRITE`, z. B. `GreenWhiteBackground` am SoftKey).
- **Funktionsweise**: Zwei unabhängige Remote-Subscribe-Quellen werden per `AX_OR_2` zusammengeführt und treiben gemeinsam den physischen Ausgang sowie zwei getrennte Rückmeldekanäle.

## Programmablauf und Verbindungen

1. `ID_TEST_READ` → `SUBSCRIBE_TEST.ID`; `ID_READ` → `SUBSCRIBE_CMD.ID` (Datenverbindungen, ausgeblendet).
2. `SUBSCRIBE_TEST.OUT` → `OR_MERGE.IN1`; `SUBSCRIBE_CMD.OUT` → `OR_MERGE.IN2`.
3. `OR_MERGE.OUT` → `SPLIT.IN` → `SPLIT.OUT1` → `DigitalOutput.OUT` (physischer Ausgang), `SPLIT.OUT2` → `PUBLISH_TEST.IN`, `SPLIT.OUT3` → `PUBLISH_STATE.IN`.
4. Parameter: `Output` → `DigitalOutput.Output`; `ID_TEST_WRITE` → `PUBLISH_TEST.ID`; `ID_WRITE` → `PUBLISH_STATE.ID`.

## Technische Besonderheiten

- **Zwei getrennte Rückmelde-Knoten**: Der resultierende Zustand wird auf zwei eigene, vom jeweiligen Kommando-Knoten getrennte OPC-UA-Knoten zurückgemeldet (`ID_TEST_WRITE` für die bestehende IO-Diagnoseanzeige, `ID_WRITE` für die echte Funktionsrückmeldung). Kommando- und Status-Knoten dürfen sich nie denselben OPC-UA-Knoten teilen — sonst liest das eigene Subscribe das eigene Publish-Echo als neuen Befehl und der Ausgang hängt fest.
- **Pegel-ODER statt Flankenlogik**: Im Gegensatz zu den latchenden Button-Bausteinen (z. B. `Button_IXA_TO_logiBUS_QXA_BG_OPC_LATCHING`) verwendet dieser Baustein ein einfaches, dauerhaftes ODER (`AX_OR_2`) — passend für ein Kommando, das durchgehalten (nicht getoggelt) wird.
- **Generisch für einfache Ausgänge**: Anwendbar auf jeden einfachen, nicht doppelwirkenden Ausgang mit Bedienung von einem Modul ohne eigenes VT.

## Anwendungsszenarien

- Physische Ausgänge, deren bestehender IO-Test-Kanal auch nach Einführung der echten Funktion weiter nutzbar bleiben soll, ohne dass beide Kommandoquellen sich gegenseitig blockieren.
- Einfache, nicht doppelwirkende Aktoren (z. B. Blitzlicht, Beleuchtung), die sowohl vom IO-Test als auch von einer echten Bedienfunktion (SoftKey/AUX) angesteuert werden können.

## Vergleich mit ähnlichen Bausteinen

Für doppelwirkende Aktoren (Links/Rechts, Auf/Ab), bei denen sich die beiden Richtungen gegenseitig ausschließen müssen, ist `ILOCK_SWITCH_2_QXA_OPC` zu verwenden — `MERGE_SWITCH_1_QXA_OPC` besitzt keinen Interlock, da es nur eine einzige Ausgangsrichtung ansteuert.

## Zusammenfassung

`MERGE_SWITCH_1_QXA_OPC` führt IO-Test-Kommando und echtes Funktions-Kommando per Pegel-ODER zu einem gemeinsamen, einfachen Ausgang zusammen und meldet den Zustand getrennt an beide Überwachungswege zurück.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
