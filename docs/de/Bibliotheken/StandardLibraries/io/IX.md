# IX

## 🎧 Podcast

- [Infineon MOTIX BTM9020/9021EP: Datenblatt-Analyse für Automotive – Robuster Motortreiber mit intelligenter Diagnose (HW vs. SPI)](https://podcasters.spotify.com/pod/show/ms-muc-lama/episodes/Infineon-MOTIX-BTM90209021EP-Datenblatt-Analyse-fr-Automotive--Robuster-Motortreiber-mit-intelligenter-Diagnose-HW-vs--SPI-e39av51)
- [integrierten Vollbrücken-ICs MOTIX™ BTM9020EP](https://podcasters.spotify.com/pod/show/ms-muc-lama/episodes/integrierten-Vollbrcken-ICs-MOTIX-BTM9020EP-e368kse)

## Einleitung

Der IX-Funktionsblock ist ein Service-Interface-Funktionsblock für boolesche Eingabedaten. Er dient als Schnittstelle zur Kommunikation mit Eingabegeräten und ermöglicht die Abfrage und Verarbeitung von digitalen Eingangssignalen in 4diac-Systemen.

![IX](IX.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **INIT**: Service-Initialisierung - Initialisiert den Funktionsblock und konfiguriert die Hardware-Parameter
- **REQ**: Service-Anfrage - Fordert eine Abfrage des aktuellen Eingangswerts an

### **Ereignis-Ausgänge**

- **INITO**: Initialisierungsbestätigung - Bestätigt die erfolgreiche Initialisierung
- **CNF**: Bestätigung der angeforderten Service - Bestätigt eine erfolgreiche Abfrage
- **IND**: Indikation von der Ressource - Signalisiert eine Zustandsänderung des Eingangssignals

### **Daten-Eingänge**

- **QI**: Event-Input-Qualifier (BOOL) - Steuert die Aktivierung der Service-Funktionalität
- **PARAMS**: Service-Parameter (STRING) - Enthält Konfigurationsparameter für die Hardware-Schnittstelle

### **Daten-Ausgänge**

- **QO**: Event-Output-Qualifier (BOOL) - Zeigt den Status der Service-Ausführung an
- **STATUS**: Service-Status (STRING) - Liefert Statusinformationen über die Service-Ausführung
- **IN**: Eingabedaten von der Ressource (BOOL) - Enthält den aktuellen Wert des digitalen Eingangs

### **Adapter**

Keine Adapter-Schnittstellen vorhanden.

## Funktionsweise

Der IX-Funktionsblock dient als Vermittler zwischen der Steuerungslogik und physischen Eingabegeräten. Bei der Initialisierung (INIT) werden die Hardware-Parameter konfiguriert. Anschließend kann über REQ-Ereignisse der aktuelle Zustand des Eingangs abgefragt werden. Der Block kann sowohl poll-basierte Abfragen (REQ/CNF) als auch ereignisbasierte Benachrichtigungen (IND) bei Zustandsänderungen verarbeiten.

## Technische Besonderheiten

- Unterstützt sowohl anforderungsbasierte als auch ereignisgesteuerte Betriebsmodi
- Boolescher Datentyp für einfache digitale Eingänge
- Flexible Parameterkonfiguration über STRING-Parameter
- Umfassende Statusrückmeldung für Fehlerdiagnose

## Zustandsübersicht

Der Funktionsblock durchläuft folgende Hauptzustände:

1. **Nicht initialisiert**: Block ist inaktiv
2. **Initialisiert**: Block ist betriebsbereit nach erfolgreicher INIT-Verarbeitung
3. **Abfrage aktiv**: Verarbeitung einer REQ-Anfrage
4. **Indikationsbereit**: Bereit für ereignisgesteuerte Benachrichtigungen

## Anwendungsszenarien

- Abfrage von digitalen Sensoren (Endschalter, Näherungsschalter)
- Überwachung von Taster-Eingängen
- Lesen von Schalterstellungen
- Digitale Signalverarbeitung in Automatisierungssystemen

## ⚖️ Vergleich mit ähnlichen Bausteinen

Im Vergleich zu anderen Eingabe-Bausteinen bietet IX eine erweiterte Service-Schnittstelle mit sowohl poll-basierten als auch ereignisgesteuerten Betriebsmodi. Während einfachere Eingabeblöcke oft nur direkte Werte liefern, bietet IX zusätzliche Statusinformationen und Fehlerbehandlung.

## 🛠️ Zugehörige Übungen

- [Uebung_001](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_001/)
- [Uebung_001c](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_001c/)
- [Uebung_002](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_002/)
- [Uebung_002a](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_002a/)
- [Uebung_002a2](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_002a2/)
- [Uebung_002a3](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_002a3/)
- [Uebung_002a4](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_002a4/)
- [Uebung_002a5b](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_002a5b/)
- [Uebung_002b2](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_002b2/)
- [Uebung_002b3](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_002b3/)
- [Uebung_003](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_003/)
- [Uebung_003a0](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_003a0/)
- [Uebung_003a_sub](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_003a_sub/)
- [Uebung_003b2_sub](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_003b2_sub/)
- [Uebung_003b_sub](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_003b_sub/)
- [Uebung_003c_sub](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_003c_sub/)
- [Uebung_003d](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_003d/)
- [Uebung_005](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_005/)
- [Uebung_006e1](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_006e1/)
- [Uebung_006e2](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_006e2/)
- [Uebung_010](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010/)
- [Uebung_010a](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010a/)
- [Uebung_010a2](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010a2/)
- [Uebung_010a3](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010a3/)
- [Uebung_010a4](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010a4/)
- [Uebung_010b1](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010b1/)
- [Uebung_010b4_sub](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010b4_sub/)
- [Uebung_010b5_sub](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010b5_sub/)
- [Uebung_010c](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010c/)
- [Uebung_010c2](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010c2/)
- [Uebung_010c3_sub](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010c3_sub/)
- [Uebung_010c4_sub](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_010c4_sub/)
- [Uebung_019c](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_019c/)
- [Uebung_020a](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_020a/)
- [Uebung_020b](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_020b/)
- [Uebung_020c](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_020c/)
- [Uebung_020c2](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_020c2/)
- [Uebung_020c3](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_020c3/)
- [Uebung_020d](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_020d/)
- [Uebung_020e](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_020e/)
- [Uebung_020e2](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_020e2/)
- [Uebung_020f](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_020f/)
- [Uebung_020f2](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_020f2/)
- [Uebung_020g](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_020g/)
- [Uebung_020i](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_020i/)
- [Uebung_028](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_028/)
- [Uebung_029](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_029/)
- [Uebung_030](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_030/)
- [Uebung_032](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_032/)
- [Uebung_033_sub](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_033_sub/)
- [Uebung_039_sub_Outputs](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_039_sub_Outputs/)
- [Uebung_039b](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_039b/)
- [Uebung_049](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_049/)
- [Uebung_051](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_051/)
- [Uebung_052](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_052/)
- [Uebung_053](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_053/)
- [Uebung_054](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_054/)
- [Uebung_055](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_055/)
- [Uebung_056](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_056/)
- [Uebung_085](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_085/)
- [Uebung_086](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_086/)
- [Uebung_087](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_087/)
- [Uebung_087a1](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_087a1/)
- [Uebung_087a2](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_087a2/)
- [Uebung_088](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_088/)
- [Uebung_089](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_089/)
- [Uebung_090a1](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_090a1/)
- [Uebung_090a1_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_090a1_AX/)
- [Uebung_090a2](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_090a2/)
- [Uebung_090a2_AX](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_AX/Uebungen_doc/Uebung_090a2_AX/)
- [Uebung_094](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_094/)
- [Uebung_094a](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_094a/)
- [Uebung_095](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_095/)
- [Uebung_160](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_160/)
- [Uebung_160b2](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_160b2/)
- [Uebung_177](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_177/)
- [Uebung_178](https://docs.ms-muc-docs.de/projects/4diac-exercises-docs/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_178/)

## Fazit

Der IX-Funktionsblock stellt eine robuste und flexible Lösung für die Integration boolescher Eingabedaten in 4diac-Systeme dar. Seine umfassende Fehlerbehandlung und flexible Betriebsmodi machen ihn besonders geeignet für zuverlässige Automatisierungsanwendungen mit digitalen Eingangssignalen.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
