# AOption_AUI_DEMUX_3

![AOption_AUI_DEMUX_3](AOption_AUI_DEMUX_3.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock AOption_AUI_DEMUX_3 ist ein Demultiplexer (natives Generic-FB, kein ECC und kein Algorithmus in diesem Typ) für 3 Options-Ausgange vom Typ `Option::AOption`. Die Auswahl erfolgt über einen **Index** am bidirektionalen Adapter `K` (`adapter::types::bidirectional::AUI2`, UINT). Es handelt sich um eine Single-Select-Variante: höchstens ein Ausgang ist gleichzeitig aktiv.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Der Funktionsblock verfügt über keine direkten Ereignis-Eingänge – die Kommunikation läuft ausschließlich über die Adapter.

### **Ereignis-Ausgänge**

Der Funktionsblock verfügt über keine direkten Ereignis-Ausgänge.

### **Daten-Eingänge**

Der Funktionsblock verfügt über keine direkten Daten-Eingänge.

### **Daten-Ausgänge**

Der Funktionsblock verfügt über keine direkten Daten-Ausgänge.

### **Adapter**

- **K** (Socket, `adapter::types::bidirectional::AUI2`): Index. Bidirektional: `EO1`/`DO1` = neuer K-Wunsch kommt herein, `EI1`/`DI1` = Quittung mit dem übernommenen Wert geht zurück
- **OUT1** (Plug, `Option::AOption`): Option 1, ausgewählt wenn K = 1
- **OUT2** (Plug, `Option::AOption`): Option 2, ausgewählt wenn K = 2
- **OUT3** (Plug, `Option::AOption`): Option 3, ausgewählt wenn K = 3

Jeder Ausgang `OUTx` verwendet den Adapter `Option::AOption`: das Ereignis `INIT_REQ` (zusammen mit `QI_OUT`) läuft in Richtung Plug → Socket, die Bestätigung `INIT_CNF` in Gegenrichtung.

## Funktionsweise

**Indexzuordnung:**

- `K = 0` – alle Ausgänge AUS
- `K = 1` – `OUT1` AN, alle anderen AUS
- `K = 2` – `OUT2` AN, alle anderen AUS
- `K = 3` – `OUT3` AN, alle anderen AUS
- jeder andere Wert (> 3) – alle Ausgänge AUS

**Umschaltverhalten (Break-before-Make):** Wechselt `K` von einem aktiven Ausgang auf einen anderen, wird zuerst der bisher aktive Ausgang deaktiviert (`INIT_REQ` mit `QI=FALSE`). Erst wenn von diesem Ausgang `INIT_CNF` eingetroffen ist, wird der neu ausgewählte Ausgang aktiviert (`INIT_REQ` mit `QI=TRUE`). Es ist also nie mehr als eine Option gleichzeitig aktiv. Pro eintreffendem `K`-Wert wird jeder Ausgang maximal einmal getriggert; nicht betroffene Ausgänge werden nicht angefasst.

**Busy-Gating über den bidirektionalen K-Adapter:** Solange ein Umschaltvorgang läuft (Warten auf `INIT_CNF` vom bisherigen und/oder neuen Ausgang), wird jeder weitere über `K` (Richtung Plug → Socket, `EO1`/`DO1`) eintreffende Indexwunsch ignoriert – er wird weder gepuffert noch nachgeholt. Nach Abschluss meldet der Baustein dies rückwärts über `K` (Richtung Socket → Plug, `EI1`/`DI1`) mit dem übernommenen K-Wert zurück (nicht mit dem Zustand der Ausgänge: auch ein ungültiger Wert wird unverändert quittiert). Erst danach werden wieder neue Indexwünsche angenommen. Es zählt immer der Stand nach Abschluss des letzten tatsächlich bearbeiteten Wechsels.

## ⚖️ Vergleich mit ähnlichen Bausteinen

[AOption_AD_DEMUX_3](AOption_AD_DEMUX_3.md) ist die Multi-Select-Variante mit Bitmaske (`AD2`): dort können mehrere Ausgänge gleichzeitig aktiv sein.

## 🛠️ Zugehörige Übungen

- [Uebung_094b2_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_094b2_AX/)
- [Uebung_094b3_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_094b3_AX/)
- [Uebung_094b3b_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_094b3b_AX/)

