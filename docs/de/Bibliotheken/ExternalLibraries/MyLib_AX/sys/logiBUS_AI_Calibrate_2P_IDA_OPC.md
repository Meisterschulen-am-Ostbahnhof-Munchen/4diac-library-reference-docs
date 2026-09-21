# logiBUS_AI_Calibrate_2P_IDA_OPC

![logiBUS_AI_Calibrate_2P_IDA_OPC_network](./logiBUS_AI_Calibrate_2P_IDA_OPC_network.svg)

* * * * * * * * * *

## Einleitung

`logiBUS_AI_Calibrate_2P_IDA_OPC` bindet einen physischen Analogeingang (`logiBUS_AI_IDA`) an eine vollständige VT- und OPC-UA-gestützte 2-Punkt-Kalibrierung (`AR_CALIBRATE_2P_REF`) an — genutzt vom [AI_Calibrate_2P-Trainingsbeispiel](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Meins/InputOutputTester/Button_AI_Calibrate_2P_OPC_UA/InputOutputTesterButton_AI_Calibrate_2P_OPC_UA/). Der Rohwert des Eingangs wird über die 2-Punkt-Kalibrierkette in einen physikalisch skalierten Wert gewandelt; Nullpunkt (`Zero`) und Spanne (`Span`) sowie deren gemessene Rohwerte (`ZeroRaw`/`SpanRaw`) sind sowohl per VT als auch per OPC-UA einstell- bzw. ablesbar und werden per INI-Datei persistiert. Bei Teilkalibrierung (nur ein Punkt kalibriert) greift sofort eine saubere Ausweichlösung auf den verfügbaren Referenzwert.

## Verwendete Funktionsbausteine (FBs)

- **logiBUS_AI_IDA** (`logiBUS::io::AI::logiBUS_AI_IDA`): physischer Analogeingang, liefert den Rohwert als Adapter.
- **AR_CALIBRATE_2P_REF** (Adapter-Composite): 2-Punkt-Kalibrierung (Nullpunkt/Spanne) mit Rohwert-Diagnose (`ZeroRaw`/`SpanRaw`), Teilkalibrierungs-Ausweichlogik und konfigurierbarem Clipping (`xClipping`).
- **VT- und OPC-UA-Brücken** (analog zu [`NumericValue_TO_AR2_OPC`](./NumericValue_TO_AR2_OPC.md)/[`OPC_TO_AR2`](./OPC_TO_AR2.md) und [`INI_IN_AND_STORE_AR2`](./INI_IN_AND_STORE_AR2.md)): stellen `Zero`/`Span` und `ZeroRaw`/`SpanRaw` sowohl über VT-Eingabefelder/Anzeigen als auch über OPC-UA bereit und persistieren die Kalibrierwerte per INI-Datei.

## Zusammenfassung

Kompletter 2-Punkt-Kalibrier-Baustein für einen physischen Analogeingang: Rohwert-Erfassung, 2-Punkt-Kalibrierung mit Rohwert-Diagnose, sofort wirksamer Teilkalibrierung, VT-Anzeige/-Eingabe und OPC-UA-Anbindung in einem Composite.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
