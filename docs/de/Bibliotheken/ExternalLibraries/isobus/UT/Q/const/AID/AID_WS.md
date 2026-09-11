# AID_WS

![AID_WS](./AID_WS.svg)

* * * * * * * * * *
## Einleitung

Der Baustein `AID_WS` stellt eine Sammlung von globalen Konstanten dar, die die Attribut-IDs (Attribute IDs) für ein Working Set im ISOBUS-Kontext (ISO 11783) definieren. Diese Konstanten werden verwendet, um auf spezifische Attribute eines Working Sets zuzugreifen, wie z.B. Hintergrundfarbe, Auswählbarkeit und die aktive Maske. Der Baustein ist als `GlobalConstants`-Typ in 4diac umgesetzt und dient als zentrale Referenz für diese Attribut-IDs.

## Schnittstellenstruktur

Da es sich um einen GlobalConstants-Baustein handelt, besitzt dieser weder Ereignis- noch Datenein- oder -ausgänge. Die nachfolgenden Abschnitte sind daher leer. Alle Informationen sind als Konstanten innerhalb des Bausteins definiert.

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

Der Baustein definiert drei globale Konstanten vom Typ `USINT` (Unsigned Short Integer), die jeweils eine spezifische Attribut-ID für ein Working Set repräsentieren:

- `BACKGROUND_COLOUR` (Wert 1): Diese Konstante identifiziert das Attribut „Background colour index" eines Working Sets. Der Wert entspricht der Attribut-ID `AID_WS_BACKGROUND_COLOUR`.
- `SELECTABLE` (Wert 2): Diese Konstante kennzeichnet das Attribut „Selectable", das angibt, ob ein Working Set vom Bediener ausgewählt werden kann (0 = FALSE, 1 = TRUE). Die zugehörige Attribut-ID ist `AID_WS_SELECTABLE`.
- `ACTIVE_MASK` (Wert 3): Diese Konstante verweist auf das Attribut „Active Mask", das die Objekt-ID der Daten- oder Alarmmaske angibt, die angezeigt werden soll, solange das Working Set aktiv ist. Die Attribut-ID lautet `AID_WS_ACTIVE_MASK`.

Diese Konstanten werden typischerweise in ISOBUS-Kommunikationsmodulen verwendet, um Attribute eines Working Sets über den entsprechenden Dienst (z.B. Get/Set Attribute) zu adressieren. Sie sind als `VAR_GLOBAL CONSTANT` deklariert und stehen daher im gesamten Projekt zur Verfügung.

## Technische Besonderheiten

- **Typ**: `USINT` – 8-Bit vorzeichenloser Integer, passend zu ISOBUS-Datenformaten.
- **Initialwerte**: Die Werte 1, 2 und 3 entsprechen den offiziellen Attribut-IDs gemäß ISO 11783-6.
- **Paketname**: Der Baustein ist in das Paket `isobus::UT::Q::const::AID` eingebettet, was auf eine strukturierte Bibliothek für UT (Universal Terminal) Konstanten hinweist.
- **Lizenz**: Die enthaltenen Informationen und die Deklaration unterliegen der Eclipse Public License 2.0.

## Zustandsübersicht

Als reiner Konstanten-Baustein besitzt `AID_WS` keinen internen Zustand oder Zustandsautomaten. Die Werte sind statisch und unveränderlich.

## Anwendungsszenarien

Typische Anwendungsfälle sind:

- Implementierung von ISOBOBUS-UT-Funktionen, bei denen auf Working-Set-Attribute zugegriffen wird.
- Erstellung von Dienstfunktionen zur Abfrage oder Änderung von Working-Set-Eigenschaften (z.B. Hintergrundfarbe ändern, Auswählbarkeit setzen).
- Einbindung in größere 4diac-Anwendungen zur Steuerung von Terminals in landwirtschaftlichen Maschinen.

## Vergleich mit ähnlichen Bausteinen

In der ISOBUS-Welt existieren ähnliche Konstanten-Bausteine für andere Objekttypen, z.B. `AID_OBJECT_POOL`, `AID_MASK`, `AID_SOFTKEY` etc. Diese folgen demselben Muster und definieren die jeweiligen Attribut-IDs. `AID_WS` ist spezifisch auf Working Sets zugeschnitten und enthält nur die drei oben genannten Konstanten. Andere Bausteine könnten weitere Attribute enthalten, abhängig vom Objekttyp.

## Fazit

`AID_WS` ist ein einfacher, aber essenzieller GlobalConstants-Baustein für die ISOBUS-Kommunikation. Er bietet eine klare und zentrale Definition der Attribut-IDs für Working Sets und erleichtert die Wartung und Lesbarkeit von 4diac-Anwendungen im landwirtschaftlichen Umfeld. Durch die Verwendung als globale Konstanten wird eine hohe Konsistenz und Fehlervermeidung bei der Adressierung von Attributen gewährleistet.