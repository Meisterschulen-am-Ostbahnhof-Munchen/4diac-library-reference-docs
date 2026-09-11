# AID_SG

![AID_SG](./AID_SG.svg)

* * * * * * * * * *
## Einleitung
AID_SG ist eine Sammlung von globalen Konstanten, die die Attribut-IDs für skalierte Grafikobjekte im ISOBUS-Protokoll definieren. Diese Konstanten werden verwendet, um auf spezifische Eigenschaften wie Breite, Höhe, Skalierungsart, Optionen und Wert zuzugreifen. Die Definitionen sind als `GLOBALCONSTANTS`-Block in 4diac implementiert und dienen als Referenzwerte für die Kommunikation mit ISOBUS-fähigen Geräten.

## Schnittstellenstruktur
Da es sich bei AID_SG um eine Konstantendefinition und nicht um einen Funktionsblock handelt, existieren keine Ein‑ oder Ausgänge im herkömmlichen Sinne. Die Struktur beschränkt sich auf die Definition von globalen Konstanten.

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

## Funktionsweise
Die Konstanten in AID_SG bilden eine eindeutige Kennung (Attribut-ID) für verschiedene Attribute eines skalierten Grafikobjekts. In ISOBUS‑Nachrichten werden diese IDs als Parameterwerte verwendet, um dem Empfänger mitzuteilen, welches Attribut (z. B. Breite oder Skalierungsart) gesetzt oder abgefragt werden soll. Durch die Verwendung von globalen Konstanten wird eine einheitliche und fehlerfreie Referenzierung im gesamten Projekt sichergestellt.

## Technische Besonderheiten
Die einzelnen Konstanten sind wie folgt definiert:

| Konstante | Wert | Bedeutung |
|-----------|------|-----------|
| `WIDTH`   | 1    | AID_SG_WIDTH – Breite in Pixeln |
| `HEIGHT`  | 2    | AID_SG_HEIGHT – Höhe in Pixeln |
| `SCALE_TYPE` | 3 | AID_SG_SCALE_TYPE – Bits 0‑2: Skalierungswert (0 = keine Skalierung, 1 = auf Breite skalieren, 2 = auf Höhe skalieren, 3 = auf Breite und Höhe, 4 = auf passende Größe). Bits 3‑4: Horizontale Ausrichtung (0 = links, 1 = mittig, 2 = rechts). Bits 5‑6: Vertikale Ausrichtung (0 = oben, 1 = mittig, 2 = unten) |
| `OPTIONS` | 4    | AID_SG_OPTIONS – Bitmaske: Bit 0 = Blinken |
| `VALUE`   | 5    | AID_SG_VALUE – Aktueller Wert |

Diese Konstanten sind vom Typ `USINT` (Unsigned Short Integer) und tragen feste Werte, die der ISOBUS‑Spezifikation entsprechen.

## Zustandsübersicht
Nicht anwendbar, da AID_SG keine Zustandslogik besitzt.

## Anwendungsszenarien
AID_SG wird in ISOBUS‑basierten Steuerungssystemen eingesetzt, um skalierte Grafikobjekte auf Bedienterminals darzustellen. Typische Anwendungen umfassen:

- Setzen der Breite oder Höhe eines grafischen Objekts über die entsprechenden Attribut‑IDs.
- Konfiguration der Skalierungsart und Ausrichtung mithilfe des `SCALE_TYPE`.
- Steuerung von Blinkeffekten über das `OPTIONS`‑Flag.
- Übertragung des aktuellen Anzeigewerts über `VALUE`.

## Vergleich mit ähnlichen Bausteinen
Im ISOBUS‑Umfeld existieren vergleichbare Konstantendefinitionen für andere Objekttypen, z. B. für Textobjekte (`AID_Text`) oder Linien (`AID_Line`). Diese folgen dem gleichen Muster: Sie definieren numerische IDs für die jeweiligen Attribute. Im Unterschied zu einem Funktionsblock bieten sie jedoch keine Logik, sondern stellen lediglich eine Referenztabelle für die Kommunikation bereit.

## Fazit
AID_SG stellt eine wichtige Grundlage für die Implementierung ISOBUS‑konformer Geräte dar, indem es eine klare und standardisierte Referenz für Attribut‑IDs skalierten Grafikobjekte bereitstellt. Durch die Verwendung als globale Konstanten wird die Lesbarkeit und Wartbarkeit des Codes erhöht und die Einhaltung des ISOBUS‑Protokolls erleichtert.