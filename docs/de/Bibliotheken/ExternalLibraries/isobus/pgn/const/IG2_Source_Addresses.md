# IG2_Source_Addresses

![IG2_Source_Addresses](./IG2_Source_Addresses.svg)

* * * * * * * * * *
## Einleitung
Der Baustein `IG2_Source_Addresses` ist eine Sammlung von globalen Konstanten, die die festgelegten **Source Addresses** für die **Industry Group 2 (IG2)** im ISOBUS-Kommunikationsprotokoll (ISO 11783) definiert. Diese Adressen werden für verschiedene Funktionen und Steuergeräte in landwirtschaftlichen Maschinen verwendet und sind je nach Zweck entweder fest reserviert oder für die Selbstkonfiguration vorgesehen. Die Konstanten sind als `BYTE`-Typ (8-Bit-Wert) mit initialen Werten von 128 bis 247 definiert und finden in der Automatisierungstechnik für Traktor-Implement-Kommunikation Anwendung.

## Schnittstellenstruktur
Der Baustein besitzt keine dynamischen Schnittstellen wie Ereignisse oder Datenein-/ausgänge, da er ausschließlich globale Konstanten bereitstellt. Dennoch werden die Konstanten im Folgenden als **Daten-Ausgänge** aufgeführt, da sie von anderen Funktionsblöcken im gesamten Projekt ausgelesen werden können.

### **Ereignis-Eingänge**
Nicht vorhanden. Es gibt keine erwartbaren Ereignisse, die diesen Baustein auslösen.

### **Ereignis-Ausgänge**
Nicht vorhanden. Der Baustein erzeugt keine Ereignisse, da er nur statische Konstanten liefert.

### **Daten-Eingänge**
Keine. Es werden keine Werte von außen übergeben.

### **Daten-Ausgänge**
Die folgenden Konstanten sind als globale Datenwerte verfügbar:

| Name | Wert | Kommentar |
|------|------|-----------|
| `SA_RESERVED_128` | 128 | Adresse 128 bis 235 sind von ISO für die selbstkonfigurierbare Adressierung reserviert. |
| `SA_DATA_LOGGER` | 236 | Datenlogger – Server, der die Protokollierungsfunktion gemäß ISO 11783-10 bereitstellt. |
| `SA_TIM_SERVER` | 237 | TIM-Server – Steuerfunktion für Tractor Implement Management (Operator-Assistance-System). |
| `SA_SEQUENCE_CONTROLLER` | 238 | Sequenzcontroller – Master im Sequenzsteuerungssystem nach ISO 11783-14. |
| `SA_POSITION_CONTROL` | 239 | Positionssteuerung – Mehr-Achsen-Positionssteuerung beweglicher Geräteelemente. |
| `SA_TRACTOR_ECU` | 240 | Traktor-ECU – Gateway zwischen Traktor und Implement-Bus. |
| `SA_TAILINGS_MONITORING` | 241 | Monitoring der Rücklaufmenge (unausgedroschenes Material) zur Dreschmaschine. |
| `SA_HEADER_CONTROL` | 242 | Steuerung der Haspeldrehzahl und Materialzufuhrrate des Schneidwerks. |
| `SA_PRODUCT_LOSS_MONITORING` | 243 | Überwachung des Produktverlusts während des Ernteprozesses. |
| `SA_PRODUCT_MOISTURE_SENSING` | 244 | Überwachung der Feuchtigkeit des Ernteguts. |
| `SA_NON_VIRTUAL_TERMINAL_DISPLAY_IMPLEMENT_BUS` | 245 | Nicht-Virtual-Terminal-Display am Implement-Bus – Anzeige in der Kabine. |
| `SA_OPERATOR_CONTROLS_MACHINE_SPECIFIC` | 246 | Maschinenspezifische Bedienerelemente – nicht-auxiliäre Bedienereingaben. |
| `SA_TASK_CONTROL_MAPPING_COMPUTER` | 247 | Task-Control-Rechner – sendet, empfängt und loggt Prozessdaten. |

### **Adapter**
Keine. Es existieren keine Adapter-Schnittstellen.

## Funktionsweise
`IG2_Source_Addresses` ist ein reiner Konstanten-Baustein. Er definiert feste numerische Werte, die als symbolische Namen im gesamten Projekt verwendet werden können. Die Werte entsprechen den im ISOBUS-Standard vergebenen Source-Adressen für spezifische Funktionen innerhalb der Industrie- und Landwirtschaftsgruppe 2. Durch die Verwendung dieser Konstanten wird sichergestellt, dass Adressen nicht hart verdrahtet oder fehleranfällig in mehreren Bausteinen wiederholt werden müssen. Die Konstanten sind global verfügbar und können von jedem anderen Funktionsblock oder Programm gelesen werden.

## Technische Besonderheiten
- Die Adressen von 128 bis 235 sind für die **selbstkonfigurierbare Adressvergabe** reserviert. Geräte der IG2, die eine bevorzugte Adresse verwenden, müssen in der Lage sein, ihre Adresse automatisch zu konfigurieren.
- Ab Adresse 236 beginnt der Bereich der **fest zugewiesenen** Source-Adressen für spezielle Funktionen (Datenlogger, TIM usw.).
- Alle Konstanten sind vom Typ `BYTE` und besitzen einen Initialwert im Bereich 0..255.
- Der Baustein stellt keine Laufzeitlogik dar; er definiert lediglich benannte Konstanten, die zur Verbesserung der Lesbarkeit und Wiederverwendbarkeit des Codes beitragen.

## Zustandsübersicht
Da der Baustein keine Zustandsautomaten oder Verhaltenslogik besitzt, gibt es keine Zustände. Die Werte sind statisch und unveränderlich.

## Anwendungsszenarien
- Implementierung eines ISOBUS-Kommunikationsmoduls, bei dem auf die festgelegten Source-Adressen von verschiedenen Funktionsblöcken zugegriffen werden muss.
- Entwicklung von Traktor-Implement-Gateways oder Steuergeräten, die die standardisierten Adressen benötigen, um mit anderen Komponenten zu kommunizieren.
- Einbindung in ein größeres Projekt zur Vereinfachung der Wartung, wenn sich Adressen in Zukunft ändern sollten – dann müssen nur die Konstantenwerte angepasst werden.

## Vergleich mit ähnlichen Bausteinen
Im Vergleich zu anderen Konstanten-Bausteinen, die beispielsweise für Netzwerkprotokolle oder Konfigurationsparameter existieren, spezialisiert sich `IG2_Source_Addresses` auf die spezifischen Source-Adressen der ISOBUS-Industriegruppe 2. Andere Bausteine könnten z.B. `IG1_Source_Addresses` für Gruppe 1 oder allgemeine Konstanten für PGN-Nummern sein. Der vorliegende Baustein enthält jedoch nur eine begrenzte, aber klar definierte Menge an Werten, die direkt aus dem Standard übernommen wurden.

## Fazit
Der Baustein `IG2_Source_Addresses` ist eine nützliche, modularisierte Sammlung von ISOBUS-Standardadressen für die Industriegruppe 2. Er erhöht die Wartbarkeit und Lesbarkeit von Automatisierungsprojekten in der Landtechnik, indem er zentrale Konstanten bereitstellt. Da er keine Dynamik besitzt, ist er einfach zu integrieren und benötigt keine besondere Laufzeitressourcen. Durch die klare Zuordnung der Werte zu den jeweiligen Funktionen trägt er zur Fehlervermeidung und Standardkonformität bei.