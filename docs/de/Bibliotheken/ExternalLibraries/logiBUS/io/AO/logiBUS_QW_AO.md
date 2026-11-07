# logiBUS_QW_AO

![logiBUS_QW_AO](./logiBUS_QW_AO.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock `logiBUS_QW_AO` ist ein Ausgabeservice-Interface für eine **Analogspannung** über den Sigma-Delta-Modulator und einen RC-Tiefpass. `OUT` (`WORD`) ist die gewünschte Spannung in mV nach dem Tiefpass, die Pulsdichte läuft mit 10 MHz.

**Voraussetzung:** Auf dem Board oder einer Zusatzplatine muss ein RC-Filter vorhanden sein. Der Baustein ist nicht für die Leistungsausgänge gedacht.

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
| OUT | WORD | Spannung in mV nach dem RC-Tiefpass: VDD_IO / 256 · Dichte + VDD_IO / 2, wobei VDD_IO mit 3300 mV angenommen wird. Bereich 0..3287 mV in 256 Stufen (etwa 13 mV). Höhere Werte werden begrenzt, `STATUS` meldet das. |
| Output | logiBUS::io::DQ::logiBUS_DO_S | Identifiziert den Ausgang Output_Q1..Q8; Initialwert `logiBUS_DO::Invalid` |

### **Daten-Ausgänge**

| Variable | Typ | Kommentar |
|---|---|---|
| QO | BOOL | Ereignis-Ausgangsqualifizierer |
| STATUS | STRING | Service-Status |

## Funktionsweise

Bei `INIT` wird der Ausgang übernommen. Jedes `REQ` rechnet die Spannung `OUT` in eine Pulsdichte (8 Bit) um und gibt sie aus. Werte über 3287 mV werden auf diesen Wert begrenzt, `STATUS` meldet es. `CNF` bestätigt.

## Technische Besonderheiten

- Auflösung 256 Stufen, etwa 13 mV; Bereich 0..3287 mV.
- VDD_IO wird mit 3300 mV angenommen, nicht gemessen.
- Die tatsächliche Spannung hängt vom RC-Filter und der Last ab.
- Quelle: Kommentare der `.fbt` in `logiBUS_io-3.3.0`; das Verhalten der Firmware ist dort beschrieben und hier nicht auf der Hardware geprüft.

## Vergleich

| Baustein | Unterschied |
|---|---|
| `logiBUS_QD_SDM` | gleiche Pulsdichte, aber Wert 0..8191 und 1 MHz; kein Spannungswert in mV |

## Anwendungsszenarien

- Einfacher Analogausgang (Sollwert, Hilfsspannung) mit RC-Filter am Logikausgang.
- Spannungssollwerte in mV für nachgeschaltete Elektronik.
