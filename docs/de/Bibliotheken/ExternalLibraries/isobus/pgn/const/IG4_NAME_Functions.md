# IG4_NAME_Functions

![IG4_NAME_Functions](./IG4_NAME_Functions.svg)

* * * * * * * * * *
## Einleitung

Der Baustein `IG4_NAME_Functions` stellt eine Sammlung globaler Konstanten für die **Industry Group 4** des ISOBUS-Standards (ISO 11783) bereit. Diese Konstanten definieren die **Funktionscodes** einzelner elektronischer Steuergeräte (ECUs) in einem ISOBUS-Netzwerk. Die Werte sind als `BYTE`-Konstanten ausgeführt und ermöglichen eine eindeutige Identifizierung der Funktion eines Geräts innerhalb der Industriegruppe 4 (Schiffs- und Bootsanwendungen). Der Baustein dient als zentrale Definitionsquelle für die Entwicklung von Anwendungen, die auf die ISOBUS-Kommunikation zugreifen.

## Schnittstellenstruktur

Der Baustein besitzt **keine** Ereignis- oder Dateneingänge und auch keine klassischen Datenausgänge. Stattdessen stellt er eine Reihe von globalen Konstanten bereit, die direkt über ihren symbolischen Namen referenziert werden können. Diese Konstanten werden im Folgenden als **globale Konstanten** aufgeführt und können als Werte für die Konfiguration oder Kommunikation in anderen Funktionsbausteinen verwendet werden.

### **Ereignis-Eingänge**
Keine.

### **Ereignis-Ausgänge**
Keine.

### **Daten-Eingänge**
Keine.

### **Daten-Ausgänge**
Die folgenden globalen Konstanten werden als Datenwerte bereitgestellt (Typ: `BYTE`). Sie sind inhaltlich nach den ISOBUS-Systemen gruppiert.

