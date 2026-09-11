# IG4_Source_Addresses

![IG4_Source_Addresses](./IG4_Source_Addresses.svg)

* * * * * * * * * *

## Einleitung

Der Baustein **IG4_Source_Addresses** ist ein globaler Konstanten-Container für ISOBUS-spezifische Quelladressen (Source Addresses) der Industriegruppe 4 (Industry Group 4). Er stellt eine Sammlung von Konstanten mit festen BYTE-Werten bereit, die in der Automatisierungstechnik, insbesondere im Bereich der mobilen Arbeitsmaschinen und Marine-Antriebssysteme, zur eindeutigen Identifikation von ECUs (Electronic Control Units) verwendet werden. Die Werte sind nach dem Standard SAE J1939 bzw. ISO 11783 (ISOBUS) definiert und ermöglichen eine standardisierte Adressierung in CAN-basierten Netzwerken.

## Schnittstellenstruktur

Der Baustein besitzt keine Ereignis- oder Datenschnittstellen im herkömmlichen Sinne, da er ausschließlich globale Konstanten bereitstellt. Die Konstanten sind als Variablen mit festen Werten (InitialValue) definiert und können in anderen Bausteinen direkt referenziert werden.

### **Ereignis-Eingänge**

Keine.

### **Ereignis-Ausgänge**

Keine.

### **Daten-Eingänge**

Keine.

### **Daten-Ausgänge**

Keine – die Werte werden über die Konstantenbereitstellung der globalen Variablen zugänglich gemacht.

### **Adapter**

Keine.

## Funktionsweise

Der Baustein definiert eine Reihe von vordefinierten Konstanten, die jeweils eine spezifische Quelladresse (0–255) für verschiedene ECUs repräsentieren. Diese Adressen sind Teil des ISOBUS-Adressierungskonzepts und dienen der eindeutigen Identifikation von Steuergeräten in CAN-Netzwerken. Die Werte sind in der XML-Datei als `VarDeclaration` mit `Type="BYTE"` und einem `InitialValue` hinterlegt.

Die folgende Tabelle fasst alle definierten Konstanten zusammen:

| Name | Wert (BYTE) | Beschreibung |
|------|-------------|--------------|
| `SA_RESERVED_128` | 128 | Reserviert für dynamische Adresszuweisung (selbstkonfigurierend) |
| `SA_RESERVED_208` | 208 | Reserviert für individuelle fest vergebene Adressen |
| `SA_PROPULSION_SENSOR_HUB_GATEWAY_1` | 228 | Propulsions-Sensor-Hub & Gateway #1 |
| `SA_PROPULSION_SENSOR_HUB_GATEWAY_2` | 229 | Propulsions-Sensor-Hub & Gateway #2 |
| `SA_PROPULSION_SENSOR_HUB_GATEWAY_3` | 230 | Propulsions-Sensor-Hub & Gateway #3 |
| `SA_PROPULSION_SENSOR_HUB_GATEWAY_4` | 231 | Propulsions-Sensor-Hub & Gateway #4 |
| `SA_TRANSMISSION_3` | 232 | Getriebe-ECU für das dritte Getriebe |
| `SA_TRANSMISSION_4` | 233 | Getriebe-ECU für das vierte Getriebe |
| `SA_TRANSMISSION_5` | 234 | Getriebe-ECU für das fünfte Getriebe |
| `SA_TRANSMISSION_6` | 235 | Getriebe-ECU für das sechste Getriebe |
| `SA_DISPLAY_1_FOR_PROTECTION_SYSTEM_FOR_MARINE_ENGINES` | 236 | Display #1 für Schutzsystem von Schiffsdieselmotoren |
| `SA_PROTECTION_SYSTEM_FOR_MARINE_ENGINES` | 237 | Schutzsystem-Steuergerät für Schiffsdieselmotoren |
| `SA_ALARM_SYSTEM_CONTROL_1_FOR_MARINE_ENGINES` | 238 | Alarmsystem-Steuerung #1 für Schiffsdieselmotoren |
| `SA_ENGINE_3` | 239 | Motor-ECU für den dritten Motor |
| `SA_ENGINE_4` | 240 | Motor-ECU für den vierten Motor |
| `SA_ENGINE_5` | 241 | Motor-ECU für den fünften Motor |
| `SA_MARINE_DISPLAY_1` | 242 | Marine-Display #1 |
| `SA_MARINE_DISPLAY_2` | 243 | Marine-Display #2 |
| `SA_MARINE_DISPLAY_3` | 244 | Marine-Display #3 |
| `SA_MARINE_DISPLAY_4` | 245 | Marine-Display #4 |
| `SA_MARINE_DISPLAY_5` | 246 | Marine-Display #5 |
| `SA_MARINE_DISPLAY_6` | 247 | Marine-Display #6 |

Diese Konstanten können in anderen Funktionsbausteinen als Quelladressen für die Kommunikation verwendet werden, um eine konsistente und herstellerunabhängige Adressierung sicherzustellen.

## Technische Besonderheiten

- Der Baustein ist als **GlobalConstants**-Element definiert und wird in der 4diac-IDE als globaler Konstanten-Container behandelt.
- Alle Konstanten sind vom Typ `BYTE` (8-Bit-Wert, Bereich 0–255).
- Die Werte entsprechen den im ISOBUS-Standard (ISO 11783) festgelegten Adressen für die Industriegruppe 4 (Marine- und Propulsionsanwendungen).
- Die Konstanten können in anderen Bausteinen direkt über ihren Namen referenziert werden, ohne dass sie als Eingänge übergeben werden müssen.
- Die XML-Datei enthält zusätzlich eine `CompilerInfo`-Angabe (`isobus::pgn::const`), die auf die Paketstruktur für die Nativerzeugung hinweist.

## Zustandsübersicht

Da es sich um einen Konstanten-Container handelt, existieren keine internen Zustände oder Übergänge. Der Baustein ist statisch und wird zur Laufzeit nicht verändert.

## Anwendungsszenarien

- **ISOBUS-Netzwerkkonfiguration**: Verwendung der Quelladressen zur einheitlichen Konfiguration von Steuergeräten in landwirtschaftlichen oder mobilen Maschinen.
- **Marine-Antriebssysteme**: Adressierung von Motoren, Getrieben, Displays und Schutzsystemen in Schiffsanwendungen.
- **Entwicklung von ISOBUS-konformen Anwendungen**: Bereitstellung vordefinierter Adressen für die Kommunikation zwischen ECU und Bediengeräten.
- **Prototypenerstellung**: Schnelle Verwendung standardisierter Adressen ohne manuelle Definition.

## Vergleich mit ähnlichen Bausteinen

In der 4diac-Bibliothek existieren weitere globale Konstanten-Container für andere Industriegruppen (z.B. IG0 bis IG3). Diese unterscheiden sich in den spezifischen Adressbereichen und den zugeordneten Gerätetypen. Der vorliegende Baustein ist auf die Besonderheiten der Industriegruppe 4 (Marine und Propulsion) zugeschnitten und ergänzt die anderen Gruppen.

## Fazit

**IG4_Source_Addresses** ist ein einfacher, aber essentieller Baustein, der die standardisierten ISOBUS-Quelladressen für Marine- und Propulsionsanwendungen in einer zentralen, wiederverwendbaren Form bereitstellt. Durch die Verwendung globaler Konstanten wird die Konsistenz in der Adressierung erhöht und die Entwicklung von Kommunikationsanwendungen vereinfacht. Der Baustein eignet sich hervorragend für Projekte, die eine konforme Adressierung gemäß ISO 11783 erfordern.
