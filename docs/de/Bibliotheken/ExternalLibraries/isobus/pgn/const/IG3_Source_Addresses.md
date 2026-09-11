# IG3_Source_Addresses

![IG3_Source_Addresses](./IG3_Source_Addresses.svg)

* * * * * * * * * *
## Einleitung

Das globale Konstantenobjekt **IG3_Source_Addresses** definiert feste Quelladressen (Source Addresses) für Geräte des ISOBUS‑Netzwerks (ISO 11783) speziell für die Industriegruppe 3. Diese Adressen werden zur eindeutigen Identifizierung von Steuergeräten, Sensoren und Anzeigeeinheiten innerhalb eines landwirtschaftlichen oder bautechnischen Fahrzeugs verwendet. Der Baustein stellt keinen aktiven Funktionsblock dar, sondern liefert einen Satz von Konstanten, die in anderen Applikationen als globale Variablen referenziert werden können.

## Schnittstellenstruktur

Da es sich um ein reines Konstantenobjekt handelt, besitzt **IG3_Source_Addresses keine** prozessbezogenen Schnittstellen. Es werden weder Ereignisse verarbeitet noch Datenein‑ oder -ausgänge verwendet. Die folgenden Abschnitte sind daher nicht zutreffend und dienen nur der vollständigen Strukturbeschreibung.

### **Ereignis-Eingänge**

Keine vorhanden.

### **Ereignis-Ausgänge**

Keine vorhanden.

### **Daten-Eingänge**

Keine vorhanden.

### **Daten-Ausgänge**

Keine vorhanden.

### **Adapter**

Keine vorhanden.

## Funktionsweise

Die Konstanten definieren die im ISOBUS‑Standard festgelegten Quelladressen für Geräte der Industriegruppe 3. Jede Konstante hat einen sprechenden Namen, der das zugeordnete Gerät beschreibt, und einen festen Wert vom Typ `BYTE`. Die Werte sind in folgende Bereiche aufgeteilt:

- **128–207**: Reserviert für dynamische Adresszuweisung (selbstkonfigurierbar)  
- **208–223**: Reserviert für individuell vorab zugewiesene Adressen  
- **224–247**: Spezifische Geräteadressen (z. B. Sensoren, Controller, Displays)

Die folgende Tabelle listet alle definierten Konstanten:

| Konstante | Wert | Gerätebeschreibung |
|-----------|------|---------------------|
| `SA_RESERVED_128` | 128 | Reserviert für dynamische Adressvergabe |
| `SA_RESERVED_208` | 208 | Reserviert für individuelle Adressvergabe |
| `SA_ROTATION_SENSOR` | 224 | Drehwinkelsensor |
| `SA_LIFT_ARM_CONTROLLER` | 225 | Hubarm‑Steuerung für Baumaschinen |
| `SA_SLOPE_SENSOR` | 226 | Neigungssensor |
| `SA_MAIN_CONTROLLER_SKID_STEER_LOADER` | 227 | Hauptcontroller für Skid‑Steer‑Lader |
| `SA_LOADER_CONTROL` | 228 | Steuerung der Laderhydraulik |
| `SA_LASER_TRACER` | 229 | Lasertracker für Positionserfassung |
| `SA_LAND_LEVELING_SYSTEM_DISPLAY` | 230 | Anzeige für Landplaniersystem |
| `SA_SINGLE_LAND_LEVELING_SYSTEM_SUPERVISOR` | 231 | Supervisor für Einzel‑Landplaniersystem |
| `SA_LAND_LEVELING_ELECTRIC_MAST` | 232 | Elektrischer Mast für Landplaniersystem |
| `SA_SINGLE_LAND_LEVELING_SYSTEM_OPERATOR_INTERFACE` | 233 | Bedienoberfläche des Landplaniersystems |
| `SA_LASER_RECEIVER` | 234 | Laserempfänger |
| `SA_SUPPLEMENTAL_SENSOR_PROCESSING_UNIT_1` | 235 | Zusätzliche Sensor‑Verarbeitungseinheit 1 |
| `SA_SUPPLEMENTAL_SENSOR_PROCESSING_UNIT_2` | 236 | Zusätzliche Sensor‑Verarbeitungseinheit 2 |
| `SA_SUPPLEMENTAL_SENSOR_PROCESSING_UNIT_3` | 237 | Zusätzliche Sensor‑Verarbeitungseinheit 3 |
| `SA_SUPPLEMENTAL_SENSOR_PROCESSING_UNIT_4` | 238 | Zusätzliche Sensor‑Verarbeitungseinheit 4 |
| `SA_SUPPLEMENTAL_SENSOR_PROCESSING_UNIT_5` | 239 | Zusätzliche Sensor‑Verarbeitungseinheit 5 |
| `SA_SUPPLEMENTAL_SENSOR_PROCESSING_UNIT_6` | 240 | Zusätzliche Sensor‑Verarbeitungseinheit 6 |
| `SA_ENGINE_MONITOR_1` | 241 | Motor‑Monitor 1 |
| `SA_ENGINE_MONITOR_2` | 242 | Motor‑Monitor 2 |
| `SA_ENGINE_MONITOR_3` | 243 | Motor‑Monitor 3 |
| `SA_ENGINE_MONITOR_4` | 244 | Motor‑Monitor 4 |
| `SA_ENGINE_MONITOR_5` | 245 | Motor‑Monitor 5 |
| `SA_ENGINE_MONITOR_6` | 246 | Motor‑Monitor 6 |
| `SA_ENGINE_MONITOR_7` | 247 | Motor‑Monitor 7 |

