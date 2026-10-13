# FB_MM710_IMU

![FB_MM710_IMU](./FB_MM710_IMU.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock **FB_MM710_IMU** ist ein serviceorientierter Baustein (SIFB) zur Anbindung des Bosch MM7.10 IMU‑Sensors über CAN/J1939. Er ermöglicht das Auslesen von Beschleunigungs‑, Drehraten‑ und Neigungswerten sowie die Überwachung von System‑ und Fehlerzuständen. Der FB kapselt die gesamte CAN‑Kommunikation und Signalverarbeitung und stellt die Daten standardisiert über Ereignis‑ und Datenausgänge zur Verfügung.

## Schnittstellenstruktur

### **Ereignis‑Eingänge**

| Ereignis | Typ   | Beschreibung                                                                                                                                           |
| -------- | ----- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| INIT     | EInit | Initialisierung des Bausteins. Mit diesem Ereignis werden die CAN‑Parameter (Index, Source‑Address) sowie der Aktivierungs‑Qualifier QI gesetzt.       |

### **Ereignis‑Ausgänge**

| Ereignis | Typ   | Beschreibung                                                                                      |
| -------- | ----- | ------------------------------------------------------------------------------------------------- |
| INITO    | EInit | Bestätigung der erfolgreichen Initialisierung (QO = TRUE) oder Fehlermeldung.                     |
| IND      | Event | Indikation von der Ressource: feuert bei jedem Zyklus ohne aktiven Fehler und publiziert dabei auch das Zurücksetzen von `bCommError`/`bCRCError` auf FALSE. Liefert QO, STATUS, alle Mess- und Statuswerte (`rAcc*`, `rRate*`, `rRoll`, `rPitch`, `rYaw`, `rTempRateZ`, `uiHW_Index`, `eStatus*`, `bAllSignalsReady`, `uiSysStatus`, `uiSysStatus5`, `uiSysDiag`, `uiMessageCounter`). |
| ERROR    | Event | Feuert ausschließlich beim tatsächlichen Eintreten eines Fehlers (Entry-Trigger); liefert QO, STATUS, `bCommError`, `bCRCError` und `sErrorMsg`. Das Zurücksetzen von `bCommError`/`bCRCError` wird nicht hier, sondern über IND gemeldet. |

### **Daten‑Eingänge**

| Variable | Typ    | Beschreibung                                                                          |
| -------- | ------ | ------------------------------------------------------------------------------------- |
| QI       | BOOL   | Aktivierungs‑Qualifier: Nur bei QI = TRUE wird die Initialisierung (INIT) ausgeführt. |
| PARAMS   | STRING | Service‑Parameter, z. B. CAN‑Interface‑Konfiguration (optional).                      |
| u8CanIdx | USINT  | CAN‑Node‑Index (Standard‑Initialwert: `INVALID`).                                     |
| SA       | BYTE   | Source‑Address für J1939‑Kommunikation (Initialwert: `16#DA`).                        |

### **Daten‑Ausgänge**

| Variable                    | Typ    | Beschreibung                                                     |
| --------------------------- | ------ | ---------------------------------------------------------------- |
| QO                          | BOOL   | Quittierung der Initialisierung (TRUE = erfolgreich).            |
| STATUS                      | STRING | Statusmeldung (z. B. „Initialized“, „Error“).                    |
| rAccX, rAccY, rAccZ         | REAL   | Beschleunigungswerte in X‑, Y‑ und Z‑Richtung [m/s²].            |
| rRateX, rRateY, rRateZ      | REAL   | Drehraten um die jeweilige Achse [deg/s].                        |
| rRoll, rPitch, rYaw         | REAL   | Neigungswinkel (Roll, Pitch, Yaw) [deg].                         |
| rTempRateZ                  | REAL   | Sensortemperatur [°C].                                           |
| uiHW_Index                  | UINT   | Hardware‑Index (0 = MM5.10, 1 = MM7.10).                         |
| eStatusAccX … eStatusAccZ   | BYTE   | Signalqualität der Beschleunigung (einzeln `eStatusAccX`, `eStatusAccY`, `eStatusAccZ`; 0 = bereit, 1 .. 7 = Fehler). |
| eStatusRateX … eStatusRateZ | BYTE   | Signalqualität der Drehraten (einzeln `eStatusRateX`, `eStatusRateY`, `eStatusRateZ`; 0 = bereit, 1 .. 7 = Fehler).      |
| bAllSignalsReady            | BOOL   | TRUE, wenn alle Signal‑Status 0 sind.                            |
| uiSysStatus                 | BYTE   | Systemstatus aus TX‑Nachricht 1.                                 |
| uiSysStatus5                | BYTE   | Systemstatus aus TX‑Nachricht 2.                                 |
| uiSysDiag                   | BYTE   | Systemdiagnosecode (aus TX2).                                    |
| uiMessageCounter            | UINT   | Nachrichtenzähler (0..15) zur Timeout‑Überwachung.               |
| bCommError                  | BOOL   | TRUE bei CAN‑Timeout.                                            |
| bCRCError                   | BOOL   | TRUE bei fehlerhafter CRC‑Prüfung.                               |
| sErrorMsg                   | STRING | Fehlertext (z. B. „CAN timeout“).                                |

### **Adapter**

Keine Adapter definiert.

## Funktionsweise

Der FB_MM710_IMU initialisiert beim Eintreffen von **INIT** mit QI = TRUE die CAN‑Kommunikation und den internen Empfangspuffer. Nach erfolgreicher Initialisierung wird **INITO** mit QO = TRUE gesetzt. Der Baustein hat keinen Anforderungseingang: Sind gültige Daten vom Sensor empfangen, wird in jedem Zyklus ohne aktiven Fehler **IND** ausgegeben und alle Datenausgänge aktualisiert. Tritt ein Kommunikations‑ oder CRC‑Fehler auf oder überschreitet der Nachrichtenzähler einen Timeout, wird **ERROR** gesetzt.

Die Signal‑Status (eStatus*) ermöglichen eine Einzelfehleranalyse für jede Achse. Der Hardware‑Index unterscheidet zwischen älteren MM5.10 und aktuellen MM7.10 Sensoren.

## Technische Besonderheiten

- **CAN/J1939‑Protokoll** – Verwendung einer festen Source‑Address (Standard: `16#DA`).
- **Time‑out‑Überwachung** – Der `uiMessageCounter` (0‑15) wird bei jeder gültigen Nachricht hochgezählt; bleibt er aus, wird nach 16 fehlenden Nachrichten ein Kommunikationsfehler gemeldet.
- **Signal‑Status‑Bits** – Liefern granularere Informationen als einfache „ready/error“-Flags.
- **CRC‑Prüfung** – Fehlerhafte CAN‑Frames werden erkannt und über `bCRCError` und **ERROR** gemeldet.
- **Hardware‑Unterscheidung** – `uiHW_Index` erlaubt adaptives Verhalten für unterschiedliche Sensor‑Versionen.

## Zustandsübersicht

Der Baustein durchläuft folgende Zustände (nicht explizit als ECC, aber aus dem Verhalten ableitbar):

1. **Inaktiv** – Nach dem Start, wartet auf INIT.
2. **Initialisieren** – Nach INIT‑Ereignis; Aufbau der CAN‑Kommunikation.
3. **Bereit** – Nach erfolgreichem INITO; auf Sensor‑Nachrichten wartend.
4. **Daten empfangen** – Nach erfolgreicher CAN‑Nachricht; IND wird gesendet.
5. **Fehler** – Bei Timeout oder CRC‑Fehler; ERROR wird gesendet (Rückfall in Bereit nach Fehlerbehandlung).

## Anwendungsszenarien

- **Mobile Arbeitsmaschinen** – Neigungs‑ und Beschleunigungsüberwachung von Baggern, Kränen oder Gabelstaplern.
- **Fahrzeugdynamik** – Erfassung von Roll‑, Pitch‑ und Yaw‑Winkeln für Stabilitätskontrollen.
- **Industrieroboter** – Überwachung von Vibrationen und unerwarteten Bewegungen.
- **IoT‑Sensorknoten** – Einbindung in übergeordnete Steuerungen mittels CAN‑Bus.

## Vergleich mit ähnlichen Bausteinen

Gegenüber einfachen IMU‑Treibern (z. B. per SPI/I²C) bietet dieser FB eine direkte Integration in J1939‑Netzwerke. Die Signal‑Status‑Bits ermöglichen eine Diagnose, die bei Standard‑Bausteinen oft fehlt. Der integrierte Hardware‑Index (MM5.10 / MM7.10) erlaubt eine einfache Migration. Andere CAN‑IMU‑Bausteine verwenden ggf. proprietäre Nachrichtenformate, während dieser FB auf dem offenen J1939‑Standard basiert.

## Fazit

Der FB_MM710_IMU ist ein mächtiger Baustein für die zuverlässige Erfassung von IMU‑Daten in CAN‑basierten Automatisierungssystemen. Seine umfangreichen Status‑ und Fehlerinformationen unterstützen eine lückenlose Diagnose, und die einfache Parametrisierung über INIT macht ihn flexibel einsetzbar. Besonders in sicherheitskritischen Anwendungen mit J1939 ist er eine optimale Wahl.
