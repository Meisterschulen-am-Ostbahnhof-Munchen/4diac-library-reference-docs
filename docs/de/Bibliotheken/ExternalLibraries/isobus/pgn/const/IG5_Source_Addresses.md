# IG5_Source_Addresses

![IG5_Source_Addresses](./IG5_Source_Addresses.svg)

* * * * * * * * * *
## Einleitung
Der Baustein **IG5_Source_Addresses** ist ein globaler Konstantenblock (GlobalConstants) im Rahmen der 4diac-IDE und der IEC 61499-Norm. Er definiert eine Sammlung von Quelladressen (Source Addresses) für das ISOBUS-Protokoll (ISO 11783), die speziell für die *Industry Group 5* – also für Anwendungen im Bereich Fahrzeug- und Generatorsteuerung – reserviert sind. Diese Konstanten ermöglichen eine einheitliche und wartbare Adressierung von Steuergeräten innerhalb eines ISOBUS-Netzwerks.

## Schnittstellenstruktur
Da es sich um einen GlobalConstants-Baustein handelt, besitzt er weder Ereignis- noch Datenschnittstellen. Stattdessen stellt er eine Reihe von benannten Konstanten bereit, die in anderen Funktionsbausteinen über ihren globalen Namen referenziert werden können.

### **Ereignis-Eingänge**
Keine.

### **Ereignis-Ausgänge**
Keine.

### **Daten-Eingänge**
Keine.

### **Daten-Ausgänge**
Keine (nur Konstanten, die als Global Constants verfügbar sind).

### **Adapter**
Keine.

## Funktionsweise
Der Baustein definiert die unten aufgeführten Konstanten, jeweils vom Typ `BYTE`. Diese Werte sind fest vorgegeben und können zur Laufzeit nicht verändert werden. Durch die Verwendung dieser benannten Konstanten können Entwickler auf standardisierte Quelladressen in ISOBUS-Kommunikation verweisen, ohne sich die numerischen Werte merken zu müssen. Die Zuordnung entspricht den Spezifikationen der SAE J1939/ISOBUS.

Die folgenden Konstanten sind enthalten:

| Konstante | Wert | Beschreibung |
|-----------|------|--------------|
| `SA_RESERVED_128` | 128 | Reservierter Adressbereich (128–207) für dynamische Adressvergabe |
| `SA_RESERVED_208` | 208 | Reservierter Adressbereich (208–229) für individuell vorbelegte Adressen |
| `SA_GENERATOR_VOLTAGE_REGULATOR` | 230 | Generator-Spannungsregler – steuert die Generator-Ausgangsspannung |
| `SA_ENGINE_3` | 231 | Motor Nr. 3 – ECU des dritten Motors in einem System |
| `SA_ENGINE_4` | 232 | Motor Nr. 4 – ECU des vierten Motors |
| `SA_ENGINE_5` | 233 | Motor Nr. 5 – ECU des fünften Motors |
| `SA_GENERATOR_SET_CONTROLLER` | 234 | Generator-Set-Controller – Datenerfassung und Steuerung einer Generatoranlage |
| `SA_SUPPLEMENTAL_SENSOR_PROCESSING_UNIT_1` | 235 | Zusätzliche Sensorverarbeitungseinheit Nr. 1 |
| `SA_SUPPLEMENTAL_SENSOR_PROCESSING_UNIT_2` | 236 | Zusätzliche Sensorverarbeitungseinheit Nr. 2 |
| `SA_SUPPLEMENTAL_SENSOR_PROCESSING_UNIT_3` | 237 | Zusätzliche Sensorverarbeitungseinheit Nr. 3 |
| `SA_SUPPLEMENTAL_SENSOR_PROCESSING_UNIT_4` | 238 | Zusätzliche Sensorverarbeitungseinheit Nr. 4 |
| `SA_SUPPLEMENTAL_SENSOR_PROCESSING_UNIT_5` | 239 | Zusätzliche Sensorverarbeitungseinheit Nr. 5 |
| `SA_SUPPLEMENTAL_SENSOR_PROCESSING_UNIT_6` | 240 | Zusätzliche Sensorverarbeitungseinheit Nr. 6 |
| `SA_ENGINE_MONITOR_1` | 241 | Motorüberwachung Nr. 1 |
| `SA_ENGINE_MONITOR_2` | 242 | Motorüberwachung Nr. 2 |
| `SA_ENGINE_MONITOR_3` | 243 | Motorüberwachung Nr. 3 |
| `SA_ENGINE_MONITOR_4` | 244 | Motorüberwachung Nr. 4 |
| `SA_ENGINE_MONITOR_5` | 245 | Motorüberwachung Nr. 5 |
| `SA_ENGINE_MONITOR_6` | 246 | Motorüberwachung Nr. 6 |
| `SA_ENGINE_MONITOR_7` | 247 | Motorüberwachung Nr. 7 |

