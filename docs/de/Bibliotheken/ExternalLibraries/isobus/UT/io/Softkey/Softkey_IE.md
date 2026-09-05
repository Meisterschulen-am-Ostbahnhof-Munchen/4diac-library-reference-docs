# Softkey_IE

## 🎧 Podcast

- [ISO 11783-6: Softkeys und das Virtual Terminal verstehen – Dein Schlüssel zur Landmaschinen-Mechatronik](https://podcasters.spotify.com/pod/show/isobus-vt-objects/episodes/ISO-11783-6-Softkeys-und-das-Virtual-Terminal-verstehen--Dein-Schlssel-zur-Landmaschinen-Mechatronik-e36a8b0)

## Einleitung

Der Softkey_IE Funktionsblock ist ein Eingabeservice-Schnittstellen-Funktionsblock für Ereigniseingabedaten, der speziell für die Verarbeitung von Softkey-Ereignissen gemäß ISO 11783-6 entwickelt wurde. Er dient als Schnittstelle zwischen der Anwendungslogik und den physikalischen Softkey-Eingabegeräten in landwirtschaftlichen und mobilen Arbeitsmaschinen.

![Softkey_IE](Softkey_IE.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **INIT**: Service-Initialisierung mit Parametern QI, PARAMS, u16ObjId und InputEvent
- **REQ**: Service-Anfrage mit Parameter QI

### **Ereignis-Ausgänge**

- **INITO**: Initialisierungsbestätigung mit Parametern QO und STATUS
- **CNF**: Bestätigung der angeforderten Service-Anfrage mit Parametern QO und STATUS
- **IND**: Indikation von der Ressource mit Parametern QO und STATUS

### **Daten-Eingänge**

- **QI** (BOOL): Ereignis-Eingangsqualifizierer
- **PARAMS** (STRING): Service-Parameter
- **u16ObjId** (UINT): Objekt-ID mit Initialwert ID_NULL
- **InputEvent** (SoftKeyActivationCode_S): Identifiziert das Ereignis gemäß ISO 11783-6 mit Initialwert "Invalid"

### **Daten-Ausgänge**

- **QO** (BOOL): Ereignis-Ausgangsqualifizierer
- **STATUS** (STRING): Service-Status

### **Adapter**

Keine Adapter-Schnittstellen vorhanden.

## Funktionsweise

Der Softkey_IE Funktionsblock verwaltet die Kommunikation mit Softkey-Eingabegeräten gemäß dem ISO-Bus-Standard 11783-6. Bei der Initialisierung (INIT) werden die Service-Parameter und Objekt-ID konfiguriert. Service-Anfragen (REQ) lösen die entsprechende Funktionalität aus, während Indikationen (IND) eingehende Ereignisse von den physikalischen Softkeys signalisieren.

## Technische Besonderheiten

- Unterstützt ISO 11783-6 Standard für landwirtschaftliche Fahrzeuge
- Verwendet spezifische SoftKeyActivationCode-Struktur zur Ereignisidentifikation
- Integriert Objekt-ID-Verwaltung für Geräteidentifikation
- Bietet umfassende Statusrückmeldungen über STRING-Parameter

## Zustandsübersicht

Der Funktionsblock verfügt über einen initialisierten und einen Betriebszustand. Nach erfolgreicher INIT-Initialisierung wechselt der Block in den Betriebszustand, in dem REQ-Anfragen verarbeitet und IND-Ereignisse empfangen werden können.

## Anwendungsszenarien

- Steuerung von Bedienpanels in landwirtschaftlichen Maschinen
- Implementierung von Softkey-Funktionalitäten in mobilen Arbeitsgeräten
- ISO-Bus-konforme Eingabeverarbeitung in Fahrzeugen
- Benutzerschnittstellen für komplexe Maschinensteuerungen

## ⚖️ Vergleich mit ähnlichen Bausteinen

Im Vergleich zu generischen Eingabebausteinen bietet Softkey_IE spezifische ISO 11783-6-Konformität und ist optimiert für die Anforderungen landwirtschaftlicher und mobiler Arbeitsmaschinen. Die Integration von SoftKeyActivationCode ermöglicht eine standardisierte Ereignisbehandlung.

## 🛠️ Zugehörige Übungen

- [Uebung_010b2](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_010b2/)
- [Uebung_010b2_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010b2_AX/)
- [Uebung_010b6](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_010b6/)
- [Uebung_010b6_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010b6_AX/)
- [Uebung_013](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_013/)
- [Uebung_013_AX](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_013_AX/)
- [Uebung_014](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_014/)
- [Uebung_015](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_015/)
- [Uebung_015a](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_015a/)
- [Uebung_016](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_016/)
- [Uebung_016a](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_016a/)
- [Uebung_017](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_017/)
- [Uebung_018](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_018/)
- [Uebung_018a](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_018a/)
- [Uebung_019a](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_019a/)
- [Uebung_019b](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_019b/)
- [Uebung_021](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_021/)
- [Uebung_022](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_022/)
- [Uebung_023](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_023/)
- [Uebung_024](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_024/)
- [Uebung_025](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_025/)
- [Uebung_026](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_026/)
- [Uebung_039](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_039/)
- [Uebung_039a](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_039a/)
- [Uebung_039a_sub_Outputs](https://docs.ms-muc-docs.de/projects/visual-programming-languages-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_039a_sub_Outputs/)

## Fazit

Der Softkey_IE Funktionsblock stellt eine spezialisierte Lösung für die Softkey-Ereignisverarbeitung in ISO 11783-6-konformen Systemen dar. Durch seine standardisierte Schnittstelle und umfassende Statusrückmeldungen eignet er sich ideal für den Einsatz in komplexen mobilen Arbeitsmaschinen-Steuerungen.
