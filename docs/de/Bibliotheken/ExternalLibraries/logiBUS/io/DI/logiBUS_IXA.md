# logiBUS_IXA

<img width="1838" height="367" alt="image" src="https://github.com/user-attachments/assets/fcf18e0e-a542-4642-a00f-1438f4caf5fb" />

* * * * * * * * * *

## Einleitung

Der logiBUS_IXA ist ein Composite-Funktionsblock zur Verarbeitung von booleschen Eingangsdaten. Er dient als Schnittstelle für digitale Eingänge und ermöglicht die Initialisierung und Abfrage von Eingangssignalen über standardisierte Service-Schnittstellen.

![logiBUS_IXA](logiBUS_IXA.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **INIT**: Service-Initialisierungsereignis mit den zugehörigen Daten QI, PARAMS und Input
- **REQ**: Service-Anfrageereignis mit dem zugehörigen Datenwert QI

### **Ereignis-Ausgänge**

- **INITO**: Initialisierungsbestätigung mit den zugehörigen Datenwerten QO und STATUS

### **Daten-Eingänge**

- **QI**: Boolescher Ereignis-Eingangsqualifizierer
- **PARAMS**: Service-Parameter als Zeichenkette
- **Input**: Identifiziert die Eingänge Q1 bis Q8 mit dem Typ logiBUS_DI_S und Initialwert "Invalid"

### **Daten-Ausgänge**

- **QO**: Boolescher Ereignis-Ausgangsqualifizierer
- **STATUS**: Service-Status als Zeichenkette

### **Adapter**

- **IN**: Unidirektionaler Adapter vom Typ AX für die Eingangsdatenverarbeitung

## Funktionsweise

Der Composite-Funktionsblock logiBUS_IXA kapselt den Basisfunktionsblock logiBUS_IX und erweitert dessen Funktionalität durch zusätzliche Adapter-Schnittstellen. Bei INIT-Ereignissen werden die Parameter an den internen IX-Block weitergeleitet, der die Initialisierung durchführt. Die IND-Ereignisse des IX-Blocks werden an den Eingangsadapter IN weitergegeben, während die Datenverbindungen die entsprechende Signalverarbeitung sicherstellen.

## Technische Besonderheiten

- Verwendet den spezifischen Datentyp logiBUS_DI_S für die Eingangsidentifikation
- Implementiert standardisierte Service-Schnittstellen gemäß 61499-2
- Unterstützt Parameterübergabe via STRING-Datentyp
- Bietet Statusrückmeldungen über STATUS-Ausgang

## Zustandsübersicht

Der Funktionsblock verfügt über zwei Hauptzustände:

1. **Nicht initialisiert**: Vor dem INIT-Ereignis
2. **Initialisiert und betriebsbereit**: Nach erfolgreicher INIT-Bestätigung

## Anwendungsszenarien

- Anbindung digitaler Eingänge in Automatisierungssystemen
- Integration in logiBUS-basierte Steuerungsarchitekturen
- Verwendung in SPS-Systemen mit booleschen Signalverarbeitungsanforderungen

## ⚖️ Vergleich mit ähnlichen Bausteinen

Im Vergleich zu einfachen digitalen Eingangsblöcken bietet logiBUS_IXA erweiterte Service-Funktionalitäten mit Parametrierungsmöglichkeiten und Statusrückmeldungen. Die Composite-Struktur ermöglicht eine bessere Wiederverwendbarkeit und erweiterte Diagnosefähigkeiten.

## 🛠️ Zugehörige Übungen

- [Uebung_001_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_001_AX/)
- [Uebung_001_AX_b](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_001_AX_b/)
- [Uebung_001c_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_001c_AX/)
- [Uebung_002_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_002_AX/)
- [Uebung_002a2_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_002a2_AX/)
- [Uebung_002a3_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_002a3_AX/)
- [Uebung_002a5_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_002a5_AX/)
- [Uebung_002a5b_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_002a5b_AX/)
- [Uebung_002a6_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_002a6_AX/)
- [Uebung_002a7_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_002a7_AX/)
- [Uebung_002a_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_002a_AX/)
- [Uebung_002b3_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_002b3_AX/)
- [Uebung_003_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_003_AX/)
- [Uebung_003a0_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_003a0_AX/)
- [Uebung_003a_AX_sub](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_003a_AX_sub/)
- [Uebung_003c_sub_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_003c_sub_AX/)
- [Uebung_003d_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_003d_AX/)
- [Uebung_005_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_005_AX/)
- [Uebung_006e1_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_006e1_AX/)
- [Uebung_006e2_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_006e2_AX/)
- [Uebung_020a_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_020a_AX/)
- [Uebung_020b_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_020b_AX/)
- [Uebung_020c3_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_020c3_AX/)
- [Uebung_020c_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_020c_AX/)
- [Uebung_020d_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_020d_AX/)
- [Uebung_020e2_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_020e2_AX/)
- [Uebung_020e_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_020e_AX/)
- [Uebung_020f2_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_020f2_AX/)
- [Uebung_020f_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_020f_AX/)
- [Uebung_020g_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_020g_AX/)
- [Uebung_020i_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_020i_AX/)
- [Uebung_020j2_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_020j2_AX/)
- [Uebung_020j_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_020j_AX/)
- [Uebung_090a1_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_090a1_AX/)
- [Uebung_090a2_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_090a2_AX/)
- [Uebung_094a_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_094a_AX/)
- [Uebung_095_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_095_AX/)
- [Uebung_103](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_103/)
- [Uebung_103c](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_103c/)
- [Uebung_103c2](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_103c2/)
- [Uebung_160_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_160_AX/)
- [Uebung_160b2_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_160b2_AX/)
- [Uebung_177_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_177_AX/)
- [Uebung_178_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_178_AX/)

## Fazit

Der logiBUS_IXA Funktionsblock stellt eine robuste und flexible Lösung für die Verarbeitung digitaler Eingangssignale in 4diac-basierten Automatisierungssystemen dar. Durch seine standardisierten Schnittstellen und erweiterten Service-Funktionen eignet er sich besonders für komplexe Anwendungen mit hohen Anforderungen an Diagnose und Parametrierbarkeit.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
