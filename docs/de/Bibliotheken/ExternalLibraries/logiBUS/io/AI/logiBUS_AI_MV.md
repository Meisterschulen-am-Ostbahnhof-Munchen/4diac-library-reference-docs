# logiBUS_AI_MV

![logiBUS_AI_MV](./logiBUS_AI_MV.svg)

* * * * * * * * * *

## Einleitung

`logiBUS_AI_MV` ist ein Service-Interface-Funktionsblock für einen analogen Eingang in Millivolt. Er arbeitet wie [`logiBUS_AI_ID`](logiBUS_AI_ID.md), aber `IN` liefert die kalibrierte Spannung am Pin statt des Rohwerts.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Ereignis | Typ   | Mit                                                                 | Beschreibung                 |
| -------- | ----- | ------------------------------------------------------------------- | ---------------------------- |
| INIT     | EInit | QI, PARAMS, Input, AnalogInput_hysteresis, TimeDelta, TimeRateLimit | Service-Initialisierung      |
| REQ      | Event | QI                                                                  | Service-Anfrage (sofortige Abfrage) |

### **Ereignis-Ausgänge**

| Ereignis | Typ   | Mit                | Beschreibung                                 |
| -------- | ----- | ------------------ | -------------------------------------------- |
| INITO    | EInit | QO, STATUS         | Bestätigung der Initialisierung              |
| CNF      | Event | QO, STATUS, IN     | Bestätigung der angeforderten Service-Operation |
| IND      | Event | QO, STATUS, IN     | Indikation von der Resource                  |

### **Daten-Eingänge**

| Name                   | Typ                           | Initialwert | Beschreibung                                                                            |
| ---------------------- | ----------------------------- | ----------- | --------------------------------------------------------------------------------------- |
| QI                     | BOOL                          | –           | Event-Input-Qualifier                                                                   |
| PARAMS                 | STRING                        | –           | Service-Parameter                                                                       |
| Input                  | logiBUS::io::AI::logiBUS_AI_S | Invalid     | Auswahl des Eingangs (I1..I8)                                                           |
| AnalogInput_hysteresis | DWORD                         | –           | IND, wenn sich die Spannung um mehr als so viele mV ändert (0 = aus, 1 = jede Änderung) |
| TimeDelta              | DWORD                         | 250         | Zyklisches IND spätestens nach so vielen ms (0 oder 16#FFFFFFFF = aus)                  |
| TimeRateLimit          | DWORD                         | 100         | Höchstens ein IND pro so vielen ms (kleiner als TimeDelta)                              |

### **Daten-Ausgänge**

| Name   | Typ    | Beschreibung                                                |
| ------ | ------ | ----------------------------------------------------------- |
| QO     | BOOL   | Event-Output-Qualifier                                      |
| STATUS | STRING | Service-Status                                              |
| IN     | DWORD  | Eingabedaten: kalibrierte Spannung am Pin in Millivolt      |

### **Adapter**

Keine Adapter-Schnittstellen vorhanden.

## Funktionsweise

`IN` ist die Spannung am Pin des Chips in **Millivolt**. Die Firmware rechnet den gemittelten Rohwert mit der Kalibrierung des Chips um (ESP-IDF `adc_cali`, Werte aus dem eFuse). Die Dämpfung ist dieselbe wie bei `logiBUS_AI_ID`: ESP32-S3 etwa 0-2900 mV (±50 mV), ESP32-P4 etwa 0-3300 mV (±15 mV), laut Datenblatt.

Die Spannung an der Klemme hängt vom Spannungsteiler der Platine ab und ist nicht eingerechnet.

Hysterese und Zeitsteuerung funktionieren wie bei `logiBUS_AI_ID`, nur gilt `AnalogInput_hysteresis` in mV:

- `AnalogInput_hysteresis = 0` schaltet den Änderungs-Trigger aus (mit `1` kommt jede Änderung).
- `TimeDelta = 0` oder `16#FFFFFFFF` schaltet das zyklische IND aus.
- Sind beide aus, sendet der Baustein kein IND, der Wert kommt nur über REQ.

## Technische Besonderheiten

- **Kalibrierung nötig:** Gibt es für den Kanal keine Kalibrierung, meldet INIT `QO` = FALSE und `STATUS` = `ADC calibration not available`. Das betrifft den ESP32-S31: Mit ESP-IDF 6.1 gibt es dafür noch keine Kalibrierung. Auf ESP32-S3 und ESP32-P4 läuft der Baustein.
- **Einheit:** `IN` und `AnalogInput_hysteresis` sind beide in mV. Für den Rohwert (0-4095) gibt es [`logiBUS_AI_ID`](logiBUS_AI_ID.md).

## Vergleich mit ähnlichen Bausteinen

- **[`logiBUS_AI_ID`](logiBUS_AI_ID.md):** Gleiche Schnittstelle, aber `IN` ist der Rohwert und die Hysterese gilt im Rohwert.
- **[`logiBUS_AI_MVA`](logiBUS_AI_MVA.md):** Composite um diesen Baustein mit Adapter-Ausgang `IN` und externem Anfrage-Socket `SREQ`.
