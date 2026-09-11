# IG3_Device_Classes

![IG3_Device_Classes](./IG3_Device_Classes.svg)

* * * * * * * * * *
## Einleitung

Der Baustein `IG3_Device_Classes` ist eine globale Konstantendefinition (Global Constants) gemäß IEC 61499. Er stellt eine Sammlung von Konstanten bereit, die die spezifischen Geräteklassen (Vehicle Systems) der Industry Group 3 (IG3) im ISOBUS-Protokoll (ISO 11783) definieren. Diese Konstanten werden üblicherweise verwendet, um den Fahrzeugtyp oder die Geräteklasse in Kommunikationsprotokollen (z.B. über PGNs) zu identifizieren. Der Baustein ist als Parameterliste oder Konstantenquelle für Applikationen gedacht, die mit land- oder forstwirtschaftlichen Maschinen arbeiten.

## Schnittstellenstruktur

Der Baustein `IG3_Device_Classes` besitzt keine Ein-/Ausgangsschnittstellen im klassischen Sinne eines Funktionsblocks. Es handelt sich um eine Auflistung globaler Konstanten, die direkt im System verfügbar sind. Daher entfallen Ereignis- und Daten-Eingänge sowie -Ausgänge und Adapter.

### **Ereignis-Eingänge**

Keine.

### **Ereignis-Ausgänge**

Keine.

### **Daten-Eingänge**

Keine.

### **Daten-Ausgänge**

Keine.

### **Adapter**

Keine.

## Funktionsweise

Der Baustein definiert eine Reihe von benannten Konstanten vom Typ `BYTE`. Jede Konstante repräsentiert einen spezifischen Fahrzeug- oder Gerätetyp gemäß der ISOBUS-Spezifikation (Industry Group 3). Die Werte sind fest und unveränderlich. Durch die Deklaration als globale Konstanten können sie in anderen Bausteinen oder Applikationen referenziert werden, ohne dass sie explizit übergeben werden müssen.

Die Konstantenwerte und ihre Bedeutungen sind:

| Konstante | Wert | Beschreibung |
|-----------|------|--------------|
| `DC_NON_SPECIFIC_SYSTEM` | 0 | Nicht spezifisches System |
| `DC_SKID_STEER_LOADER` | 1 | Skid-Steer-Lader |
| `DC_ARTICULATED_DUMP_TRUCK` | 2 | Gelenk-Muldenkipper |
| `DC_BACKHOE` | 3 | Baggerlader |
| `DC_CRAWLER` | 4 | Raupenfahrzeug |
| `DC_EXCAVATOR` | 5 | Bagger |
| `DC_FORKLIFT` | 6 | Gabelstapler |
| `DC_FOUR_WHEEL_DRIVE_LOADER` | 7 | Allrad-Lader |
| `DC_GRADER` | 8 | Straßenhobel |
| `DC_MILLING_MACHINE` | 9 | Fräsmaschine |
| `DC_RECYCLER_AND_SOIL_STABILIZER` | 10 | Recycler und Bodenstabilisator |
| `DC_BINDING_AGENT_SPREADER` | 11 | Bindemittelstreuer |
| `DC_PAVER` | 12 | Straßenfertiger |
| `DC_FEEDER` | 13 | Beschicker |
| `DC_SCREENING_PLANT` | 14 | Siebanlage |
| `DC_STACKER` | 15 | Stapelgerät |
| `DC_ROLLER` | 16 | Walze |
| `DC_CRUSHER` | 17 | Brecher |
| `DC_NOT_AVAILABLE` | 127 | Nicht verfügbar |

Die Konstanten können direkt in Programmen und Funktionsblöcken verwendet werden, z.B. um einen empfangenen Byte-Wert einer Geräteklasse zuzuordnen oder um eine solche Identifikation zu senden.

## Technische Besonderheiten

- **Typ**: `BYTE` – Wertebereich 0–255.
- **Gültigkeitsbereich**: Global, d.h. sie sind im gesamten Projekt einsehbar und nutzbar.
- **Unveränderlichkeit**: Die Konstanten sind als `CONSTANT` deklariert und können zur Laufzeit nicht verändert werden.
- **Standardkonformität**: Die Werte entsprechen den von ISO 11783 (ISOBUS) definierten Geräteklassen für Industry Group 3 (Vehicle Systems).
- **Paket**: Ursprünglich definiert im Paket `isobus::pgn::const`, was auf die Verwendung im Kontext von PGN-Konstanten hinweist.
- **Dateiformat**: Die Definition erfolgt im XML-Format für die 4diac-IDE und wird dort als Global Constants verwaltet.

## Zustandsübersicht

Da es sich um eine reine Konstantendefinition handelt, besitzt der Baustein keinen Zustandsautomaten und keine Laufzeitlogik. Die Konstanten sind permanent verfügbar und ändern ihren Wert nicht während der Programmausführung.

## Anwendungsszenarien

- **Fahrzeugidentifikation**: Wenn ein ISOBUS-fähiges Steuergerät (ECU) seine Geräteklasse in einer Botschaft (z.B. PGN) senden möchte, kann der entsprechende Konstantenwert (z.B. `DC_EXCAVATOR`) verwendet werden.
- **Empfang und Validierung**: Beim Empfang einer Nachricht mit einer Geräteklassen-Identifikation kann der Wert gegen die Konstanten verglichen werden, um die Fahrzeugart zu ermitteln und entsprechende Reaktionen auszulösen.
- **Konfiguration**: In Projekten, die unterschiedliche Maschinentypen unterstützen, können diese Konstanten zur Konfiguration von Parametern oder zur Auswahl von Steuerlogiken verwendet werden.
- **Datenvisualisierung**: Anzeige des Fahrzeugtyps in einer Bedienoberfläche durch Zuordnung des numerischen Werts zu einer Beschreibung.

## Vergleich mit ähnlichen Bausteinen

Im ISOBUS-Kontext gibt es weitere Global-Constants-Bausteine für andere Industry Groups (z.B. IG0, IG1, IG2). Diese unterscheiden sich in den definierten Konstantenwerten und deren Bedeutung. `IG3_Device_Classes` ist speziell für die Gruppe 3 (Fahrzeugsysteme) ausgelegt. Andere Bausteine könnten z.B. `IG0_Device_Classes` oder `IG1_Device_Classes` heißen und andere Listen enthalten. Der hier beschriebene Baustein stellt die spezifischen Geräteklassen für land- und baumaschinenähnliche Fahrzeuge bereit.

## Fazit

Der Baustein `IG3_Device_Classes` ist eine einfache, aber wichtige Komponente für ISOBUS-basierte Anwendungen in der 4diac-IDE. Durch die Bereitstellung standardkonformer Konstanten für Geräteklassen wird eine einheitliche und fehlerfreie Kommunikation im Agrar- und Baubereich unterstützt. Er ist unverzichtbar für Entwickler, die Fahrzeugidentifikationen in ihren Steuerungssystemen integrieren müssen. Die Verwendung als globale Konstanten erleichtert die Wartung und erhöht die Lesbarkeit des Codes erheblich.