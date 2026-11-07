# logiBUS_QD_SDM

![logiBUS_QD_SDM](./logiBUS_QD_SDM.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock `logiBUS_QD_SDM` ist ein Ausgabeservice-Interface für einen **Sigma-Delta-Ausgang** (Pulsdichtemodulation). Der Wert `OUT` (`DWORD`, 0..8191, 13 Bit wie bei den PWM-Bausteinen) wird als Pulsdichte mit 8 Bit (256 Stufen) bei 1 MHz ausgegeben. Es gibt keine Rampe: Der neue Wert gilt sofort.

**Nur Logikpegel.** Der Baustein ist für LEDs und LED-Treiber gedacht, nicht für die Leistungsausgänge: Die Hardware-Seite [Leistungsstufen der Ausgänge](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/visual-programming-languages-docs/de/latest/hw/logiBUS/Leistungsstufen/) beschreibt, was hinter den Ausgängen sitzt. Die Firmware prüft das nicht.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Ereignis | Typ | Verknüpft mit / Kommentar |
|---|---|---|
| INIT | EInit | Service-Initialisierung; verknüpft mit QI, PARAMS, Output |
| REQ | Event | Service-Anforderung; verknüpft mit QI, OUT |

### **Ereignis-Ausgänge**

| Ereignis | Typ | Verknüpft mit / Kommentar |
|---|---|---|
| INITO | EInit | Initialisierungsbestätigung; verknüpft mit QO, STATUS |
| CNF | Event | Bestätigung der angeforderten Service-Operation; verknüpft mit QO, STATUS |

### **Daten-Eingänge**

| Variable | Typ | Kommentar |
|---|---|---|
| QI | BOOL | Ereignis-Eingangsqualifizierer |
| PARAMS | STRING | Service-Parameter |
| OUT | DWORD | Wert 0..8191 (13 Bit), als Pulsdichte mit 8 Bit ausgegeben |
| Output | logiBUS::io::DQ::logiBUS_DO_S | Identifiziert den Ausgang Output_Q1..Q8; Initialwert `logiBUS_DO::Invalid` |

### **Daten-Ausgänge**

| Variable | Typ | Kommentar |
|---|---|---|
| QO | BOOL | Ereignis-Ausgangsqualifizierer |
| STATUS | STRING | Service-Status |

## Funktionsweise

Bei `INIT` werden Ausgang (und bei `_G` auch `GAMMA`) übernommen. Jedes `REQ` setzt einen neuen Wert `OUT`; er wird sofort als Pulsdichte ausgegeben und mit `CNF` bestätigt.

## Technische Besonderheiten

- Pulsdichte mit 8 Bit (256 Stufen), 1 MHz.
- Wertebereich `OUT`: 0..8191 (13 Bit), keine Rampe.
- Kein LEDC- und kein MCPWM-Timer-Parameter (`FREQ`, `RAMP`) wie bei `logiBUS_QD_PWM`.
- Quelle: Kommentare der `.fbt` in `logiBUS_io-3.3.0`; das Verhalten der Firmware ist dort beschrieben und hier nicht auf der Hardware geprüft.

## Vergleich

| Baustein | Unterschied |
|---|---|
| `logiBUS_QD_PWM` / `logiBUS_QD_PWM_G` | PWM mit Frequenz, Rampe und 13 Bit Auflösung (LEDC); geeignet auch für Leistungsausgänge, soweit die Leistungsstufe es zulässt |
| `logiBUS_QD_SDM` | Pulsdichte mit 8 Bit bei 1 MHz, ohne Rampe, nur Logikpegel |

## Anwendungsszenarien

- Helligkeitssteuerung von LEDs und LED-Treibern am Logikausgang.
- Anwendungen, in denen eine hohe Taktfrequenz wichtiger ist als eine feine Auflösung.
