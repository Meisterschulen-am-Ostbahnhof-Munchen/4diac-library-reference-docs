# logiBUS_QDA_FREQ

![logiBUS_QDA_FREQ](./logiBUS_QDA_FREQ.svg)

![logiBUS_QDA_FREQ_network](./logiBUS_QDA_FREQ_network.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock `logiBUS_QDA_FREQ` ist ein Composite um `logiBUS_QD_FREQ`. Er bekommt die Frequenz über einen Adapter (Socket `FREQ`, Typ `AD`) und eignet sich damit für die Verschaltung mit anderen Bausteinen ohne eigene Ereignisverdrahtung. Ausgang: Rechtecksignal mit festem Tastgrad von 50 %, Frequenz in Millihertz (`1000` = 1 Hz).

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Ereignis | Typ | Verknüpft mit / Kommentar |
|---|---|---|
| INIT | EInit | Service-Initialisierung; verknüpft mit QI, PARAMS, Output, FMIN, FMAX |
| *kein weiterer Ereignis-Eingang* | | *Neue Frequenzen kommen über den Adapter* |

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
| FMIN | DWORD | Niedrigste Frequenz in mHz, gelesen bei INIT; Initialwert `10000` (10 Hz). Legt die Auflösung des Timers fest. |
| FMAX | DWORD | Höchste Frequenz in mHz, gelesen bei INIT; Initialwert `10000000` (10 kHz). Höhere `FREQ` werden begrenzt. |

### **Daten-Ausgänge**

| Variable | Typ | Kommentar |
|---|---|---|
| QO | BOOL | Ereignis-Ausgangsqualifizierer |
| STATUS | STRING | Service-Status |

### **Adapter**

| Typ | Name | Richtung | Kommentar |
|---|---|---|---|
| adapter::types::unidirectional::AD | FREQ | Socket | Empfängt Ereignis (E1) und Frequenz in mHz (D1); `0` = aus |

## Funktionsweise

Das Composite leitet `INIT` mit `QI`, `PARAMS`, `Output`, `FMIN` und `FMAX` an den internen `logiBUS_QD_FREQ` (`QX`) weiter. Das Ereignis `E1` und der Wert `D1` des Adapters `FREQ` gehen als `REQ` und `FREQ` an `QX`. `CNF` von `QX` geht nach außen. `FMIN` und `FMAX` werden, wie alle Composite-Eingänge, nur bei `INIT` übernommen.

## Auflösung, `FMIN` und `FMAX`

Der Frequenzausgang läuft auf dem MCPWM mit einem eigenen Timer je Ausgang und belegt keinen LEDC-Timer (Quelle: `PWM_KONZEPT.md`). Der Timer ist 16 Bit breit. `FMIN` legt seine Auflösung fest, ungefähr `FMIN` · 65536. Je höher `FREQ` über `FMIN` liegt, desto gröber sind die Schritte: Die Schrittweite ist etwa `FREQ`² / Auflösung. Mit dem Initialwert `FMIN` = 10 Hz nennt der Kommentar des Bausteins als Beispiel eine Auflösung von 640 kHz, eine Schrittweite von 1,6 Hz bei 1 kHz und 156 Hz bei 10 kHz.

`FMAX` begrenzt die Frequenz nach oben, aber nie über das, was der Chip mit der Auflösung aus `FMIN` erzeugen kann (die Hälfte der Auflösung). Frequenzen außerhalb von `FMIN` bis `FMAX` werden begrenzt, und `STATUS` meldet es. `FREQ = 0` ist ein Sonderwert und bedeutet „Ausgang aus“; der Kommentar des Bausteins nennt `0` getrennt von der Begrenzung. Ob `0` den `STATUS` verändert oder auf `FMIN` angehoben wird, steht dort nicht und ist hier nicht geprüft.

## Leistungsstufen

Die Firmware prüft die Frequenz nicht gegen die Leistungsstufe hinter dem Ausgang. Grenzen und Abschaltenergie stehen auf der Hardware-Seite [Leistungsstufen der Ausgänge](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/visual-programming-languages-docs/de/latest/hw/logiBUS/Leistungsstufen/).

## Technische Besonderheiten

- Tastgrad fest 50 %, kein Eingang dafür.
- Frequenz in mHz als `DWORD` (Bereich der DDI 583: `0` bis `2147483647`).
- `QI = FALSE` setzt den Ausgang laut Konzept auf 0 beziehungsweise low.
- Quelle: Kommentare der `.fbt` und `PWM_KONZEPT.md`; das Verhalten der Firmware ist dort beschrieben und hier nicht geprüft.

## Vergleich

| Baustein | Unterschied |
|---|---|
| `logiBUS_QD_PWM` | feste Frequenz (bei INIT), veränderlicher Tastgrad |
| `logiBUS_QDA_FREQ` | veränderliche Frequenz, fester Tastgrad 50 % |
