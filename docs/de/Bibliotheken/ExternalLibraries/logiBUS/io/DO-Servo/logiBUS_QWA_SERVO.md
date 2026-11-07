# logiBUS_QWA_SERVO

![logiBUS_QWA_SERVO](./logiBUS_QWA_SERVO.svg)

![logiBUS_QWA_SERVO_network](./logiBUS_QWA_SERVO_network.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock `logiBUS_QWA_SERVO` ist ein Composite um `logiBUS_QW_SERVO`. Er bekommt die Position über einen Adapter (Socket `OUT`, Typ `AW`, in `D1`) und leitet `FREQ`, `PULSE_MIN` und `PULSE_MAX` bei INIT weiter.

**Signalpegel:** Der Servo braucht ein Logikpegel-Signal. Bei Boards mit Leistungsstufe wird das Signal vor der Leistungsstufe abgegriffen.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Ereignis | Typ | Verknüpft mit / Kommentar |
|---|---|---|
| INIT | EInit | Service-Initialisierung; verknüpft mit QI, PARAMS, Output, FREQ, PULSE_MIN, PULSE_MAX |
| *kein weiterer Ereignis-Eingang* | | *Neue Werte kommen über den Adapter* |

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
| Output | logiBUS::io::DQ::logiBUS_DO_S | Identifiziert den Ausgang Output_Q1..Q8; Initialwert `logiBUS_DO::Invalid` |
| FREQ | UDINT | Wiederholrate des Servoimpulses in Hz, gelesen bei INIT; Initialwert `50` (ein Impuls alle 20 ms). Servos mit gleicher `FREQ` teilen sich einen MCPWM-Timer. |
| PULSE_MIN | TIME | Impulsbreite für `OUT` = 0, gelesen bei INIT (Auflösung 1 µs); Initialwert `T#1000us`. Ist `PULSE_MAX` kleiner als `PULSE_MIN`, kehrt sich die Richtung um. |
| PULSE_MAX | TIME | Impulsbreite für `OUT` = `0xFAFF` (100 %), gelesen bei INIT (Auflösung 1 µs); Initialwert `T#2000us`. |

### **Daten-Ausgänge**

| Variable | Typ | Kommentar |
|---|---|---|
| QO | BOOL | Ereignis-Ausgangsqualifizierer |
| STATUS | STRING | Service-Status |

### **Adapter**

| Typ | Name | Richtung | Kommentar |
|---|---|---|---|
| adapter::types::unidirectional::AW | OUT | Socket | Empfängt Ereignis (E1) und Position 0..0xFAFF (D1) |

## Funktionsweise

Das Composite leitet `INIT` mit `QI`, `PARAMS`, `Output`, `FREQ`, `PULSE_MIN` und `PULSE_MAX` an den internen `logiBUS_QW_SERVO` (`QX`) weiter. Ereignis `E1` und Wert `D1` des Adapters `OUT` gehen als `REQ` und `OUT` an `QX`; `CNF` von `QX` geht nach außen.

## Änderung gegenüber Version 3.2.0

Bis `logiBUS_io` 3.2.0 lief der Servo auf dem LEDC, fest mit 250 Hz und 800 bis 2180 µs. Seit 3.3.0 läuft er auf dem MCPWM mit den Parametern `FREQ`, `PULSE_MIN` und `PULSE_MAX` und den Initialwerten 50 Hz und 1000 bis 2000 µs (`logiBUS_QW_SERVO` Version 1.2, `logiBUS_QWA_SERVO` Version 1.1).

!!! warning "Bestehende Anwendungen"
    Anwendungen, die auf die alten festen Werte gebaut sind, steuern mit den neuen Initialwerten einen kleineren Weg an. Bei einem 9-g-Servo waren es nur noch etwa ±35° statt vorher etwa ±80°. Passende Werte für `FREQ`, `PULSE_MIN` und `PULSE_MAX` stehen in der Tabelle unten und beim jeweiligen Servo.

## Werte für gängige Servos

| Servo | `FREQ` | `PULSE_MIN` | `PULSE_MAX` | Hinweis |
|---|---|---|---|---|
| Initialwerte des Bausteins | 50 | `T#1000us` | `T#2000us` | am Oszilloskop gemessen (Franz, 2026-11-06): 50 Hz, 0 % = 1,0 ms, 50 % = 1,5 ms, 100 % = 2,0 ms |
| 9-g-Servo, 180° ([Hardware-Seite](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/visual-programming-languages-docs/de/latest/hw/logiBUS/Servos/)) | 50 | `T#500us` | `T#2500us` | Herstellerangabe 0,5 bis 2,5 ms für 0 bis 180°; fährt damit 90° in jede Richtung (getestet) |
| Blue Bird BMS-L530MG (Autoservo) | 250 | `T#1100us` | `T#1900us` | Beispiel aus dem Kommentar des Bausteins |
| Werte bis 3.2.0 | 250 | `T#800us` | `T#2180us` | fest im Treiber, nicht mehr Initialwert |

## Hinweise zum Betrieb

- Fährt ein Servo mit zu großer Spanne gegen den mechanischen Anschlag, brummt er und wird warm. Dann `PULSE_MIN` erhöhen und `PULSE_MAX` verringern, jede Grenze für sich (Details auf der [Hardware-Seite](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/visual-programming-languages-docs/de/latest/hw/logiBUS/Servos/)).
- Versorgung des Servos aus einer eigenen Quelle, nicht aus dem 3,3-V-Regler des ESP, mit gemeinsamer Masse (Details auf der [Hardware-Seite](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/visual-programming-languages-docs/de/latest/hw/logiBUS/Servos/)).

## Technische Besonderheiten

- Läuft auf dem MCPWM; Servos mit gleicher `FREQ` teilen sich einen Timer.
- `FREQ`, `PULSE_MIN` und `PULSE_MAX` werden nur bei `INIT` gelesen.
- Auflösung der Impulsbreite 1 µs.
- `OUT` ist `WORD` (0..`0xFAFF`), nicht der volle 16-Bit-Bereich.
- Quelle: Kommentare der `.fbt` in `logiBUS_io-3.3.0`; das Verhalten der Firmware ist dort beschrieben und hier nicht auf der Hardware geprüft.

## Vergleich

| Baustein | Unterschied |
|---|---|
| `logiBUS_QD_PWM` | PWM-Ausgang mit 13 Bit und Rampe; kein Servoimpuls mit festem Zeitfenster |

## Anwendungsszenarien

- Servoansteuerung (Modellbau-Servo, Klappe, Ventilstellung) aus der 4diac-Anwendung.
- Autoservos mit 250 Hz durch passende Werte für `FREQ`, `PULSE_MIN` und `PULSE_MAX`.
