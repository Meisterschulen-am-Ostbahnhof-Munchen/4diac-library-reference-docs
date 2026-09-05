# logiBUS_IX

<img width="1789" height="343" alt="image" src="https://github.com/user-attachments/assets/8c558337-facf-438d-87ba-69a1b8e110a9" />

* * * * * * * * * *

## Einleitung

Der logiBUS_IX Funktionsblock ist ein Eingabeservice-Interface für boolesche Eingangsdaten, der speziell für die Kommunikation mit logiBUS-Eingabemodulen entwickelt wurde. Er dient als Schnittstelle zwischen der Steuerungslogik und physischen Eingabesignalen und ermöglicht die Abfrage von digitalen Eingangswerten.

![logiBUS_IX](logiBUS_IX.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **INIT**: Service-Initialisierungsereignis
- **REQ**: Service-Anfrageereignis

### **Ereignis-Ausgänge**

- **INITO**: Initialisierungsbestätigung
- **CNF**: Bestätigung der angeforderten Service-Anfrage
- **IND**: Anzeige vom Ressourcen-Interface

### **Daten-Eingänge**

- **QI**: Ereignis-Eingangsqualifizierer (BOOL)
- **PARAMS**: Service-Parameter (STRING)
- **Input**: Identifiziert den Eingang I1..I8 (logiBUS_DI_S) - Initialwert: Invalid

### **Daten-Ausgänge**

- **QO**: Ereignis-Ausgangsqualifizierer (BOOL)
- **STATUS**: Service-Status (STRING)
- **IN**: Eingangsdaten von der Ressource (BOOL)

### **Adapter**

Keine Adapter-Schnittstellen vorhanden.

## Funktionsweise

Der Funktionsblock initialisiert sich über das INIT-Ereignis und kann anschließend über das REQ-Ereignis Eingangsdaten anfordern. Bei erfolgreicher Initialisierung gibt er das INITO-Ereignis zurück. Die tatsächlichen Eingangswerte werden über den IN-Ausgang bereitgestellt, während der STATUS-Ausgang Informationen über den aktuellen Betriebszustand liefert.

## Technische Besonderheiten

- Unterstützt bis zu 8 digitale Eingänge (I1..I8)
- Verwendet spezielle logiBUS-Datentypen für die Eingangsidentifikation
- Bietet umfangreiche Statusrückmeldungen über den STATUS-Ausgang
- Initialisiert mit einem ungültigen Eingangswert (Invalid)

## Zustandsübersicht

Der Funktionsblock durchläuft typischerweise folgende Zustände:

1. **Nicht initialisiert**: Vor der INIT-Anforderung
2. **Initialisiert**: Nach erfolgreicher INIT-Verarbeitung
3. **Bereit**: Kann REQ-Anfragen verarbeiten
4. **Aktiv**: Verarbeitet gerade eine Service-Anfrage

## Anwendungsszenarien

- Abfrage von digitalen Eingangssignalen in Automatisierungssystemen
- Integration von logiBUS-Eingabemodulen in 4diac-basierte Steuerungen
- Überwachung von Schalterzuständen und Sensorsignalen
- Industrielle E/A-Steuerung mit Statusüberwachung

## ⚖️ Vergleich mit ähnlichen Bausteinen

Im Vergleich zu einfachen digitalen Eingangsblöcken bietet logiBUS_IX:

- Erweiterte Statusinformationen
- Parametrierbare Service-Parameter
- Spezifische logiBUS-Hardware-Integration
- Umfangreichere Initialisierungs- und Bestätigungsmechanismen

## 🛠️ Zugehörige Übungen

- [Uebung_001](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_001/)
- [Uebung_001c](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_001c/)
- [Uebung_002](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_002/)
- [Uebung_002a](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_002a/)
- [Uebung_002a2](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_002a2/)
- [Uebung_002a3](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_002a3/)
- [Uebung_002a4](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_002a4/)
- [Uebung_002a5b](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_002a5b/)
- [Uebung_002b2](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_002b2/)
- [Uebung_002b3](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_002b3/)
- [Uebung_003](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_003/)
- [Uebung_003a0](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_003a0/)
- [Uebung_003a_sub](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_003a_sub/)
- [Uebung_003b_sub](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_003b_sub/)
- [Uebung_003c_sub](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_003c_sub/)
- [Uebung_003d](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_003d/)
- [Uebung_005](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_005/)
- [Uebung_006e1](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_006e1/)
- [Uebung_006e2](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_006e2/)
- [Uebung_019c](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_019c/)
- [Uebung_020a](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_020a/)
- [Uebung_020b](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_020b/)
- [Uebung_020c](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_020c/)
- [Uebung_020c2](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_020c2/)
- [Uebung_020c3](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_020c3/)
- [Uebung_020d](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_020d/)
- [Uebung_020e](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_020e/)
- [Uebung_020e2](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_020e2/)
- [Uebung_020f](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_020f/)
- [Uebung_020f2](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_020f2/)
- [Uebung_020g](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_020g/)
- [Uebung_020i](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_020i/)
- [Uebung_028](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_028/)
- [Uebung_029](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_029/)
- [Uebung_030](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_030/)
- [Uebung_032](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_032/)
- [Uebung_033_sub](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_033_sub/)
- [Uebung_049](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_049/)
- [Uebung_051](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_051/)
- [Uebung_052](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_052/)
- [Uebung_053](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_053/)
- [Uebung_054](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_054/)
- [Uebung_055](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_055/)
- [Uebung_056](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_056/)
- [Uebung_085](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_085/)
- [Uebung_086](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_086/)
- [Uebung_087](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_087/)
- [Uebung_087a1](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_087a1/)
- [Uebung_087a2](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_087a2/)
- [Uebung_088](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_088/)
- [Uebung_089](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_089/)
- [Uebung_090a1](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_090a1/)
- [Uebung_090a1_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_090a1_AX/)
- [Uebung_090a2](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_090a2/)
- [Uebung_090a2_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_090a2_AX/)
- [Uebung_094](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_094/)
- [Uebung_094a](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_094a/)
- [Uebung_095](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_095/)
- [Uebung_160](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_160/)
- [Uebung_160b2](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_160b2/)
- [Uebung_177](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_177/)
- [Uebung_178](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_178/)

## Fazit

Der logiBUS_IX Funktionsblock stellt eine robuste und flexible Lösung für die Integration von logiBUS-Eingabemodulen in 4diac-basierte Steuerungssysteme dar. Seine umfangreiche Statusrückmeldung und parametrierbare Schnittstelle machen ihn besonders geeignet für industrielle Anwendungen, bei denen zuverlässige E/A-Kommunikation erforderlich ist.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
