# Button_IE

<img width="1385" height="216" alt="image" src="https://github.com/user-attachments/assets/95422805-a0b9-47d0-9696-02c3ede5c9cf" />

* * * * * * * * * *

## Einleitung

Der Button_IE Funktionsblock ist ein Eingabeservice-Interface-Funktionsblock für Ereigniseingabedaten. Er dient als Schnittstelle für Taster-Ereignisse in Steuerungssystemen und ermöglicht die Verarbeitung verschiedener Tasteraktivitäten wie Drücken, Loslassen oder Mehrfachklicks.

![Button_IE](Button_IE.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **INIT**: Service-Initialisierung
- **REQ**: Service-Anfrage

### **Ereignis-Ausgänge**

- **INITO**: Initialisierungsbestätigung
- **CNF**: Bestätigung der angeforderten Service-Anfrage
- **IND**: Indikation von der Ressource

### **Daten-Eingänge**

- **QI** (BOOL): Ereignis-Eingangsqualifizierer
- **PARAMS** (STRING): Service-Parameter
- **u16ObjId** (UINT): Objekt-ID (Initialwert: ID_NULL)
- **InputEvent** (ButtonActivationCode_S): Identifiziert das Ereignis (Down, Up, Single-Click, Double-Click etc.) mit Initialwert "Invalid"

### **Daten-Ausgänge**

- **QO** (BOOL): Ereignis-Ausgangsqualifizierer
- **STATUS** (STRING): Service-Status

### **Adapter**

Keine Adapter-Schnittstellen vorhanden.

## Funktionsweise

Der Button_IE Funktionsblock verarbeitet Tasterereignisse über das ISOBUS-UT-Protokoll. Bei Initialisierung (INIT) werden die Service-Parameter und Objekt-ID konfiguriert. Der Block kann verschiedene Tasteraktivitäten wie Einfachklick, Doppelklick, Drücken und Loslassen erkennen und entsprechend verarbeiten. Über die IND-Ausgabe werden eingehende Ereignisse von der Hardware-Ressource gemeldet.

## Technische Besonderheiten

- Verwendet ISOBUS-UT-spezifische Datentypen für Tasteraktivierungscodes
- Unterstützt Initialisierung mit spezifischen Objekt-IDs
- Bietet umfassende Statusrückmeldungen über den STATUS-Ausgang
- Implementiert qualifizierte Ereignisverarbeitung über QI/QO-Signale

## Zustandsübersicht

Der Funktionsblock verfügt über einen Initialisierungszustand (INIT/INITO) und operative Zustände für Service-Anfragen (REQ/CNF) sowie asynchrone Ereignisindikationen (IND). Die genaue Zustandsmaschine ist implementierungsabhängig.

## Anwendungsszenarien

- Landwirtschaftliche Steuerungssysteme mit ISOBUS-Kompatibilität
- Bedienpanels mit Tastereingaben
- Maschinensteuerungen mit Ereignis-basierten Eingaben
- Systeme, die verschiedene Tasteraktivitäten unterscheiden müssen

## ⚖️ Vergleich mit ähnlichen Bausteinen

Im Vergleich zu einfachen digitalen Eingangsblöcken bietet Button_IE erweiterte Funktionalität für Tasterspezifische Ereignisse wie Mehrfachklicks und unterscheidet zwischen verschiedenen Aktivierungszuständen. Die ISOBUS-Integration macht ihn speziell für landwirtschaftliche Anwendungen geeignet.

## 🛠️ Zugehörige Übungen

- [Uebung_010b7](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-en/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010b7/)
- [Uebung_010b7_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-en/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010b7_AX/)
- [Uebung_010b8](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-en/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010b8/)
- [Uebung_010b8_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-en/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010b8_AX/)
- [Uebung_010b9](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-en/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010b9/)
- [Uebung_010b9_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-en/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010b9_AX/)
- [Uebung_010bA](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-en/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010bA/)
- [Uebung_010bA_AX](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-en/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010bA_AX/)

## Fazit

Button_IE ist ein spezialisierter Funktionsblock für die Verarbeitung von Tasterereignissen in ISOBUS-Umgebungen. Seine Fähigkeit, verschiedene Tasteraktivitäten zu unterscheiden und umfassende Statusinformationen bereitzustellen, macht ihn besonders geeignet für anspruchsvolle Steuerungsanwendungen in der Agrartechnik und verwandten Bereichen.