Diese Konstanten können in 4diac‑Applikationen direkt referenziert werden, um sicherzustellen, dass ISOBUS‑Nachrichten an die korrekte Quelladresse gesendet werden.

## Technische Besonderheiten

- Alle Konstanten sind als `BYTE` deklariert und besitzen feste Initialwerte.
- Die Werte sind gemäß ISO 11783 (ISOBUS) standardisiert und dürfen nicht verändert werden.
- Die Konstanten sind im Compiler‑Paket `isobus::pgn::const` gekapselt, was eine saubere Strukturierung des Projekts unterstützt.
- Die Datei enthält keine Logik oder Zustandsautomaten – sie ist rein deklarativ.

## Zustandsübersicht

Nicht zutreffend, da das Objekt keine aktiven Zustände besitzt.

## Anwendungsszenarien

- **Entwicklung von ISOBUS‑Steuergeräten**: Die Konstanten können verwendet werden, um die eigenen Quelladressen zu konfigurieren oder um Adressen anderer Geräte im Netzwerk zu referenzieren.
- **Kommunikation mit spezifischen Geräten**: Beispielsweise können Nachrichten an den `SA_ROTATION_SENSOR` (224) adressiert werden, um Drehwinkeldaten zu empfangen.
- **Integration in 4diac‑Projekte**: Die Konstanten werden als globale Variablen in Funktionsblöcken verwendet, um den Programmcode lesbarer und wartbarer zu gestalten.

## Vergleich mit ähnlichen Bausteinen

Es existieren analoge `GlobalConstants`‑Objekte für andere Industriegruppen, z. B. für die Gruppen 1 und 2. Diese definieren jeweils die passenden Quelladressen für ihre spezifischen Geräte. Der vorliegende Baustein konzentriert sich ausschließlich auf die Industriegruppe 3, die vorwiegend in der Bau‑ und Landtechnik relevante Geräte (wie Lader, Planiersysteme, Sensoren) umfasst.

## Fazit

Das `IG3_Source_Addresses`‑Objekt stellt eine zentrale, standardkonforme Sammlung von Quelladressen für ISOBUS‑Anwendungen bereit. Durch die Verwendung dieser Konstanten wird die Einhaltung des ISO‑Standards erleichtert und die Softwareentwicklung für Steuergeräte in der Industriegruppe 3 vereinfacht. Auch wenn es sich nicht um einen klassischen Funktionsblock handelt, erfüllt es eine wichtige Rolle in der Projektstrukturierung und Adressverwaltung.