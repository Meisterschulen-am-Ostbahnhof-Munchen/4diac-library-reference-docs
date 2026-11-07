# logiBUS_QD_SDM_G

![logiBUS_QD_SDM_G](./logiBUS_QD_SDM_G.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock `logiBUS_QD_SDM_G` ist wie `logiBUS_QD_SDM` ein Sigma-Delta-Ausgang, rechnet den Wert aber mit einer Gammakorrektur für LEDs um: `OUT` (0..8191) ist die Helligkeit, wie das Auge sie sieht, die Pulsdichte folgt (OUT / 8191)^GAMMA. Ausgabe mit 8 Bit (256 Stufen) bei 1 MHz, ohne Rampe.

**Nur Logikpegel.** Der Baustein ist für LEDs und LED-Treiber gedacht, nicht für die Leistungsausgänge: Die Hardware-Seite [Leistungsstufen der Ausgänge](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/visual-programming-languages-docs/de/latest/hw/logiBUS/Leistungsstufen/) beschreibt, was hinter den Ausgängen sitzt. Die Firmware prüft das nicht.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Ereignis | Typ | Verknüpft mit / Kommentar |
|---|---|---|
| INIT | EInit | Service-Initialisierung; verknüpft mit QI, PARAMS, Output, GAMMA |
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
| OUT | DWORD | Helligkeit (0..8191), mit GAMMA umgerechnet |
| Output | logiBUS::io::DQ::logiBUS_DO_S | Identifiziert den Ausgang Output_Q1..Q8; Initialwert `logiBUS_DO::Invalid` |
| GAMMA | REAL | Gamma der Helligkeitskurve, gelesen bei INIT: Pulsdichte aus (OUT / 8191)^GAMMA. `1.0` = linear, `2.2` = sRGB (Initialwert). Ungültige Werte (≤ 0, nicht endlich) laufen linear wie `1.0`. |

### **Daten-Ausgänge**

| Variable | Typ | Kommentar |
|---|---|---|
| QO | BOOL | Ereignis-Ausgangsqualifizierer |
| STATUS | STRING | Service-Status |

## Funktionsweise

Bei `INIT` werden Ausgang und `GAMMA` übernommen. Jedes `REQ` rechnet `OUT` mit (OUT / 8191)^GAMMA in die Pulsdichte um und gibt sie sofort aus; `CNF` bestätigt. Ein ungültiges `GAMMA` (≤ 0 oder nicht endlich) läuft linear wie `1.0`.

## Technische Besonderheiten

- Pulsdichte mit 8 Bit (256 Stufen), 1 MHz.
- Wertebereich `OUT`: 0..8191 (13 Bit), keine Rampe.
- Kein LEDC- und kein MCPWM-Timer-Parameter (`FREQ`, `RAMP`) wie bei `logiBUS_QD_PWM`.
- Mit 8 Bit werden die untersten Stufen der Gammakurve grob; das steht im Kommentar des Bausteins.
- Quelle: Kommentare der `.fbt` in `logiBUS_io-3.3.0`; das Verhalten der Firmware ist dort beschrieben und hier nicht auf der Hardware geprüft.

## Vergleich

| Baustein | Unterschied |
|---|---|
| `logiBUS_QD_PWM` / `logiBUS_QD_PWM_G` | PWM mit Frequenz, Rampe und 13 Bit Auflösung (LEDC); geeignet auch für Leistungsausgänge, soweit die Leistungsstufe es zulässt |
| `logiBUS_QD_SDM_G` | Pulsdichte mit 8 Bit bei 1 MHz, ohne Rampe, nur Logikpegel |

## Anwendungsszenarien

- Helligkeitssteuerung von LEDs und LED-Treibern am Logikausgang.
- Anwendungen, in denen eine hohe Taktfrequenz wichtiger ist als eine feine Auflösung.
