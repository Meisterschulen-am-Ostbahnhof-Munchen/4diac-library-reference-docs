# logiBUS_QD_PWM

* * * * * * * * * *

## Einleitung

Der Funktionsblock `logiBUS_QD_PWM` ist ein Ausgabeservice-Interface-Funktionsblock für doppelte Wort-Ausgabedaten. Er dient als Schnittstelle zur Steuerung von PWM-Ausgaben (Pulsweitenmodulation) über das logiBUS-System und ermöglicht die Ansteuerung von Ausgängen Q1 bis Q8.

![logiBUS_QD_PWM](logiBUS_QD_PWM.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **INIT**: Service-Initialisierung
  - Verknüpft mit: QI, PARAMS, Output, FREQ, RAMP
- **REQ**: Service-Anforderung
  - Verknüpft mit: QI, OUT, RAMP

### **Ereignis-Ausgänge**

- **INITO**: Initialisierungsbestätigung
  - Verknüpft mit: QO, STATUS
- **CNF**: Bestätigung der angeforderten Service-Operation
  - Verknüpft mit: QO, STATUS
- **RAMPO**: Rampe beendet, Wert erreicht (bei `RAMP = T#0ms` direkt nach dem Sprung, nicht nach INIT)
  - Verknüpft mit: QO

### **Daten-Eingänge**

- **QI** (BOOL): Ereignis-Eingangsqualifizierer
- **PARAMS** (STRING): Service-Parameter
- **OUT** (DWORD): Ausgabedaten zur Ressource (13-Bit-PWM-Rohwert `0` bis `8191`, entsprechend `0 %` bis `100 %` Tastgrad, $2^{13} = 8192$ Zustände)
- **Output** (logiBUS_DO_S): Identifiziert den Ausgang Output_Q1..Q8
  - Initialwert: `logiBUS_DO::Invalid`
- **FREQ** (UDINT): PWM-Frequenz in Hz, gelesen bei INIT. Initialwert: `400`
- **RAMP** (TIME): Übergangszeit zum neuen Wert (Hardware-Rampe), gelesen bei INIT und REQ. Initialwert: `T#0ms` (Sprung)

### **Daten-Ausgänge**

- **QO** (BOOL): Ereignis-Ausgangsqualifizierer
- **STATUS** (STRING): Service-Status

### **Adapter**

Keine Adapter-Schnittstellen vorhanden.

## Funktionsweise

Der Funktionsblock ermöglicht die PWM-Steuerung von Ausgängen über das logiBUS-System. Bei der Initialisierung (INIT) werden die Service-Parameter konfiguriert und der spezifische Ausgang identifiziert. Über die REQ-Anforderung können PWM-Daten (`OUT`, 13-Bit-Wert `0` bis `8191` in einer DWORD-Variable) an den konfigurierten Ausgang gesendet werden. Der Block bestätigt sowohl Initialisierung als auch Service-Anforderungen über die entsprechenden Ausgangsereignisse.

## Frequenz und Rampe (ab Version 1.2)

- **`FREQ`** (UDINT, in Hz, Standard `400`): PWM-Frequenz, wird nur bei `INIT` gelesen. Eine neue Frequenz braucht ein neues `INIT`. Ausgänge mit gleicher Frequenz teilen sich einen LEDC-Timer. Es gibt 4 Timer, auch das LED-Blinken belegt einen.
- **`RAMP`** (TIME, Standard `T#0ms`): Übergangszeit zum neuen Wert als Hardware-Rampe, blockiert nicht. `T#0ms` ist ein Sprung. `RAMP` wird bei `INIT` und bei `REQ` gelesen.
- **`RAMPO`** (Ereignis-Ausgang, mit `QO`): Der Wert ist erreicht. Das ist das Ende der Rampe, bei `T#0ms` direkt nach dem Sprung. Nach `INIT` kommt kein `RAMPO`.
- Der Tastgrad bleibt `0` bis `8191` (13 Bit).
- Quelle dieser Aussagen: Kommentare in der Bausteinbeschreibung (`.fbt`) und `PWM_KONZEPT.md`; das Verhalten der Firmware ist dort beschrieben und hier nicht geprüft.

### Hinweise zu den Leistungsstufen

Die Firmware prüft die Frequenz nicht gegen die Leistungsstufe des Boards. Die folgenden Grenzen sind nur ein Hinweis (Quelle: Kommentar zu `FREQ` im Baustein und `PWM_KONZEPT.md`):

| Leistungsstufe | Hinweis |
|---|---|
| APIXON 15 PWR Q1 (BTH50030) | höchstens 100 Hz |
| BTS7030, BTT6030, BTH6070 | für Ventile 100 bis 200 Hz |
| Trainer20 | ohne Leistungsstufe |

Welches Board welche Leistungsstufe trägt, steht noch nicht fest und ist hier nicht dokumentiert.

## Änderung gegenüber Version 1.1

- **Neu:** Eingänge `FREQ` und `RAMP`, Ereignis `RAMPO`.
- **Verhaltensänderung:** Der Standardwert von `RAMP` ist `T#0ms` (Sprung). Bisher ging jeder neue Wert als feste Rampe über 500 ms an den Ausgang. Bestehende Programme springen deshalb jetzt auf den neuen Wert. Wer das weiche Anfahren behalten will, setzt `RAMP = T#500ms`.
- **Voraussetzung:** Die ELF `logiBUS_io-3.1.0` (`-xtensa.elf` für S3, `-riscv32.elf` für P4/S31) braucht eine Firmware mit den neuen Host-Symbolen `elf_isr_ledc_fade_end`, `ledc_cb_register`, `ledc_fade_stop`, `ledc_get_duty`, `ledc_timer_pause`, `lroundf` und `powf`; auf P4/S31 zusätzlich `ledc_fill_multi_fade_param_list` und `ledc_set_multi_fade` (Host-Tabellen: LOGIBUS `3142a703`). Auf älterer Firmware lädt die ELF nicht, das Laden bricht wegen des fehlenden Symbols ab. `FORTE_ABI` bleibt `6`, daran ist der Unterschied nicht zu erkennen. Firmware und ELF immer zusammen ausliefern. (Angabe aus der Entwicklung, nicht an der Firmware geprüft.)
- **Stand:** Noch nicht auf Hardware getestet.

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

- [Uebung_034](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_034/)
- [Uebung_034a1_Q1](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_034a1_Q1/)
- [Uebung_034a1_Q2](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_034a1_Q2/)
- [Uebung_034a1_Q4](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_034a1_Q4/)
- [Uebung_034b](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_034b/)
- [Uebung_152](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_152/)
- [Uebung_153](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/de/latest/Uebungen/test_B/Uebungen_doc/Uebung_153/)

## Fazit

Der `logiBUS_QD_PWM` Funktionsblock stellt eine leistungsstarke Schnittstelle für PWM-Ausgaben im logiBUS-System bereit. Durch die flexible Konfiguration und die Unterstützung für 13-Bit-PWM-Daten (`0`–`8191`) eignet er sich ideal für präzise Steuerungsanwendungen in industriellen Automatisierungssystemen.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Das PWM-Signal & Infografik auf ms-muc-docs.de](https://www.ms-muc-docs.de/automatisierung/das-pwm-signal-die-kunst-spannung-zu-zerhacken/das-pwm-signal-die-kunst-spannung-zu-zerhacken-website/)