| Konstantenname | Wert | Beschreibung |
|----------------|------|--------------|
| **Nicht spezifisches System (System 0)** | | |
| `F_ALARM_SYSTEM_CONTROL_FOR_MARINE_ENGINES` | 128 | Alarmsteuerung für Schiffsmaschinen – ECU, die die Alarmfunktionen steuert. |
| `F_PROTECTION_SYSTEM_FOR_MARINE_ENGINES` | 129 | Schutzsystem für Schiffsmaschinen – erste ECU, die Schutzfunktionen steuert. |
| `F_DISPLAY_FOR_PROTECTION_SYSTEM_FOR_MARINE_ENGINES` | 130 | Anzeige für das Schutzsystem – ECU, die Informationen/Indikatoren anzeigt. |
| `F_NOT_AVAILABLE` | 255 | Nicht verfügbar – Platzhalter bis zur expliziten Zuweisung. |
| **Systemwerkzeuge (System 10)** | | |
| `F_SYSTEM_TOOLS_NOT_AVAILABLE` | 255 | Nicht verfügbar. |
| **Sicherheitssysteme (System 20)** | | |
| `F_SAFETY_SYSTEMS_NOT_AVAILABLE` | 255 | Nicht verfügbar. |
| **Gateway (System 25)** | | |
| `F_GATEWAY` | 130 | Gateway – Funktion nicht näher spezifiziert. |
| **Stromversorgungs- und Beleuchtungssysteme (System 30)** | | |
| `F_POWER_MGMT_LIGHTING_SWITCH` | 130 | Schalter für Stromversorgungs-/Beleuchtungssysteme. |
| `F_POWER_MGMT_LIGHTING_LOAD` | 140 | Last für Stromversorgungs-/Beleuchtungssysteme. |
| **Lenksysteme (System 40)** | | |
| `F_STEERING_SYSTEMS_FOLLOW_UP_CONTROLLER` | 130 | Folgeregler (Follow-up Controller). |
| `F_STEERING_SYSTEMS_MODE_CONTROLLER` | 140 | Modusregler. |
| `F_STEERING_SYSTEMS_AUTOMATIC_STEERING_CONTROLLER` | 150 | Automatiksteuerung. |
| `F_STEERING_SYSTEMS_HEADING_SENSORS` | 160 | Kurssensorik (Heading Sensors). |
| **Antriebssysteme (System 50)** | | |
| `F_PROPULSION_SYSTEMS_ENGINEROOM_MONITORING` | 130 | Maschinenraumüberwachung. |
| `F_PROPULSION_SYSTEMS_ENGINE_INTERFACE` | 140 | Motor-Schnittstelle. |
| `F_PROPULSION_SYSTEMS_ENGINE_CONTROLLER` | 150 | Motorsteuerung. |
| `F_PROPULSION_SYSTEMS_ENGINE_GATEWAY` | 160 | Motor-Gateway. |
| `F_PROPULSION_SYSTEMS_CONTROL_HEAD` | 170 | Steuerkopf (Control Head). |
| `F_PROPULSION_SYSTEMS_ACTUATOR` | 180 | Aktor. |
| `F_PROPULSION_SYSTEMS_GAUGE_INTERFACE` | 190 | Anzeige-Schnittstelle (Gauge Interface). |
| `F_PROPULSION_SYSTEMS_GAUGE_LARGE` | 200 | Große Anzeige. |
| `F_PROPULSION_SYSTEMS_GAUGE_SMALL` | 210 | Kleine Anzeige. |
| `F_PROPULSION_SYSTEMS_PROPULSION_SENSORS_GATEWAY` | 220 | Antriebssensoren & Gateway. |
| **Navigationssysteme (System 60)** | | |
| `F_NAVIGATION_SYSTEMS_SOUNDER_DEPTH` | 130 | Echolot (Tiefensensor). |
| `F_NAVIGATION_SYSTEMS` | 140 | Allgemeine Navigationsfunktion. |
| `F_NAVIGATION_SYSTEMS_GLOBAL_NAVIGATION_SATELLITE_SYSTEM_GNSS` | 145 | Globales Navigationssatellitensystem (GNSS). |
| `F_NAVIGATION_SYSTEMS_LORAN_C` | 150 | LORAN-C-Navigation. |
| `F_NAVIGATION_SYSTEMS_SPEED_SENSORS` | 155 | Geschwindigkeitssensoren. |
| `F_NAVIGATION_SYSTEMS_TURN_RATE_INDICATOR` | 160 | Drehratenanzeige. |
| `F_NAVIGATION_SYSTEMS_INTEGRATED_NAVIGATION` | 170 | Integrierte Navigation. |
| `F_NAVIGATION_SYSTEMS_RADAR_AND_OR_RADAR_PLOTTING` | 200 | Radar und/oder Radar-Plotting. |
| `F_NAVIGATION_SYSTEMS_ELECTRONIC_CHART_DISPLAY_INFORMATION_SYSTEM_ECDIS` | 205 | Elektronisches Seekarten-Anzeige- und Informationssystem (ECDIS). |
| `F_NAVIGATION_SYSTEMS_ELECTRONIC_CHART_SYSTEM_ECS` | 210 | Elektronisches Kartensystem (ECS). |
| `F_NAVIGATION_SYSTEMS_DIRECTION_FINDER` | 220 | Funkpeiler (Direction Finder). |
| **Kommunikationssysteme (System 70)** | | |
| `F_COMMUNICATIONS_SYSTEMS_EMERGENCY_POSITION_INDICATING_BEACON_EPIRB` | 130 | Notfallbake zur Positionsanzeige (EPIRB). |
| `F_COMMUNICATIONS_SYSTEMS_AUTOMATIC_IDENTIFICATION_SYSTEM` | 140 | Automatisches Identifikationssystem (AIS). |
| `F_COMMUNICATIONS_SYSTEMS_DIGITAL_SELECTIVE_CALLING_DSC` | 150 | Digitaler Selektivruf (DSC). |
| `F_COMMUNICATIONS_SYSTEMS_DATA_RECEIVER` | 160 | Datenempfänger. |
| `F_COMMUNICATIONS_SYSTEMS_SATELLITE` | 170 | Satellitenkommunikation. |
| `F_COMMUNICATIONS_SYSTEMS_RADIO_TELEPHONE_MF_HF` | 180 | Funktelefon (Mittel-/Kurzwelle). |
| `F_COMMUNICATIONS_SYSTEMS_RADIO_TELEPHONE_VHF` | 190 | Funktelefon (UKW). |
| **Instrumentierung/allgemeine Systeme (System 80)** | | |
| `F_INSTRUMENTATION_GENERAL_TIME_DATE_SYSTEMS` | 130 | Zeit-/Datumsysteme. |
| `F_INSTRUMENTATION_GENERAL_VOYAGE_DATA_RECORDER` | 140 | Reisedatenschreiber (Voyage Data Recorder). |
| `F_INSTRUMENTATION_GENERAL_INTEGRATED_INSTRUMENTATION` | 150 | Integrierte Instrumentierung. |
| `F_INSTRUMENTATION_GENERAL_GENERAL_PURPOSE_DISPLAYS` | 160 | Allzweckanzeigen. |
| `F_INSTRUMENTATION_GENERAL_GENERAL_SENSOR_BOX` | 170 | Allgemeine Sensorbox. |
| `F_INSTRUMENTATION_GENERAL_WEATHER_INSTRUMENTS` | 180 | Wetterinstrumente. |
| `F_INSTRUMENTATION_GENERAL_TRANSDUCER_GENERAL` | 190 | Allgemeiner Messumformer. |
| `F_INSTRUMENTATION_GENERAL_NMEA_0183_CONVERTER` | 200 | NMEA-0183-Konverter. |
| **Umweltsysteme (HVAC) (System 90)** | | |
| `F_ENVIRONMENTAL_HVAC_NOT_AVAILABLE` | 255 | Nicht verfügbar. |
| **Deck-, Ladungs- und Fischereiausrüstung (System 100)** | | |
| `F_DECK_CARGO_FISHING_NOT_AVAILABLE` | 255 | Nicht verfügbar. |