## Technische Besonderheiten
- Die Werte entsprechen den in der ISO 11783 (ISOBUS) und SAE J1939 festgelegten Adressbereichen für die **Industry Group 5**.
- Einige Adressen (128–229) sind reserviert und werden für spezielle Zwecke (dynamische oder individuelle Vergabe) verwendet. Die übrigen sind konkreten Funktionseinheiten zugeordnet.
- Die Konstanten sind als **globale Konstanten** deklariert, d.h. sie sind in allen Bausteinen eines 4diac-Projekts sichtbar und nutzbar.
- Der Baustein ist Teil des Pakets `isobus::pgn::const` und dient als referenzierbare Definition für die PGN‑basierten Kommunikationsdienste.

## Zustandsübersicht
Nicht anwendbar – dieser Baustein besitzt keinen Zustandsautomaten und keine internen Zustandslogik. Er dient ausschließlich als Konstantensammlung.

## Anwendungsszenarien
- **ISOBUS-Netzwerkkonfiguration**: Beim Aufbau eines ISOBUS-Netzes für landwirtschaftliche Fahrzeuge oder Generatorsteuerungen können die definierten Quelladressen direkt als globale Konstanten verwendet werden, um die Kommunikationsparameter einheitlich festzulegen.
- **Entwicklung von Steuerungsfunktionen**: Funktionsbausteine, die mit ISOBUS-Geräten kommunizieren (z.B. Motorsteuerung, Generatorsteuerung), können diese Konstanten nutzen, um die Zieladressen zu spezifizieren, ohne hartkodierte Zahlen verwenden zu müssen.
- **Systemerweiterungen**: Bei Hinzufügen weiterer Geräte können die entsprechenden Konstanten in anderen Bausteinen referenziert werden, um eine klare und wartbare Adressverwaltung zu gewährleisten.

## Vergleich mit ähnlichen Bausteinen
In der 4diac-Bibliothek für ISOBUS existieren möglicherweise weitere GlobalConstants-Bausteine für andere Industry Groups (z.B. IG1, IG2, ...). Diese folgen dem gleichen Muster und definieren die entsprechenden Quelladressen für ihre jeweilige Gruppe. Der vorliegende Baustein ist speziell auf die Anforderungen der **Industry Group 5** zugeschnitten. Andere Bausteine könnten z.B. Adressen für landwirtschaftliche Geräte (IG1) oder Baumaschinen (IG2) enthalten.

## Fazit
Der **IG5_Source_Addresses**-Baustein stellt eine klar definierte und standardkonforme Sammlung von Quelladressen für ISOBUS-Anwendungen der Industry Group 5 bereit. Durch die Verwendung als globale Konstanten wird die Lesbarkeit und Wartbarkeit von ISOBUS-Kommunikationsfunktionen erheblich verbessert. Er ist ein essentielles Hilfsmittel für Entwickler, die auf die Adressierung von Generator-, Motor- und Überwachungseinheiten in einem ISOBUS-Netzwerk zugreifen müssen.