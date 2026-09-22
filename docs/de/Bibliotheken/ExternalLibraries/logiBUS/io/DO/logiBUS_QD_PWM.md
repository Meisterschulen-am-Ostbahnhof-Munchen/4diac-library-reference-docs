# logiBUS_QD_PWM

<img width="1848" height="333" alt="image" src="https://github.com/user-attachments/assets/ea4b0496-56de-4eb9-a419-6cd8c9b095bb" />

* * * * * * * * * *

## Einleitung

Der Funktionsblock `logiBUS_QD_PWM` ist ein Ausgabeservice-Interface-Funktionsblock für doppelte Wort-Ausgabedaten. Er dient als Schnittstelle zur Steuerung von PWM-Ausgaben (Pulsweitenmodulation) über das logiBUS-System und ermöglicht die Ansteuerung von Ausgängen Q1 bis Q8.

![logiBUS_QD_PWM](logiBUS_QD_PWM.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **INIT**: Service-Initialisierung
  - Verknüpft mit: QI, PARAMS, Output
- **REQ**: Service-Anforderung
  - Verknüpft mit: QI, OUT

### **Ereignis-Ausgänge**

- **INITO**: Initialisierungsbestätigung
  - Verknüpft mit: QO, STATUS
- **CNF**: Bestätigung der angeforderten Service-Operation
  - Verknüpft mit: QO, STATUS

### **Daten-Eingänge**

- **QI** (BOOL): Ereignis-Eingangsqualifizierer
- **PARAMS** (STRING): Service-Parameter
- **OUT** (DWORD): Ausgabedaten zur Ressource (13-Bit-PWM-Rohwert `0` bis `8191`, entsprechend `0 %` bis `100 %` Tastgrad, $2^{13} = 8192$ Zustände)
- **Output** (logiBUS_DO_S): Identifiziert den Ausgang Output_Q1..Q8
  - Initialwert: `logiBUS_DO::Invalid`

### **Daten-Ausgänge**

- **QO** (BOOL): Ereignis-Ausgangsqualifizierer
- **STATUS** (STRING): Service-Status

### **Adapter**

Keine Adapter-Schnittstellen vorhanden.

## Funktionsweise

Der Funktionsblock ermöglicht die PWM-Steuerung von Ausgängen über das logiBUS-System. Bei der Initialisierung (INIT) werden die Service-Parameter konfiguriert und der spezifische Ausgang identifiziert. Über die REQ-Anforderung können PWM-Daten (`OUT`, 13-Bit-Wert `0` bis `8191` in einer DWORD-Variable) an den konfigurierten Ausgang gesendet werden. Der Block bestätigt sowohl Initialisierung als auch Service-Anforderungen über die entsprechenden Ausgangsereignisse.

## Technische Besonderheiten

- **13-Bit-PWM-Auflösung**: Verwendet eine 13-Bit-Normierung (`0` bis `8191`) im `DWORD`-Datentyp für die PWM-Ausgabe (`0` = 0 % Tastgrad, `8191` = 100 % Tastgrad, Skalierungsfaktor für ISO-Designer: `0.0122085215480405` bzw. `100 / 8191`).
- Unterstützt bis zu 8 Ausgänge (Q1-Q8) über die Output-Konfiguration
- String-basierte Parameterkonfiguration für flexible Service-Einstellungen
- Statusrückmeldung über STRING-Variable für detaillierte Fehlerinformationen

## Zustandsübersicht

Der Funktionsblock verfügt über zwei Hauptzustände:

1. **Nicht initialisiert**: Block wartet auf INIT-Ereignis
2. **Initialisiert und betriebsbereit**: Block kann REQ-Anforderungen verarbeiten und PWM-Daten ausgeben

## Anwendungsszenarien

- Steuerung von PWM-gesteuerten Aktoren (Motoren, Heizelemente)
- Ansteuerung von LED-Beleuchtung mit Helligkeitssteuerung
- Regelung von Ventilen mit proportionaler Steuerung
- Industrielle Automatisierungsanwendungen mit logiBUS-Hardware

## ⚖️ Vergleich mit ähnlichen Bausteinen

Im Vergleich zu einfachen digitalen Ausgabeblöcken bietet `logiBUS_QD_PWM` eine präzise PWM-Steuerung mit 13-Bit-Auflösung (`0`–`8191` im DWORD). Gegenüber analogen Ausgabeblöcken ermöglicht er die direkte PWM-Steuerung ohne zusätzliche Wandlung.

## 🛠️ Zugehörige Übungen

- [Uebung_034](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_034/)
- [Uebung_034a1_Q1](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_034a1_Q1/)
- [Uebung_034a1_Q2](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_034a1_Q2/)
- [Uebung_034a1_Q4](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_034a1_Q4/)
- [Uebung_034b](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_034b/)
- [Uebung_152](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_152/)
- [Uebung_153](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs-de/en/latest/Uebungen/test_B/Uebungen_doc/Uebung_153/)

## Fazit

Der `logiBUS_QD_PWM` Funktionsblock stellt eine leistungsstarke Schnittstelle für PWM-Ausgaben im logiBUS-System bereit. Durch die flexible Konfiguration und die Unterstützung für 13-Bit-PWM-Daten (`0`–`8191`) eignet er sich ideal für präzise Steuerungsanwendungen in industriellen Automatisierungssystemen.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Das PWM-Signal & Infografik auf ms-muc-docs.de](https://www.ms-muc-docs.de/automatisierung/das-pwm-signal-die-kunst-spannung-zu-zerhacken/das-pwm-signal-die-kunst-spannung-zu-zerhacken-website/)
