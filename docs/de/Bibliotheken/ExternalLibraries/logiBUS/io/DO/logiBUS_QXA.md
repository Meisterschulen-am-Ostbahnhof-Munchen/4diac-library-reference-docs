# logiBUS_QXA

<img width="2042" height="360" alt="image" src="https://github.com/user-attachments/assets/a209d37d-5012-4889-853b-e7a36dfc6644" />

* * * * * * * * * *

## Einleitung

Der logiBUS_QXA ist ein zusammengesetzter Funktionsblock für die Ausgabe von booleschen Daten. Er dient als Schnittstelle für digitale Ausgabefunktionen und ermöglicht die Steuerung von bis zu 8 digitalen Ausgängen über ein standardisiertes Protokoll.

![logiBUS_QXA](logiBUS_QXA.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **INIT**: Service-Initialisierungsereignis mit den zugehörigen Daten QI, PARAMS und Output

### **Ereignis-Ausgänge**

- **INITO**: Initialisierungsbestätigung mit den Daten QO und STATUS
- **CNF**: Bestätigung des angeforderten Services mit den Daten QO und STATUS

### **Daten-Eingänge**

- **QI**: Boolescher Ereigniseingangsqualifizierer
- **PARAMS**: Service-Parameter als Zeichenkette
- **Output**: Identifizierung der Ausgänge Q1 bis Q8 vom Typ logiBUS_DO_S mit Initialwert "Invalid"

### **Daten-Ausgänge**

- **QO**: Boolescher Ereignisausgangsqualifizierer
- **STATUS**: Service-Status als Zeichenkette

### **Adapter**

- **OUT**: Unidirektionaler Adapter vom Typ AX für die Ausgabekommunikation

## Funktionsweise

Der logiBUS_QXA fungiert als Wrapper für den logiBUS_QX-Funktionsblock und bietet eine vereinheitlichte Schnittstelle für digitale Ausgabefunktionen. Bei Initialisierung (INIT-Ereignis) werden die Konfigurationsparameter übergeben und die Ausgänge entsprechend konfiguriert. Der Block ermöglicht die Steuerung von bis zu 8 digitalen Ausgängen über die Output-Datenstruktur.

## Technische Besonderheiten

- Verwendet den logiBUS_QX-Kernfunktionsblock für die eigentliche Ausgabelogik
- Unterstützt bis zu 8 digitale Ausgänge (Q1 bis Q8)
- Initialisierung mit spezifischen Parametern über die PARAMS-Eingabe
- Rückmeldung des Betriebszustands über STATUS-Ausgabe

## Zustandsübersicht

Der Funktionsblock durchläuft folgende Zustände:

1. **Nicht initialisiert**: Vor dem INIT-Ereignis
2. **Initialisierung**: Während der Verarbeitung des INIT-Ereignis
3. **Betriebsbereit**: Nach erfolgreicher Initialisierung (INITO-Bestätigung)
4. **Aktiver Betrieb**: Verarbeitung von Ausgabeanforderungen über den OUT-Adapter

## Anwendungsszenarien

- Steuerung von digitalen Aktoren in Automatisierungssystemen
- Anbindung von Ausgabemodulen in verteilten Steuerungssystemen
- Integration in logiBUS-basierte Steuerungsarchitekturen
- Industrielle Automatisierung mit booleschen Ausgabesignalen

## ⚖️ Vergleich mit ähnlichen Bausteinen

Im Vergleich zu einfachen digitalen Ausgabeblöcken bietet logiBUS_QXA:

- Erweiterte Parametrierungsmöglichkeiten
- Statusrückmeldungen für Fehlerdiagnose
- Standardisierte Schnittstelle über Adapter
- Unterstützung für multiple Ausgänge in einer Struktur

## 🛠️ Zugehörige Übungen

- [Uebung_001_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_001_AX/)
- [Uebung_001_AX_b](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_001_AX_b/)
- [Uebung_001c_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_001c_AX/)
- [Uebung_002_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_002_AX/)
- [Uebung_002a2_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_002a2_AX/)
- [Uebung_002a3_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_002a3_AX/)
- [Uebung_002a5_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_002a5_AX/)
- [Uebung_002a5b_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_002a5b_AX/)
- [Uebung_002a6_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_002a6_AX/)
- [Uebung_002a7_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_002a7_AX/)
- [Uebung_002a_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_002a_AX/)
- [Uebung_002b3_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_002b3_AX/)
- [Uebung_003_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_003_AX/)
- [Uebung_003a0_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_003a0_AX/)
- [Uebung_003a_AX_sub](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_003a_AX_sub/)
- [Uebung_003d_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_003d_AX/)
- [Uebung_004a2_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004a2_AX/)
- [Uebung_004a3_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004a3_AX/)
- [Uebung_004a4_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004a4_AX/)
- [Uebung_004a5_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004a5_AX/)
- [Uebung_004a6_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004a6_AX/)
- [Uebung_004a7_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004a7_AX/)
- [Uebung_004a8_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004a8_AX/)
- [Uebung_004a9_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004a9_AX/)
- [Uebung_004a_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004a_AX/)
- [Uebung_004b_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004b_AX/)
- [Uebung_004b_AX_ASR](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004b_AX_ASR/)
- [Uebung_004b_AX_ASR_X](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004b_AX_ASR_X/)
- [Uebung_004c1_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004c1_AX/)
- [Uebung_004c2_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004c2_AX/)
- [Uebung_004c3_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004c3_AX/)
- [Uebung_004c4_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004c4_AX/)
- [Uebung_004c5_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004c5_AX/)
- [Uebung_004c6_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004c6_AX/)
- [Uebung_004c7_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004c7_AX/)
- [Uebung_005_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_005_AX/)
- [Uebung_006_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_006_AX/)
- [Uebung_006a2_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_006a2_AX/)
- [Uebung_006a3_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_006a3_AX/)
- [Uebung_006a4_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_006a4_AX/)
- [Uebung_006a_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_006a_AX/)
- [Uebung_006b_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_006b_AX/)
- [Uebung_006d_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_006d_AX/)
- [Uebung_006e1_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_006e1_AX/)
- [Uebung_006e2_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_006e2_AX/)
- [Uebung_007_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_007_AX/)
- [Uebung_007a1_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_007a1_AX/)
- [Uebung_007a2_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_007a2_AX/)
- [Uebung_007a3_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_007a3_AX/)
- [Uebung_008_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_008_AX/)
- [Uebung_009_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_009_AX/)
- [Uebung_010_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010_AX/)
- [Uebung_010a2_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010a2_AX/)
- [Uebung_010a3_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010a3_AX/)
- [Uebung_010a_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010a_AX/)
- [Uebung_010b1_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010b1_AX/)
- [Uebung_010b2_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010b2_AX/)
- [Uebung_010b3_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010b3_AX/)
- [Uebung_010b4_sub_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010b4_sub_AX/)
- [Uebung_010b5_sub_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010b5_sub_AX/)
- [Uebung_010b6_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010b6_AX/)
- [Uebung_010b7_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010b7_AX/)
- [Uebung_010b8_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010b8_AX/)
- [Uebung_010b9_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010b9_AX/)
- [Uebung_010bA2_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010bA2_AX/)
- [Uebung_010bA3_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010bA3_AX/)
- [Uebung_010bA4_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010bA4_AX/)
- [Uebung_010bA_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010bA_AX/)
- [Uebung_010c2_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010c2_AX/)
- [Uebung_010c3_sub_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010c3_sub_AX/)
- [Uebung_010c4_sub_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010c4_sub_AX/)
- [Uebung_010c_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010c_AX/)
- [Uebung_013_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_013_AX/)
- [Uebung_020a_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_020a_AX/)
- [Uebung_020b_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_020b_AX/)
- [Uebung_020c3_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_020c3_AX/)
- [Uebung_020c_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_020c_AX/)
- [Uebung_020d_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_020d_AX/)
- [Uebung_020e2_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_020e2_AX/)
- [Uebung_020e_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_020e_AX/)
- [Uebung_020f2_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_020f2_AX/)
- [Uebung_020f3_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_020f3_AX/)
- [Uebung_020f_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_020f_AX/)
- [Uebung_020g_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_020g_AX/)
- [Uebung_020h_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_020h_AX/)
- [Uebung_020i_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_020i_AX/)
- [Uebung_020j2_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_020j2_AX/)
- [Uebung_020j_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_020j_AX/)
- [Uebung_035a1_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_035a1_AX/)
- [Uebung_035a1b_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_035a1b_AX/)
- [Uebung_035a2_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_035a2_AX/)
- [Uebung_035a3_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_035a3_AX/)
- [Uebung_038_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_038_AX/)
- [Uebung_040_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_040_AX/)
- [Uebung_083_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_083_AX/)
- [Uebung_090a1_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_090a1_AX/)
- [Uebung_090a2_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_090a2_AX/)
- [Uebung_094a_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_094a_AX/)
- [Uebung_095_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_095_AX/)
- [Uebung_103](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_103/)
- [Uebung_103c](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_103c/)
- [Uebung_103c2](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_103c2/)
- [Uebung_150_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_150_AX/)
- [Uebung_151_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_151_AX/)
- [Uebung_160_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_160_AX/)
- [Uebung_160b2_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_160b2_AX/)
- [Uebung_160b_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_160b_AX/)
- [Uebung_171_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_171_AX/)
- [Uebung_177_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_177_AX/)
- [Uebung_178_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_178_AX/)

## Fazit

Der logiBUS_QXA ist ein robuster und flexibler Funktionsblock für digitale Ausgabefunktionen in industriellen Automatisierungssystemen. Durch seine standardisierte Schnittstelle und umfassende Parametrierungsmöglichkeiten eignet er sich ideal für den Einsatz in komplexen Steuerungsarchitekturen mit hohen Anforderungen an Zuverlässigkeit und Diagnosefähigkeit.
