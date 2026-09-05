# IE

## 🎧 Podcast

- [4diac IDE: Dein "Hello World" der Automatisierung – Das Blinking Tutorial Lokal](https://podcasters.spotify.com/pod/show/eclipse-4diac-de/episodes/4diac-IDE-Dein-Hello-World-der-Automatisierung--Das-Blinking-Tutorial-Lokal-e36971r)
- [4diac IDE: Dein Open-Source-Werkzeugkasten für verteilte Industrieautomatisierung nach IEC 61499](https://podcasters.spotify.com/pod/show/eclipse-4diac-de/episodes/4diac-IDE-Dein-Open-Source-Werkzeugkasten-fr-verteilte-Industrieautomatisierung-nach-IEC-61499-e36821e)
- [4diac IDE: Wie der IEC 61499 Standard die Industrieautomatisierung revolutioniert](https://podcasters.spotify.com/pod/show/eclipse-4diac-de/episodes/4diac-IDE-Wie-der-IEC-61499-Standard-die-Industrieautomatisierung-revolutioniert-e36756a)
- [4diac-Präsentation: Zielgruppen, Struktur und Alleinstellungsmerkmal Schärfen](https://podcasters.spotify.com/pod/show/eclipse-4diac-de/episodes/4diac-Prsentation-Zielgruppen--Struktur-und-Alleinstellungsmerkmal-Schrfen-e38ckbo)
- [Den Software-Drachen zähmen: Industrielle Automatisierung und die Zukunft der Produktion](https://podcasters.spotify.com/pod/show/eclipse-4diac-de/episodes/Den-Software-Drachen-zhmen-Industrielle-Automatisierung-und-die-Zukunft-der-Produktion-e372eg1)

## 📺 Video

- [Die große Migration](https://www.youtube.com/watch?v=XcBu7y6ch4E)
- [Die Kunst des Lötens](https://www.youtube.com/watch?v=I6Srdxx6fzU)
- [Die Welt der Normung](https://www.youtube.com/watch?v=9phDmkJVaGM)
- [Löten wie ein Profi](https://www.youtube.com/watch?v=8ulMWcxaB-c)
- [The secret of the field](https://www.youtube.com/watch?v=MmMrEXum4w4)

## Einleitung

Der IE-Funktionsblock (Input Event) ist ein Service-Interface-Funktionsblock für die Verarbeitung von Ereigniseingangsdaten. Er dient als Schnittstelle zwischen der Steuerungslogik und externen Eingabegeräten oder -signalen und ermöglicht die Initialisierung, Abfrage und Indikation von Eingabeereignissen.

![IE](IE.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **INIT**: Service-Initialisierung - Initialisiert den Funktionsblock mit Parametern
- **REQ**: Service-Anfrage - Löst eine Abfrage des Eingabezustands aus

### **Ereignis-Ausgänge**

- **INITO**: Initialisierungsbestätigung - Bestätigt die erfolgreiche Initialisierung
- **CNF**: Service-Bestätigung - Bestätigt die verarbeitete Service-Anfrage
- **IND**: Indikation von der Ressource - Signalisiert eingehende Ereignisse von der Hardware

### **Daten-Eingänge**

- **QI** (BOOL): Ereignis-Eingangs-Qualifier - Aktiviert/deaktiviert die Ereignisverarbeitung
- **PARAMS** (STRING): Service-Parameter - Konfigurationsparameter für den Service

### **Daten-Ausgänge**

- **QO** (BOOL): Ereignis-Ausgangs-Qualifier - Status der Ereignisverarbeitung
- **STATUS** (STRING): Service-Status - Rückmeldung über den aktuellen Betriebszustand

### **Adapter**

Keine Adapter-Schnittstellen vorhanden.

## Funktionsweise

Der IE-Funktionsblock arbeitet als bidirektionale Schnittstelle für Eingabeereignisse. Bei der Initialisierung (INIT) werden die Service-Parameter konfiguriert. Über REQ-Ereignisse können gezielte Abfragen des Eingabezustands durchgeführt werden. Gleichzeitig kann der Block asynchron IND-Ereignisse generieren, wenn von der Hardware spontan Eingabeereignisse erkannt werden.

## Technische Besonderheiten

- Unterstützt sowohl poll-basierte (REQ/CNF) als auch interrupt-basierte (IND) Betriebsmodi
- String-basierte Parameter- und Statusübertragung für flexible Konfiguration
- Separate Qualifier für Eingangs- und Ausgangsereignisse (QI/QO)
- Robuste Fehlerbehandlung durch STATUS-Rückmeldungen

## Zustandsübersicht

Der Funktionsblock durchläuft folgende Hauptzustände:

1. **Nicht initialisiert**: Block wartet auf INIT-Ereignis
2. **Initialisiert**: Block ist betriebsbereit und kann REQ- und IND-Ereignisse verarbeiten
3. **Abfrage aktiv**: Verarbeitung einer REQ-Anfrage
4. **Indikation aktiv**: Verarbeitung eines spontanen Eingabeereignisses

## Anwendungsszenarien

- Abfrage von digitalen Eingängen (z.B. Taster, Schalter)
- Überwachung von Sensorsignalen
- Schnittstelle zu externen Eingabegeräten
- Ereignisgesteuerte Steuerungsanwendungen
- Hardware-nahe E/A-Verwaltung in Automatisierungssystemen

## ⚖️ Vergleich mit ähnlichen Bausteinen

Im Vergleich zu einfacheren Eingabeblöcken bietet IE erweiterte Funktionalität:

- Gegenüber reinen E/A-Blöcken: Unterstützt sowohl poll- als auch event-basierte Abfragen
- Gegenüber statischen Eingabeblöcken: Dynamische Parametrierung zur Laufzeit
- Erweiterte Statusrückmeldungen für verbesserte Fehlerdiagnose

## 🛠️ Zugehörige Übungen

- [Uebung_004a](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004a/)
- [Uebung_004a2](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004a2/)
- [Uebung_004a2_2](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004a2_2/)
- [Uebung_004a2_3](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004a2_3/)
- [Uebung_004a2_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004a2_AX/)
- [Uebung_004a3](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004a3/)
- [Uebung_004a3_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004a3_AX/)
- [Uebung_004a4](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004a4/)
- [Uebung_004a4_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004a4_AX/)
- [Uebung_004a5](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004a5/)
- [Uebung_004a5_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004a5_AX/)
- [Uebung_004a6](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004a6/)
- [Uebung_004a6_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004a6_AX/)
- [Uebung_004a7](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004a7/)
- [Uebung_004a7_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004a7_AX/)
- [Uebung_004a8](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004a8/)
- [Uebung_004a8_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004a8_AX/)
- [Uebung_004a9](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004a9/)
- [Uebung_004a9_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004a9_AX/)
- [Uebung_004a_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004a_AX/)
- [Uebung_004b](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004b/)
- [Uebung_004b2](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004b2/)
- [Uebung_004b3](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004b3/)
- [Uebung_004b_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004b_AX/)
- [Uebung_004b_AX_ASR](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004b_AX_ASR/)
- [Uebung_004b_AX_ASR_X](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004b_AX_ASR_X/)
- [Uebung_004c1](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004c1/)
- [Uebung_004c1_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004c1_AX/)
- [Uebung_004c2](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004c2/)
- [Uebung_004c2_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004c2_AX/)
- [Uebung_004c3](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004c3/)
- [Uebung_004c3_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004c3_AX/)
- [Uebung_004c4](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004c4/)
- [Uebung_004c4_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004c4_AX/)
- [Uebung_004c5](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_004c5/)
- [Uebung_004c5_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_004c5_AX/)
- [Uebung_006](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_006/)
- [Uebung_006_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_006_AX/)
- [Uebung_006a](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_006a/)
- [Uebung_006a2](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_006a2/)
- [Uebung_006a2_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_006a2_AX/)
- [Uebung_006a3](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_006a3/)
- [Uebung_006a3_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_006a3_AX/)
- [Uebung_006a4](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_006a4/)
- [Uebung_006a4_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_006a4_AX/)
- [Uebung_006a_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_006a_AX/)
- [Uebung_006b](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_006b/)
- [Uebung_006b_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_006b_AX/)
- [Uebung_006d](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_006d/)
- [Uebung_006d_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_006d_AX/)
- [Uebung_007a1](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_007a1/)
- [Uebung_007a1_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_007a1_AX/)
- [Uebung_007a2](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_007a2/)
- [Uebung_007a2_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_007a2_AX/)
- [Uebung_007a3](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_007a3/)
- [Uebung_007a3_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_007a3_AX/)
- [Uebung_009a](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_009a/)
- [Uebung_010b2](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010b2/)
- [Uebung_010b2_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010b2_AX/)
- [Uebung_010b3](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010b3/)
- [Uebung_010b3_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010b3_AX/)
- [Uebung_010b6](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010b6/)
- [Uebung_010b6_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010b6_AX/)
- [Uebung_010b7](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010b7/)
- [Uebung_010b7_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010b7_AX/)
- [Uebung_010b8](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010b8/)
- [Uebung_010b8_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010b8_AX/)
- [Uebung_010b9](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010b9/)
- [Uebung_010b9_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010b9_AX/)
- [Uebung_010bA](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010bA/)
- [Uebung_010bA2](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010bA2/)
- [Uebung_010bA2_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010bA2_AX/)
- [Uebung_010bA3](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010bA3/)
- [Uebung_010bA3_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010bA3_AX/)
- [Uebung_010bA4](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010bA4/)
- [Uebung_010bA4_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010bA4_AX/)
- [Uebung_010bA_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_010bA_AX/)
- [Uebung_013](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_013/)
- [Uebung_013_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_013_AX/)
- [Uebung_014](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_014/)
- [Uebung_015](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_015/)
- [Uebung_015a](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_015a/)
- [Uebung_016](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_016/)
- [Uebung_016a](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_016a/)
- [Uebung_017](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_017/)
- [Uebung_018](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_018/)
- [Uebung_018a](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_018a/)
- [Uebung_019](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_019/)
- [Uebung_019a](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_019a/)
- [Uebung_019b](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_019b/)
- [Uebung_019c](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_019c/)
- [Uebung_020f3](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_020f3/)
- [Uebung_020f3_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_020f3_AX/)
- [Uebung_020h](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_020h/)
- [Uebung_020h_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_020h_AX/)
- [Uebung_020i](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_020i/)
- [Uebung_020i_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_020i_AX/)
- [Uebung_021](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_021/)
- [Uebung_022](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_022/)
- [Uebung_023](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_023/)
- [Uebung_024](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_024/)
- [Uebung_025](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_025/)
- [Uebung_026](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_026/)
- [Uebung_031](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_031/)
- [Uebung_034b](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_034b/)
- [Uebung_035](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_035/)
- [Uebung_035a](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_035a/)
- [Uebung_035a1_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_035a1_AX/)
- [Uebung_035a1b_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_035a1b_AX/)
- [Uebung_035a2](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_035a2/)
- [Uebung_035a2_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_035a2_AX/)
- [Uebung_035a3](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_035a3/)
- [Uebung_035a3_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_035a3_AX/)
- [Uebung_035b](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_035b/)
- [Uebung_035c](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_035c/)
- [Uebung_036](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_036/)
- [Uebung_037](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_037/)
- [Uebung_038](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_038/)
- [Uebung_038_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_038_AX/)
- [Uebung_039](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_039/)
- [Uebung_039a](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_039a/)
- [Uebung_039a_sub_Outputs](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_039a_sub_Outputs/)
- [Uebung_040](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_040/)
- [Uebung_040_2](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_040_2/)
- [Uebung_040_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_040_AX/)
- [Uebung_041](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_041/)
- [Uebung_042](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_042/)
- [Uebung_043](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_043/)
- [Uebung_080](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_080/)
- [Uebung_080b](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_080b/)
- [Uebung_080c](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_080c/)
- [Uebung_081](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_081/)
- [Uebung_082](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_082/)
- [Uebung_083](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_083/)
- [Uebung_083_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_083_AX/)
- [Uebung_084](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_084/)
- [Uebung_085](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_085/)
- [Uebung_087](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_087/)
- [Uebung_087a1](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_087a1/)
- [Uebung_091](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_091/)
- [Uebung_093](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_093/)
- [Uebung_093b](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_093b/)
- [Uebung_094](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_094/)
- [Uebung_094a](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_094a/)
- [Uebung_094a_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_094a_AX/)
- [Uebung_095](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_095/)
- [Uebung_095_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_095_AX/)
- [Uebung_110](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_110/)
- [Uebung_111](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_111/)
- [Uebung_124](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_124/)
- [Uebung_127](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_127/)
- [Uebung_128](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_128/)
- [Uebung_128b](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_128b/)
- [Uebung_132](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_132/)
- [Uebung_150_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_150_AX/)
- [Uebung_151_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_151_AX/)
- [Uebung_152](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_152/)
- [Uebung_153](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_153/)
- [Uebung_160b](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_160b/)
- [Uebung_160b_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_160b_AX/)
- [Uebung_171_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_171_AX/)
- [Uebung_179](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_179/)
- [Uebung_180](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_180/)

## Fazit

Der IE-Funktionsblock stellt eine flexible und robuste Lösung für die Behandlung von Eingabeereignissen in 4diac-basierten Steuerungssystemen dar. Seine Fähigkeit, sowohl synchrone Abfragen als auch asynchrone Indikationen zu verarbeiten, macht ihn besonders geeignet für Anwendungen, die sowohl reaktive als auch proaktive Eingabeverarbeitung erfordern.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
