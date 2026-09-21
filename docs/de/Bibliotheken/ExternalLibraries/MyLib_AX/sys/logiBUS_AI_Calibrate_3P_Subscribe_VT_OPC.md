# logiBUS_AI_Calibrate_3P_Subscribe_VT_OPC

* * * * * * * * * *

## Einleitung

`logiBUS_AI_Calibrate_3P_Subscribe_VT_OPC` bindet einen per Subscribe empfangenen Rohwert an eine vollständige VT- und OPC-UA-gestützte 3-Punkt-Kalibrierung (`AR_CALIBRATE_3P_REF`) an — genutzt vom 3-Punkt-Kalibrierungs-Trainingsbeispiel [InputOutputTesterButton_AI_Calibrate_3P_Subscribe_VT_OPC](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Meins/InputOutputTester/Button_AI_Calibrate_3P_Subscribe_VT_OPC/InputOutputTesterButton_AI_Calibrate_3P_Subscribe_VT_OPC/). Der Rohwert wird über 3 Referenzpunkte (`Min`, `Mid`, `Max`) stückweise linear skaliert. Neben den Sollwerten werden auch die gemessenen Rohwerte (`MinRaw`, `MidRaw`, `MaxRaw`) live diagnostiziert, über VT sowie OPC-UA bereitgestellt und per INI-Datei persistiert. Bei Teilkalibrierung (z.B. nur 1 oder 2 Punkte kalibriert) greift sofort eine saubere Ausweichlogik auf die verfügbaren Referenzpunkte.

## Verwendete Funktionsbausteine (FBs)

- **SUBSCRIBE_1** (`net::SUBSCRIBE_1`): empfängt den Rohwert des Sensors über das Netzwerk.
- **AR_CALIBRATE_3P_REF** (Adapter-Composite): 3-Punkt-Kalibrierung (`Min`/`Mid`/`Max`) mit Rohwert-Diagnose (`MinRaw`/`MidRaw`/`MaxRaw`), stückweise linearer Interpolation, Teilkalibrierungs-Ausweichlogik und konfigurierbarem Clipping (`xClipping`).
- **VT- und OPC-UA-Brücken** (analog zu [`NumericValue_TO_AR2_OPC`](./NumericValue_TO_AR2_OPC.md)/[`OPC_TO_AR2`](./OPC_TO_AR2.md) und [`INI_IN_AND_STORE_AR2`](./INI_IN_AND_STORE_AR2.md)): stellen `Min`/`Mid`/`Max` und `MinRaw`/`MidRaw`/`MaxRaw` sowohl über VT-Eingabefelder/Anzeigen als auch über OPC-UA bereit und persistieren die Kalibrierwerte per INI-Datei.

## Zusammenfassung

Kompletter 3-Punkt-Kalibrier-Baustein für per Subscribe empfangene Rohwerte: Rohwert-Erfassung über Netzwerk, 3-Punkt-Kalibrierung mit Rohwert-Diagnose, sofort wirksamer Teilkalibrierung, VT-Anzeige/-Eingabe und OPC-UA-Anbindung in einem Composite.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
