# AID_ON

![AID_ON](./AID_ON.svg)

* * * * * * * * * *
## Einleitung

Der Baustein `AID_ON` ist ein **GlobalConstants**-Baustein, der die numerischen Kennungen (Attribute IDs) für die Attribute eines **Output Number Objects** im ISOBUS definiert. Diese Konstanten werden verwendet, um auf einzelne Attribute eines solchen Objekts im Rahmen der ISO 11783-6 bzw. ISO 11783-10 (ISOBUS) zuzugreifen. Der Baustein stellt eine zentrale, wiederverwendbare Sammlung von Konstanten bereit, die in verschiedenen Funktionsbausteinen und Applikationen genutzt werden können, um die Lesbarkeit und Wartbarkeit des Codes zu verbessern.

## Schnittstellenstruktur

Da es sich um einen GlobalConstants-Baustein handelt, besitzt er **keine Ein-/Ausgänge** im herkömmlichen Sinne. Die Schnittstellenstruktur besteht ausschließlich aus den deklarierten Konstanten, die als globale Konstanten über `VAR_GLOBAL CONSTANT` definiert sind.

### **Ereignis-Eingänge**

Keine – der Baustein besitzt keine Ereignis-Eingänge.

### **Ereignis-Ausgänge**

Keine – der Baustein besitzt keine Ereignis-Ausgänge.

### **Daten-Eingänge**

Keine – der Baustein besitzt keine Daten-Eingänge.

### **Daten-Ausgänge**

Keine – der Baustein besitzt keine Daten-Ausgänge, da die Werte als globale Konstanten direkt im Code verfügbar sind.

### **Adapter**

Keine – der Baustein besitzt keine Adapter-Schnittstellen.

## Funktionsweise

Der Baustein definiert zwölf Konstanten vom Typ `USINT` (Unsigned Short Integer), die jeweils eine eindeutige Attribut-ID für ein **Output Number Object** im ISOBUS repräsentieren. Die Zuordnung ist wie folgt:

| Konstante            | Wert | Beschreibung                                                        |
|----------------------|------|---------------------------------------------------------------------|
| `WIDTH`              | 1    | Breite in Pixeln                                                     |
| `HEIGHT`             | 2    | Höhe in Pixeln                                                       |
| `BACKGROUND_COLOUR`  | 3    | Index der Hintergrundfarbe                                           |
| `FONT_ATT`           | 4    | Objekt-ID eines Font-Attribut-Objekts                               |
| `OPTIONS`            | 5    | Options-Bitmaske                                                     |
| `VARIABLE_REF`       | 6    | Objekt-ID einer Number Variable (Zahlenvariable)                     |
| `OFFSET`             | 7    | Offset zur Anzeige des Wertes                                        |
| `SCALE`              | 8    | Skalierungsfaktor zur Anzeige des Wertes                             |
| `NUMB_DECIMALS`      | 9    | Anzahl Dezimalstellen                                                |
| `FORMAT`             | 10   | Format: 0 = Festkomma (####.nn), 1 = Exponentialformat               |
| `JUSTIFICATION`      | 11   | Ausrichtung: Bits 0-1 horizontal (0=links, 1=mittig, 2=rechts), Bits 2-3 vertikal (0=oben, 1=mittig, 2=unten) |
| `VALUE`              | 12   | Aktueller Wert vor Skalierung                                        |

Diese Konstanten werden typischerweise bei der Kommunikation mit ISOBUS-Terminals verwendet, z.B. beim Erstellen oder Modifizieren von Objekten über die VT-Protokollschnittstelle. Durch die Verwendung dieser symbolischen Namen wird der Code lesbarer und weniger fehleranfällig, da anstelle von magischen Zahlen sprechende Konstanten verwendet werden.

## Technische Besonderheiten

- **Paketzugehörigkeit:** Die Konstanten sind im Paket `isobus::UT::Q::const::AID` organisiert, wodurch eine saubere Namensraumtrennung innerhalb des ISOBUS-Moduls erreicht wird.
- **Typsicherheit:** Alle Werte sind als `USINT` deklariert, was eine Kompatibilität mit den im ISOBUS definierten Attribut-IDs gewährleistet (1-255).
- **Konstantheit:** Die Deklaration erfolgt als `VAR_GLOBAL CONSTANT`, sodass die Werte zur Laufzeit nicht verändert werden können.
- **Kommentierung:** Jede Konstante ist mit einem erklärenden Kommentar versehen, der die Bedeutung laut ISOBUS-Spezifikation beschreibt.
- **Standardkonformität:** Der Baustein basiert auf dem IEEE 61499-1 Standard und ist im 4diac-IDE-Format als XML-Datei definiert.

## Zustandsübersicht

Da es sich um einen reinen Konstanten-Baustein handelt, existiert **kein Zustandsautomat** und keine dynamischen Zustände. Der Baustein ist statisch und hat keine Laufzeitlogik. Ein Einsatz im Rahmen von ereignisgesteuerten Abläufen ist nicht vorgesehen.

## Anwendungsszenarien

- **ISOBUS-VT-Applikationen:** Bei der Erstellung von Bedienoberflächen für Traktoren oder Landmaschinen werden Output Number Objects benötigt, um numerische Werte (z.B. Motordrehzahl, Geschwindigkeit) anzuzeigen. Die Konstanten erlauben das eindeutige Referenzieren der zu setzenden Attribute.
- **Konfiguration von Terminalanzeigen:** Beim Senden von Kommandos zum Setzen von Attributen (z.B. `SetAttribute`) können die Konstanten verwendet werden, um die gewünschte Eigenschaft zu identifizieren.
- **Entwicklungsunterstützung:** Die Verwendung sprechender Konstanten reduziert Tippfehler und erhöht die Wartbarkeit von ISOBUS-Implementierungen.

## Vergleich mit ähnlichen Bausteinen

Innerhalb des Pakets `isobus::UT::Q::const` existieren vermutlich ähnliche GlobalConstants-Bausteine für andere Objekttypen wie z.B.:
- `AID_STRING` für Output String Objects
- `AID_LIST` für Output List Objects
- `AID_ANALOG` für Analoganzeigen

Diese unterscheiden sich in der Anzahl und Semantik der Konstanten, folgen jedoch dem gleichen Muster. `AID_ON` ist speziell auf das Output Number Object zugeschnitten und stellt dessen charakteristische Attribute bereit.

## Fazit

Der `AID_ON`-Baustein ist eine nützliche, statische Konstantensammlung für ISOBUS-Entwickler. Er zentralisiert die für Output Number Objects relevanten Attribut-IDs und verbessert so die Lesbarkeit und Robustheit von Anwendungen. Die klare Strukturierung durch Paket, Kommentare und die Verwendung des IEEE 61499-1-Formats machen ihn zu einem guten Beispiel für einen wiederverwendbaren globalen Konstantenbaustein in 4diac.