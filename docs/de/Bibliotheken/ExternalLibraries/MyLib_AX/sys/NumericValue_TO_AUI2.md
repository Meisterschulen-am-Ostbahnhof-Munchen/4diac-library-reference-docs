# NumericValue_TO_AUI2

![NumericValue_TO_AUI2_network](./NumericValue_TO_AUI2_network.svg)

* * * * * * * * * *

## Einleitung

`NumericValue_TO_AUI2` liest ein VT-Eingabefeld (CInputNumber) und reicht den Wert bidirektional als AUI2-Adapter (`VALUEO`) an einen Consumer weiter (z. B. `AOption_AUI_DEMUX_N.K`). Der Baustein speichert **nicht** selbst: Der Consumer meldet den tatsächlich übernommenen Wert über denselben AUI2-Plug zurück, das Echo wird auf das VT-Feld zurückgeschrieben.

![NumericValue_TO_AUI2](NumericValue_TO_AUI2.svg)

## Schnittstelle

### Eingangsvariablen

- `QI` (BOOL, Vorgabe `TRUE`): Quality Input - TRUE aktiviert den Dienst.
- `u16ObjId` (UINT, Default `ID_NULL`): ObjectID des VT-Eingabefelds.

### Ausgangsvariablen

- `QO` (BOOL): Quality Output - TRUE, wenn der Dienst erfolgreich ausgeführt wurde (UND aus `Q_NumericValue_AUDI.QO` und `NumericValue_IDA.QO`).

### Adapter

- **Plug `VALUEO`** (`adapter::types::bidirectional::AUI2`): Wert zum Consumer und Echo zurück.

### Ereignisse

- **INIT** (Ereignis-Eingang, `EInit`): Initialisierungsanforderung
- **INITO** (Ereignis-Ausgang, `EInit`): Initialisierungsbestätigung

## Funktionsweise

1. `NumericValue_IDA` liest das VT-Eingabefeld; `AD_TO_AUI` wandelt in AUI.
2. `AUI_TO_AUI2` wandelt in den bidirektionalen AUI2-Adapter, der an `VALUEO` ausgegeben wird.
3. Das vom Consumer zurückgemeldete Echo erscheint an `AUI_TO_AUI2.AUI_OUT`, wird per `AUI_TO_AUDI` in UDINT gewandelt und von `Q_NumericValue_AUDI` (`u32NewValue`) unter `u16ObjId` auf das VT-Feld geschrieben.

`QI` wird an `Q_NumericValue_AUDI.QI` und `NumericValue_IDA.QI` verteilt; `Q_NumericValue_AUDI.QO` und `NumericValue_IDA.QO` werden über `AND_QO` (`iec61131::booleanOperators::AND_BOOL_2`) zu `QO` verknüpft.

Initialisierung: `INIT` → `NumericValue_IDA.INIT`; `NumericValue_IDA.INITO` → `Q_NumericValue_AUDI.INIT`; `Q_NumericValue_AUDI.INITO` → `INITO` (und → `AND_QO.REQ`).

## Technische Besonderheiten

- Ausgelagert aus `Uebung_094c_AX`, wo dies je Dropdown inline dupliziert war.
- Analog zu [`NumericValue_TO_AR2`](./NumericValue_TO_AR2.md), aber UINT/AUI2 statt REAL/AR2 und einfaches `u16ObjId` statt `stObj` (kein Scale/Offset/Decimals für einen reinen Indexwert nötig).
- Mit INI-Persistenz siehe [`NumericValue_TO_AUI2_AND_STORE`](./NumericValue_TO_AUI2_AND_STORE.md).

## Zusammenfassung

Bidirektionale Brücke vom VT-Eingabefeld zu einem AUI2-Plug ohne eigene Persistierung.

## 🛠️ Zugehörige Übungen

- [Uebung_094b3_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_094b3_AX/)
- [Uebung_094b4_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_094b4_AX/)
- [Uebung_094c2_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_094c2_AX/)

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