## Funktionsweise

Die Konstanten in `IG4_NAME_Functions` dienen als symbolische Namen für ISOBUS-Funktionscodes. Jede Konstante hat einen festen `BYTE`-Wert, der in der ISOBUS-Kommunikation (z. B. in der Parameter Group Numbers (PGN) für die Adressierung und Funktionsidentifikation) verwendet wird. Durch die Verwendung dieser Konstanten wird der Code lesbarer und wartbarer, da anstelle von magischen Zahlen aussagekräftige Namen verwendet werden. Die Werte sind nach dem ISOBUS-Standard für die Industriegruppe 4 festgelegt und können unverändert in eigenen Anwendungen referenziert werden.

Die Konstanten sind in logische Gruppen entsprechend der ISOBUS-Systeme unterteilt – von nicht spezifischen Systemen über Antriebs-, Navigations- und Kommunikationssysteme bis hin zu Instrumentierung und Umgebungssystemen. Jede Konstante entspricht einer bestimmten Funktion, die ein Gerät (ECU) in einem Netzwerk ausführt.

## Technische Besonderheiten

- **Typ:** Alle Konstanten sind vom Typ `BYTE` (8-Bit-Wert) und können Werte von 0–255 annehmen.
- **Wertebereich:** Die verwendeten Werte liegen zwischen 128 und 220 sowie 255 für "Nicht verfügbar". Diese Werte folgen den ISOBUS-Spezifikationen für die Industriegruppe 4.
- **Namensschema:** Die Konstanten beginnen mit `F_` gefolgt vom Systemnamen (z. B. `NAVIGATION_SYSTEMS`) und einer genaueren Bezeichnung. Das Schema erleichtert die Zuordnung.
- **Globaler Zugriff:** Die Konstanten sind global definiert und können in jedem FB oder jeder Applikation innerhalb des 4diac-Projekts verwendet werden, ohne dass eine Instanzierung erforderlich ist.
- **Keine Laufzeitänderung:** Die Werte sind konstant und können zur Laufzeit nicht verändert werden.

## Zustandsübersicht

Nicht zutreffend. Da es sich um einen reinen Konstanten-Baustein handelt, gibt es keinen internen Zustand oder eine Zustandsmaschine.

## Anwendungsszenarien

- **ISOBUS-Kommunikation:** Verwendung der Konstanten zur Identifizierung der Funktion einer ECU in einem ISOBUS-Netzwerk (z. B. in PGN-Nachrichten für die Adressierung).
- **Konfiguration von Geräten:** In der Entwicklung von Anwendungen für Schiffs- oder Bootsautomation können diese Konstanten bei der Konfiguration von Steuergeräten verwendet werden.
- **Erweiterung bestehender Systeme:** Bei der Integration neuer Geräte in ein bestehendes Netzwerk können die passenden Funktionscodes über die Konstanten zugewiesen werden.
- **Lesbare Codebasis:** Ersatz von numerischen Magic Numbers durch aussagekräftige symbolische Namen verbessert die Wartbarkeit und reduziert Fehler.

## Vergleich mit ähnlichen Bausteinen

Ähnliche Global-Konstanten-Bausteine existieren für andere Industriegruppen (z. B. Industry Group 1, 2, 3) im ISOBUS-Standard. Diese definieren jeweils eigene Funktionscodes, die auf die spezifischen Anforderungen der jeweiligen Gruppe (z. B. Landwirtschaft, Bau, Forst) zugeschnitten sind. `IG4_NAME_Functions` deckt ausschließlich die Anforderungen der Industriegruppe 4 (Marine) ab. Im Vergleich zu einem Funktionsbaustein, der Logik oder Verhalten implementiert, stellt dieser Baustein lediglich statische Daten bereit und besitzt keine Schnittstellen für Ereignisse oder Flusssteuerung. Er ist ein reiner Datencontainer, der als Unterstützung für die Entwicklung von FB-Kompositionen dient.

## Fazit

`IG4_NAME_Functions` ist ein essenzieller Bestandteil für die Entwicklung ISOBUS-konformer Anwendungen im Marinebereich. Er bündelt alle relevanten Funktionscodes für die Industriegruppe 4 in einer zentralen, leicht zugänglichen und gut dokumentierten Form. Durch die Verwendung dieser Konstanten wird die Implementierung von ISOBUS-Funktionalitäten in der 4diac-IDE deutlich vereinfacht und standardisiert. Die klare Struktur und die umfangreiche Kommentierung machen den Baustein zu einer wertvollen Ressource für Entwickler, die marine Steuerungssysteme realisieren.