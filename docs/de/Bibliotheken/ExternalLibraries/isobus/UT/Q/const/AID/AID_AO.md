# AID_AO

![AID_AO](./AID_AO.svg)

* * * * * * * * * *

## Einleitung

Der Baustein **AID_AO** ist ein globaler Konstanten-Baustein der 4diac-IDE, der die Attribut-IDs für Animationsobjekte (Animation Objects) im ISOBUS-Kontext definiert. Er stellt neun benannte Konstanten vom Typ `USINT` bereit, die in der Kommunikation mit ISOBUS-Terminals zur Identifikation von Objektattributen verwendet werden. Dieser Baustein dient ausschließlich als Nachschlagewerk für Entwickler und wird nicht direkt in einer Applikation instanziiert, sondern global referenziert.

## Schnittstellenstruktur

Da es sich um einen GlobalConstants-Baustein handelt, besitzt er keine klassischen Ein-/Ausgänge oder Ereignisse. Die **Schnittstellenstruktur** besteht ausschließlich aus den globalen Konstanten, die als Datenwerte für andere Bausteine verfügbar sind.

### **Ereignis-Eingänge**

Keine.

### **Ereignis-Ausgänge**

Keine.

### **Daten-Eingänge**

Keine.

### **Daten-Ausgänge**

Keine – die Konstanten werden nicht als Ausgänge geführt, sondern global im Projekt bereitgestellt. Sie können über den Namen (z.B. `AID_AO.WIDTH`) direkt in Ausdrücken verwendet werden.

### **Adapter**

Keine.

| Konstante | Wert | Beschreibung |
|-----------|------|--------------|
| `WIDTH`        | 1 | AID_AO_WIDTH – Breite in Pixeln |
| `HEIGHT`       | 2 | AID_AO_HEIGHT – Höhe in Pixeln |
| `REFRESHINT`   | 3 | AID_AO_REFRESHINT – Aktualisierungsintervall |
| `VALUE`        | 4 | AID_AO_VALUE – aktueller Wert |
| `ENABLED`      | 5 | AID_AO_ENABLED – 0 = gestoppt, 1 = animiert |
| `FICHILDINDEX` | 6 | AID_AO_FICHILDINDEX – Index des ersten Kindes |
| `LACHILDINDEX` | 7 | AID_AO_LACHILDINDEX – Index des letzten Kindes |
| `DEFCHILDINDEX`| 8 | AID_AO_DEFCHILDINDEX – Index des Standard-Kindes |
| `OBJECTS`      | 9 | AID_AO_OBJECTS – Optionsbits (siehe unten) |

## Funktionsweise

Dieser Baustein definiert Konstanten, die die numerischen Kennungen der Attribute eines Animationsobjekts im ISOBUS-Objektmodell festlegen. Diese IDs werden typischerweise verwendet, um über den ISOBUS-Dienst „Get/Set Attribute“ gezielt Eigenschaften eines Animationsobjekts auszulesen oder zu verändern. Die Konstanten sind als `USINT` (Unsigned Short Integer) deklariert und fest mit den Werten 1 bis 9 belegt.

Die Bedeutung der `OBJECTS`-Konstante wird in den Kommentaren näher erläutert:  

- **Bit 0**: Bestimmt die Animationssequenz (0 = Einzelschuss, 1 = Schleife)  
- **Bits 1-2**: Legen das Verhalten bei deaktivierter Animation fest:  
  - 0 = Pause  
  - 1 = Zurücksetzen auf erstes Element  
  - 2 = Standardobjekt anzeigen  
  - 3 = Leer lassen  

## Technische Besonderheiten

- Der Baustein ist als **GlobalConstants**-Element gekapselt und wird im Compilerinfo als `isobus::UT::Q::const::AID` definiert.
- Alle Konstanten sind mit `VAR_GLOBAL CONSTANT` deklariert, d.h. sie sind schreibgeschützt und projektweit gültig.
- Die Werte sind als Literale mit Präfix `USINT#` versehen, um den Datentyp eindeutig festzulegen.
- Der Baustein enthält Versionsinformationen (Version 1.0, Org: HR Agrartechnik GmbH, Autor: Franz Höpfinger, Datum: 2026-06-20) und unterliegt der Eclipse Public License 2.0 (EPL-2.0).
- Es wird keine grafische Darstellung oder Zustandslogik benötigt; der Baustein dient rein der Konstantendefinition.

## Zustandsübersicht

Nicht zutreffend – es existieren keine dynamischen Zustände, da der Baustein ausschließlich statische Konstanten bereitstellt.

## Anwendungsszenarien

- **ISOBUS-Implementierung**: Verwendung der Konstanten beim Aufbau von Nachrichten zum Lesen oder Schreiben von Attributen eines Animationsobjekts (z.B. `OBJECTS`, `ENABLED`, `WIDTH`).
- **Objektsteuerung**: Steuerung der Animation eines ISOBUS-Objekts durch Setzen der Werte für `ENABLED` und `OBJECTS`.
- **Konfiguration**: Verwaltung von Größe (`WIDTH`, `HEIGHT`) und Aktualisierungsintervall (`REFRESHINT`) eines animierten Objekts.
- **Entwicklungshilfe**: Dient als zentrale Referenz für ID-Nummern, um Fehlzuordnungen in der Programmierung zu vermeiden.

## Vergleich mit ähnlichen Bausteinen

Es existieren weitere GlobalConstants-Bausteine für andere Objekttypen, z.B. `AID_OBJ` für generische Objektattribute oder `AID_WD` für Working Set-Definitionen. Im Vergleich zu diesen definiert `AID_AO` spezifisch die Attribute für Animationsobjekte und deckt damit die besonderen Eigenschaften ab, die bei animierten Darstellungen benötigt werden (z.B. Sequenzmodus, Verhalten bei Deaktivierung). Die Konstanten sind bewusst auf die ISOBUS-Spezifikation abgestimmt.

## Fazit

Der Baustein `AID_AO` ist eine einfache, aber essenzielle Konstantensammlung für die Entwicklung ISOBUS-konformer Anwendungen mit animierten Objekten. Er füllt die Lücke zwischen abstrakten Objekt-IDs und den spezifischen Attributen eines Animationsobjekts und erleichtert die Einhaltung der ISOBUS-Standards. Durch die zentrale Definition wird eine konsistente Verwendung der ID-Werte im gesamten Projekt gewährleistet.
