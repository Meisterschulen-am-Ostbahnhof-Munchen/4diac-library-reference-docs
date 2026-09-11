# AID_WM

![AID_WM](./AID_WM.svg)

* * * * * * * * * *
## Einleitung

Die globale Konstantensammlung `AID_WM` definiert Attribut-IDs (Attribute Identifiers) für das **Window Mask**-Objekt im ISOBUS-Protokoll. Sie ist Teil des Pakets `isobus::UT::Q::const::AID` und stellt die numerischen Kennungen bereit, die verwendet werden, um Attribute eines Fenstermasken-Objekts (z. B. Hintergrundfarbe, Optionen, Name) in der ISO-Bus-Kommunikation anzusprechen. Die Konstanten sind als `USINT`-Werte festgelegt und dienen als Referenzwerte für die Parametrierung von Fenstermasken in Bedienterminals.

## Schnittstellenstruktur

Da es sich um eine **Globalkonstanten**-Definition handelt, besitzt der Baustein keine Ein-/Ausgänge oder Adapter. Die Schnittstellenstruktur beschränkt sich auf die Bereitstellung der nachfolgenden Konstanten:

### **Ereignis-Eingänge**

Nicht vorhanden.

### **Ereignis-Ausgänge**

Nicht vorhanden.

### **Daten-Eingänge**

Nicht vorhanden.

### **Daten-Ausgänge**

Nicht vorhanden.

### **Adapter**

Nicht vorhanden.

Stattdessen werden die folgenden **Konstanten** als Teil dieser Sammlung definiert:

| Konstante            | Typ    | Wert  | Beschreibung                                                                 |
|----------------------|--------|-------|------------------------------------------------------------------------------|
| `BACKGROUND_COLOUR`  | `USINT`| `1`   | Hintergrundfarbenindex eines Fenstermasken-Objekts.                         |
| `OPTIONS`            | `USINT`| `2`   | Bitmaske für Optionen: Bit 0 = Verfügbar (0 = deaktiviert/blank), Bit 1 = Transparent (ohne Hintergrundfarbe). |
| `NAME`               | `USINT`| `3`   | Objekt-ID einer Ausgabestring- oder Objektzeiger-Referenz, die den Namen enthält. |

## Funktionsweise

Die Konstanten werden als **Attribut-IDs** verwendet, um im ISOBUS-Protokoll auf die Eigenschaften eines Fenstermasken-Objekts zuzugreifen. Jede ID entspricht einem spezifischen Attribut:

- **`BACKGROUND_COLOUR`** (ID 1): Referenziert den Farbindex, der für die Hintergrunddarstellung der Fenstermaske verwendet wird.
- **`OPTIONS`** (ID 2): Enthält eine Bitmaske, die das Verhalten der Maske steuert (z. B. Sichtbarkeit, Transparenz).
- **`NAME`** (ID 3): Verweist auf eine Objekt-Referenz, die den Namen der Fenstermaske bereitstellt.

Diese IDs werden typischerweise zusammen mit den Methoden des ISO-Bus-Protokolls eingesetzt, um Attribute eines Fenstermasken-Objekts zu setzen oder abzufragen. Durch die Verwendung als globale Konstanten wird eine einheitliche und fehlerfreie Referenzierung im gesamten Code gewährleistet.

## Technische Besonderheiten

- **Datentyp**: Alle Konstanten sind als `USINT` (Unsigned Short Integer, 8 Bit) definiert.
- **Initialwerte**: Die Werte sind fest eingestellt (`1`, `2`, `3`) und können im Betrieb nicht verändert werden.
- **Paketzuordnung**: Die Konstanten gehören zum Paket `isobus::UT::Q::const::AID`, das speziell für Attribut-IDs im ISO-Bus-Bereich entwickelt wurde.
- **Lizenz**: Die Definition ist unter der Eclipse Public License 2.0 verfügbar (siehe Copyright-Hinweis im XML).

## Zustandsübersicht

Da es sich um eine reine Konstantendefinition handelt, gibt es keine dynamischen Zustände. Die Werte bleiben während der gesamten Laufzeit konstant und benötigen keine Initialisierung oder Zustandsverwaltung.

## Anwendungsszenarien

- **ISO-Bus-Kommunikation**: Verwendung in Steuergeräten oder Bedienterminals, die Fenstermasken-Objekte gemäß ISO 11783 (ISOBUS) verwalten.
- **Parametrierung**: Einfache Referenzierung der Attribut-IDs in Funktionsbausteinen oder Programmen, um z. B. die Hintergrundfarbe einer Maske zu ändern oder Optionen zu setzen.
- **Entwicklungsunterstützung**: Bereitstellung klarer, benannter Konstanten zur Vermeidung von „Magic Numbers“ im Quellcode.

## Vergleich mit ähnlichen Bausteinen

Es gibt weitere Globalkonstanten-Sammlungen im Paket `isobus::UT::Q::const`, z. B. für andere Objekttypen (z. B. `AID_OBJ`, `AID_ALM`). Diese folgen demselben Muster: Sie definieren Attribut-IDs für spezifische ISO-Bus-Objekte. Im Vergleich zu diesen ist `AID_WM` auf die Fenstermasken-Objekte spezialisiert und enthält nur die zugehörigen drei Attribute.

Im Gegensatz zu Funktionsbausteinen (FBs) weist die Konstantensammlung keine Verarbeitungslogik oder Ein-/Ausgangsstruktur auf; sie dient ausschließlich als Definitionscontainer.

## Fazit

Die Globalkonstanten-Sammlung `AID_WM` stellt eine unverzichtbare Grundlage für die Entwicklung von ISO-Bus-Anwendungen dar, die Fenstermasken verwenden. Durch die klar definierten, benannten Konstanten wird die Lesbarkeit und Wartbarkeit des Codes verbessert und die Einhaltung des ISO-11783-Standards unterstützt. Die einfache Struktur macht sie leicht zu integrieren und zu erweitern, falls weitere Attribut-IDs für Fenstermasken benötigt werden.