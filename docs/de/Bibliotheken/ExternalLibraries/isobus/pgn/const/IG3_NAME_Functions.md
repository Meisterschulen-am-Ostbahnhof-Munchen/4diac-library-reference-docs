# IG3_NAME_Functions

![IG3_NAME_Functions](./IG3_NAME_Functions.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock **IG3_NAME_Functions** ist ein GlobalConstants-Baustein, der eine Sammlung von globalen Konstanten für die ISO‑11783‑Kommunikation (ISOBUS) bereitstellt. Er definiert Funktionscodes (Function Values) für Geräte in der **Industriegruppe 3** – Baumaschinen und Anbaugeräte. Die Konstanten werden typischerweise verwendet, um Steuergeräte (ECUs) oder Sensoren eindeutig einer bestimmten Funktion innerhalb eines Maschinensystems zuzuordnen.

Die Werte sind als BYTE‑Konstanten definiert und decken verschiedene Maschinentypen ab, von generischen Systemen (Non‑specific System) über Bagger, Lader, Grader bis hin zu Spezialmaschinen wie Recyclern oder Betonpumpen.

## Schnittstellenstruktur

Da es sich um einen GlobalConstants‑Baustein handelt, besitzt er **keine** Ein‑ oder Ausgänge im Sinne eines Funktionsbausteins. Die bereitgestellten Werte sind globale Konstanten, die direkt im System verwendet werden können.

### **Ereignis-Eingänge**

Keine

### **Ereignis-Ausgänge**

Keine

### **Daten-Eingänge**

Keine

### **Daten-Ausgänge**

Keine

### **Adapter**

Keine

### **Globale Konstanten**

Die folgende Tabelle listet alle im Baustein definierten Konstanten auf. Der Typ ist jeweils `BYTE`, der Wert entspricht dem Initialwert.

| Name | Wert (dezimal) | System | Funktion / Beschreibung |
|------|----------------|--------|-------------------------|
| `F_SUPPLEMENTAL_ENGINE_CONTROL_SENSING` | 128 | Non‑specific System | Supplemental Engine Control Sensing |
| `F_LASER_RECEIVER` | 129 | Non‑specific System | Laser Receiver |
| `F_LAND_LEVELING_SYSTEM_OPERATOR_INTERFACE` | 130 | Non‑specific System | Land Leveling System Operator Interface – Bedienoberfläche für das Landplaniersystem |
| `F_LAND_LEVELING_ELECTRIC_MAST` | 131 | Non‑specific System | Land Leveling Electric Mast |
| `F_SINGLE_LAND_LEVELING_SYSTEM_SUPERVISOR` | 132 | Non‑specific System | Einzelüberwachung des Landplaniersystems |
| `F_LAND_LEVELING_SYSTEM_DISPLAY` | 133 | Non‑specific System | Anzeige des Landplaniersystems |
| `F_LASER_TRACER` | 134 | Non‑specific System | Laser Tracer |
| `F_LOADER_CONTROL` | 135 | Non‑specific System | Steuerung für Lader |
| `F_SLOPE_SENSOR` | 136 | Non‑specific System | Neigungssensor – misst die Neigung entlang einer Achse |
| `F_LIFTARM_CONTROL` | 137 | Non‑specific System | Hubarmsteuerung – steuert Heben und Kippen bei Ladegeräten |
| `F_SUPPLEMENTAL_SENSOR_PROCESSING_UNITS` | 138 | Non‑specific System | Zusätzliche Sensorverarbeitungseinheiten – I/O‑Modul für Datenerfassung |
| `F_HYDRAULIC_SYSTEM_PLANNER` | 139 | Non‑specific System | Hydrauliksystem‑Planer – koordiniert mehrere Ventilsteuerungen |
| `F_HYDRAULIC_VALVE_CONTROLLER` | 140 | Non‑specific System | Hydraulikventilsteuerung – steuert Ölfluss zu einem Zylinder |
| `F_JOYSTICK_CONTROL` | 141 | Non‑specific System | Joystick‑Steuerung |
| `F_ROTATION_SENSOR` | 142 | Non‑specific System | Rotationssensor – misst den Drehwinkel um eine Achse |
| `F_SONIC_SENSOR` | 143 | Non‑specific System | Ultraschallsensor – misst Entfernung durch Ultraschall‑Echo |
| `F_SURVEY_TOTAL_STATION_TARGET` | 144 | Non‑specific System | Ziel für Totalstation – für Vermessungsaufgaben |
| `F_HEADING_SENSOR` | 145 | Non‑specific System | Kurssensor – misst den Azimut des Fahrzeugs |
| `F_ALARM_DEVICE` | 146 | Non‑specific System | Alarmgerät – gibt akustische und/oder visuelle Warnungen aus |
| `F_NOT_AVAILABLE` | 255 | Non‑specific System | Nicht verfügbar – Platzhalter für noch nicht zugewiesene Funktionen |
| `F_SKID_STEER_LOADER_MAIN_CONTROLLER` | 128 | Skid Steer Loader | Hauptcontroller für Kompaktlader |
| `F_SKID_STEER_LOADER_NOT_AVAILABLE` | 255 | Skid Steer Loader | Nicht verfügbar |
| `F_ARTICULATED_DUMP_TRUCK_NOT_AVAILABLE` | 255 | Articulated Dump Truck | Nicht verfügbar |
| `F_BACKHOE_NOT_AVAILABLE` | 255 | Backhoe | Nicht verfügbar |
| `F_CRAWLER_BLADE_CONTROLLER` | 128 | Crawler | Plattensteuerung – steuert die Plattenhöhe |
| `F_CRAWLER_NOT_AVAILABLE` | 255 | Crawler | Nicht verfügbar |
| `F_EXCAVATOR_SLOPE_SENSOR` | 128 | Excavator | Neigungssensor – misst die Neigung entlang einer Achse |
| `F_EXCAVATOR_NOT_AVAILABLE` | 255 | Excavator | Nicht verfügbar |
| `F_FORKLIFT_NOT_AVAILABLE` | 255 | Forklift | Nicht verfügbar |
| `F_FOUR_WHEEL_DRIVE_LOADER_NOT_AVAILABLE` | 255 | Four Wheel Drive Loader | Nicht verfügbar |
| `F_GRADER_HFWD_CONTROLLER` | 128 | Grader | HFWD‑Controller – hydraulische Vorderrad‑Antriebssteuerung |
| `F_GRADER_NOT_AVAILABLE` | 255 | Grader | Nicht verfügbar |
| `F_MILLING_MACHINE_NOT_AVAILABLE` | 255 | Milling Machine | Nicht verfügbar – Fräse zum Entfernen von Straßenbelag |
| `F_RECYCLER_AND_SOIL_STABILIZER_NOT_AVAILABLE` | 255 | Recycler and Soil Stabilizer | Nicht verfügbar – Recycler zur Aufbereitung von Asphalt |
| `F_BINDING_AGENT_SPREADER_NOT_AVAILABLE` | 255 | Binding Agent Spreader | Nicht verfügbar – Streuer für Bindemittel |
| `F_PAVER_NOT_AVAILABLE` | 255 | Paver | Nicht verfügbar – Straßenfertiger |
| `F_FEEDER_NOT_AVAILABLE` | 255 | Feeder | Nicht verfügbar – Beschicker für Fertiger |
| `F_SCREENING_PLANT_NOT_AVAILABLE` | 255 | Screening Plant | Nicht verfügbar – Siebanlage |
| `F_STACKER_NOT_AVAILABLE` | 255 | Stacker | Nicht verfügbar – Absetzer |
| `F_ROLLER_NOT_AVAILABLE` | 255 | Roller | Nicht verfügbar – Walze |
| `F_CRUSHER_NOT_AVAILABLE` | 255 | Crusher | Nicht verfügbar – Brecher |

## Funktionsweise

Der Baustein definiert Konstanten, die in der ISOBUS‑Parametrierung zur Identifizierung der Funktion eines angeschlossenen Geräts verwendet werden. Die Konstanten füllen das Feld „Function" im Parameter Group Number (PGN)‑Kontext. Durch die Verwendung dieser Konstanten kann eine Applikation eindeutig erkennen, welche Aufgabe ein Gerät innerhalb einer Maschine übernimmt (z. B. Neigungssensor, Joystick, Motorsteuerung).

Der Wert 255 (dezimal) ist als „Not Available" reserviert und wird verwendet, wenn einem Gerät noch keine spezifische Funktion zugewiesen wurde. Spezifische Werte im Bereich 128–146 sind für die Industriegruppe 3 definiert.

## Technische Besonderheiten

- **Typ:** Alle Konstanten sind vom Typ `BYTE`.
- **Wertebereich:** Die definierten Werte liegen zwischen 128 und 255. Die obere Grenze (255) ist als „Not Available" definiert.
- **Standardkonformität:** Die Konstanten basieren auf dem ISO‑11783‑Standard (ISOBUS) und sind speziell für die Industriegruppe 3 (Baumaschinen) vorgesehen.
- **Globaler Gültigkeitsbereich:** Da es sich um einen `GlobalConstants`‑Baustein handelt, sind die Konstanten im gesamten Projekt verfügbar, sofern der Baustein eingebunden wird.
- **Versionierung:** Der Baustein ist auf Version 1.0 datiert und enthält einen Verweis auf den ursprünglichen Quellcode (`OriginalSource`).

## Zustandsübersicht

Ein `GlobalConstants`‑Baustein besitzt keinen inneren Zustand. Er stellt lediglich konstante Werte bereit, die zur Compile‑Zeit feststehen. Daher existiert keine Zustandsmaschine oder Laufzeitlogik.

## Anwendungsszenarien

- **Maschinenvernetzung:** In einem ISOBUS‑Netzwerk können Steuergeräte anhand ihrer Funktionscodes identifiziert werden. Der Baustein liefert die Mapping‑Konstanten für die Softwareentwicklung.
- **Parametrierung von ECUs:** Bei der Inbetriebnahme von Baumaschinen können diese Konstanten verwendet werden, um Geräte eindeutig zu konfigurieren (z. B. Zuweisung eines Joysticks zur Steuerung).
- **Diagnose und Fehlerbehebung:** Die Konstanten erlauben eine klare Zuordnung von Fehler‑ und Statusmeldungen zu bestimmten Maschinenkomponenten.
- **Plattformunabhängige Entwicklung:** Durch die Verwendung standardisierter Funktionscodes wird die Entwicklung von Applikationen für verschiedene Maschinenhersteller vereinfacht.

## Vergleich mit ähnlichen Bausteinen

In der 4diac‑IDE existieren ähnliche `GlobalConstants`‑Bausteine für andere Industriegruppen (z. B. `IG1_NAME_Functions` für Landwirtschaft oder `IG2_NAME_Functions` für Forstmaschinen). Diese unterscheiden sich in den definierten Konstanten und den zugehörigen Maschinentypen. Der vorliegende Baustein deckt ausschließlich die Anforderungen der Baumaschinenbranche ab und enthält daher spezifische Funktionen wie Lader, Bagger, Grader, Walzen usw.

## Fazit

`IG3_NAME_Functions` ist ein essenzieller Baustein für die Entwicklung von ISOBUS‑Applikationen im Bereich Baumaschinen. Durch die Bereitstellung standardisierter Funktionscodes inklusive detaillierter Beschreibungen erleichtert er die Implementierung, Wartung und Diagnose von vernetzten Maschinen. Seine einfache Struktur ohne Laufzeitlogik macht ihn zu einem robusten und klar verständlichen Bestandteil jedes 4diac‑Projekts, das mit ISOBUS‑Geräten kommuniziert.
