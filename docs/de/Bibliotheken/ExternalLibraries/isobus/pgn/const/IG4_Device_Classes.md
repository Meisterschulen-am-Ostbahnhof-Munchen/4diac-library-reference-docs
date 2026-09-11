# IG4_Device_Classes

![IG4_Device_Classes](./IG4_Device_Classes.svg)

* * * * * * * * * *

## Einleitung

Der Baustein `IG4_Device_Classes` ist ein GlobalConstants-Baustein in 4diac. Er definiert Konstanten für Geräteklassen (Device Classes) von Fahrzeugsystemen gemäß ISOBUS / Industry Group 4. Diese Konstanten sind im Compiler-Paket `isobus::pgn::const` verfügbar und können in IEC 61499-Anwendungen als globale, unveränderliche Werte referenziert werden.

## Schnittstellenstruktur

Der Baustein besitzt keine Ereignis- oder Datenschnittstellen, da es sich um eine globale Konstantensammlung handelt.

### **Ereignis-Eingänge**

Nicht vorhanden.

### **Ereignis-Ausgänge**

Nicht vorhanden.

### **Daten-Eingänge**

Nicht vorhanden.

### **Daten-Ausgänge**

Nicht vorhanden. Als globaler Konstantenbaustein stellt er jedoch folgende Werte im Projektkontext bereit:

| Konstante | Typ | Wert | Beschreibung |
|---|---|---|---|
| `DC_NON_SPECIFIC_SYSTEM` | BYTE | 0 | Nicht spezifisches System |
| `DC_SYSTEM_TOOLS` | BYTE | 10 | Systemwerkzeuge |
| `DC_SAFETY_SYSTEMS` | BYTE | 20 | Sicherheitssysteme |
| `DC_GATEWAY` | BYTE | 25 | Gateway |
| `DC_POWER_MGMT_LIGHTING` | BYTE | 30 | Energiemanagement und Beleuchtungssysteme |
| `DC_STEERING_SYSTEMS` | BYTE | 40 | Lenksysteme |
| `DC_PROPULSION_SYSTEMS` | BYTE | 50 | Antriebssysteme |
| `DC_NAVIGATION_SYSTEMS` | BYTE | 60 | Navigationssysteme |
| `DC_COMMUNICATIONS_SYSTEMS` | BYTE | 70 | Kommunikationssysteme |
| `DC_INSTRUMENTATION_GENERAL` | BYTE | 80 | Instrumentierung / allgemeine Systeme |
| `DC_ENVIRONMENTAL_HVAC` | BYTE | 90 | Umgebungssysteme (HLK) |
| `DC_DECK_CARGO_FISHING` | BYTE | 100 | Deck-, Ladungs- und Fangausrüstung |
| `DC_NOT_AVAILABLE` | BYTE | 127 | Nicht verfügbar |

### **Adapter**

Nicht vorhanden.

## Funktionsweise

Der Baustein dient als zentrale Definitionsquelle für Geräteklassen innerhalb der ISOBUS-Gruppe 4 (Fahrzeugsysteme). Er stellt symbolische Namen für numerische Werte bereit, die an anderer Stelle in der Anwendung als Konstanten referenziert werden können. Dadurch werden magische Zahlen vermieden und die Lesbarkeit sowie Wartbarkeit des IEC-61499-Codes verbessert.

## Technische Besonderheiten

- Alle Konstanten sind vom Typ `BYTE` und besitzen feste, unveränderliche Werte.
- Die Konstanten sind im Paket `isobus::pgn::const` gekapselt.
- Der Baustein wird als `GlobalConstants`-Element deklariert und besitzt keine Laufzeitsemantik; er wird nur zur Compilezeit verwendet.
- Die Werte folgen den ISOBUS-Konventionen der Industriegruppe 4.

## Zustandsübersicht

Nicht anwendbar. Der Baustein besitzt keine Zustände oder Zustandsübergänge, da er keine ausführbare Logik enthält.

## Anwendungsszenarien

- **ISOBUS-Anwendungen**: Zur Kennzeichnung von Geräten und Systemen in Fahrzeugnetzwerken.
- **Systemdiagnose**: Zuordnung von Fehlern oder Ereignissen zu bestimmten Geräteklassen.
- **Gateway-Konfiguration**: Auswahl der passenden Geräteklasse bei der Kommunikation zwischen verschiedenen Fahrzeugteilen.
- **Normkonforme Entwicklung**: Verwendung der offiziellen ISOBUS-Geräteklassen-Werte ohne manuelle Definition.

## Vergleich mit ähnlichen Bausteinen

Im Gegensatz zu Funktionsbausteinen oder Adaptern besitzt `IG4_Device_Classes` keine Ein-/Ausgänge, keine Algorithmen und kein internes Zustandsverhalten. Es ist eine reine Datenquelle für Konstanten. Ähnliche GlobalConstants-Bausteine existieren für andere Industriegruppen (z. B. `IG3_Device_Classes` für landwirtschaftliche Systeme außerhalb von Fahrzeugen). Der wesentliche Unterschied liegt in den abgedeckten Wertebereichen und der semantischen Zuordnung der Geräteklassen.

## Fazit

`IG4_Device_Classes` ist ein einfacher, aber wichtiger GlobalConstants-Baustein zur Vereinheitlichung der Geräteklassen-Definitionen in ISOBUS-Anwendungen. Durch die symbolische Darstellung der Werte wird die Codequalität verbessert und die Konformität mit der Norm sichergestellt.
