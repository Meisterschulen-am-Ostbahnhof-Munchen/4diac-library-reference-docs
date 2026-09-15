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

Der Baustein definiert drei globale Konstanten vom Typ `USINT` (Unsigned Short Integer), die jeweils eine spezifische Attribut-ID für ein Working Set repräsentieren (ISO 11783-6 Tabelle B.1):

- `BACKGROUND_COLOUR` (Wert `[1]`): **Read-only** für *Change Attribute* (ISO 11783-6). Hintergrundfarb-Index. Bei Objektpool-Erstellung angegeben, zur Laufzeit abfragbar via *Get Attribute Value* (F.58).
- `SELECTABLE` (Wert `[2]`): **Read-only** für *Change Attribute* (ISO 11783-6). Auswählbarkeit durch Bediener (0 = FALSE, 1 = TRUE). Bei Objektpool-Erstellung angegeben, zur Laufzeit abfragbar via *Get Attribute Value* (F.58).
- `ACTIVE_MASK` (Wert `[3]`): **Read-only** für *Change Attribute* (ISO 11783-6). Objekt-ID der aktiven Daten- oder Alarmmaske. Abfragbar via *Get Attribute Value* (F.58); zur Laufzeit anpassbar über den speziellen Befehl *Change Active Mask* (F.34).

Diese Konstanten werden typischerweise in ISOBUS-Kommunikationsmodulen verwendet, um Attribute eines Working Sets über den entsprechenden Dienst (z.B. Get Attribute Value) zu adressieren. Sie sind als `VAR_GLOBAL CONSTANT` deklariert und stehen daher im gesamten Projekt zur Verfügung.

## Technische Besonderheiten

- **Typ**: `USINT` – 8-Bit vorzeichenloser Integer, passend zu ISOBUS-Datenformaten.
- **Initialwerte**: Die Werte 1, 2 und 3 entsprechen den offiziellen Attribut-IDs gemäß ISO 11783-6. Alle drei Attribute sind für *Change Attribute* (F.38) schreibgeschützt.
- **Paketname**: Der Baustein ist in das Paket `isobus::UT::Q::const::AID` eingebettet, was auf eine strukturierte Bibliothek für UT (Universal Terminal) Konstanten hinweist.
- **Lizenz**: Die enthaltenen Informationen und die Deklaration unterliegen der Eclipse Public License 2.0.

## Zustandsübersicht

Als reiner Konstanten-Baustein besitzt `AID_WS` keinen internen Zustand oder Zustandsautomaten. Die Werte sind statisch und unveränderlich.

## Anwendungsszenarien

Typische Anwendungsfälle sind:

- Implementierung von ISOBUS-UT-Funktionen, bei denen auf Working-Set-Attribute zugegriffen wird.
- Erstellung von Dienstfunktionen zur Abfrage von Working-Set-Eigenschaften via *Get Attribute Value* (F.58).
- Umschalten der aktiven Maske eines Working Sets zur Laufzeit mittels *Change Active Mask* (F.34).
- Einbindung in größere 4diac-Anwendungen zur Steuerung von Terminals in landwirtschaftlichen Maschinen.

## Vergleich mit ähnlichen Bausteinen

In der ISOBUS-Welt existieren ähnliche Konstanten-Bausteine für andere Objekttypen, z.B. `AID_OBJECT_POOL`, `AID_MASK`, `AID_SOFTKEY` etc. Diese folgen demselben Muster und definieren die jeweiligen Attribut-IDs. `AID_WS` ist spezifisch auf Working Sets zugeschnitten und enthält nur die drei oben genannten Konstanten. Andere Bausteine könnten weitere Attribute enthalten, abhängig vom Objekttyp.

## Fazit

`AID_WS` ist ein einfacher, aber essenzieller GlobalConstants-Baustein für die ISOBUS-Kommunikation. Er bietet eine klare und zentrale Definition der Attribut-IDs für Working Sets und erleichtert die Wartung und Lesbarkeit von 4diac-Anwendungen im landwirtschaftlichen Umfeld. Durch die Verwendung als globale Konstanten wird eine hohe Konsistenz und Fehlervermeidung bei der Adressierung von Attributen gewährleistet.
