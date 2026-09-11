# AID_OABG

![AID_OABG](./AID_OABG.svg)

* * * * * * * * * *
## Einleitung

Der Baustein `AID_OABG` ist ein GlobalConstants-Baustein, der Konstanten für Attribut-IDs eines „Output arched bar graph object“ (bogenförmiges Balkendiagramm-Objekt) im ISOBUS-Kontext definiert. Diese Konstanten werden verwendet, um auf die verschiedenen Attribute eines solchen Objekts zuzugreifen, z. B. Breite, Höhe, Farbe, Optionen, Winkel usw. Der Baustein ist Teil des Pakets `isobus::UT::Q::const::AID`.

## Schnittstellenstruktur

Da es sich um einen GlobalConstants-Baustein handelt, besitzt er keine Ereignis-Eingänge oder -Ausgänge und auch keine Adapter. Die Schnittstelle besteht ausschließlich aus globalen Konstanten, die als Datenwerte zur Verfügung stehen. Im Folgenden sind die Konstanten als „Daten-Ausgänge“ aufgeführt, da sie von anderen Bausteinen gelesen werden können.

### **Ereignis-Eingänge**
Nicht vorhanden.

### **Ereignis-Ausgänge**
Nicht vorhanden.

### **Daten-Eingänge**
Nicht vorhanden.

### **Daten-Ausgänge**

| Name | Typ | Wert | Kommentar |
|------|-----|------|-----------|
| `WIDTH` | USINT | 1 | AID_OABG_WIDTH – Breite in Pixeln |
| `HEIGHT` | USINT | 2 | AID_OABG_HEIGHT – Höhe in Pixeln |
| `COLOUR` | USINT | 3 | AID_OABG_COLOUR – Farbindex |
| `TARGET_LINE_COLOUR` | USINT | 4 | AID_OABG_TARGET_LINE_COLOUR – Farbe der Ziellinie |
| `OPTIONS` | USINT | 5 | AID_OABG_OPTIONS – Bitmaske: Bit 0=Border zeichnen, Bit 1=Ziellinie zeichnen, Bit 2=undefiniert (0), Bit 3=Typ (0=geflutet, 1=einzelne Linie), Bit 4=Abweichung (0=gegen den Uhrzeigersinn, 1=im Uhrzeigersinn) |
| `START_ANGLE` | USINT | 6 | AID_OABG_START_ANGLE – Startwinkel |
| `END_ANGLE` | USINT | 7 | AID_OABG_END_ANGLE – Endwinkel |
| `BAR_GRAPH_WIDTH` | USINT | 8 | AID_OABG_BAR_GRAPH_WIDTH – Breite des Balkendiagramms |
| `MIN_VALUE` | USINT | 9 | AID_OABG_MIN_VALUE – Minimalwert |
| `MAX_VALUE` | USINT | 10 | AID_OABG_MAX_VALUE – Maximalwert |
| `VARIABLE_REF` | USINT | 11 | AID_OABG_VARIABLE_REF – Objekt-ID eines Variable-Objekts |
| `TARGET_VAL_VAR_REF` | USINT | 12 | AID_OABG_TARGET_VAL_VAR_REF – Referenz auf die Zielwertvariable |
| `TARGET_VALUE` | USINT | 13 | AID_OABG_TARGET_VALUE – Zielwert |
| `VALUE` | USINT | 14 | AID_OABG_VALUE – aktueller Wert |

### **Adapter**
Nicht vorhanden.

## Funktionsweise

Der Baustein definiert ausschließlich Konstanten, die als numerische Kennungen für die Attribute eines ISOBUS-Objekts vom Typ „Output arched bar graph“ dienen. Er besitzt keine eigene Logik oder Verarbeitungsfunktion. Er stellt die Attribut-IDs bereit, die von anderen Funktionsbausteinen verwendet werden, um auf die entsprechenden Attribute zuzugreifen, z. B. beim Setzen oder Lesen von Objekteigenschaften über die ISOBUS-Kommunikation.

## Technische Besonderheiten

- Die Konstanten sind als `USINT` (Unsigned Short Integer) definiert und mit festen Werten belegt.
- Sie sind im Paket `isobus::UT::Q::const::AID` zusammengefasst.
- Die Bitmasken für `OPTIONS` sind im Kommentar dokumentiert.
- Es handelt sich um einen GlobalConstants-Baustein, d. h. die Konstanten sind global verfügbar und nicht an eine Instanz gebunden.

## Zustandsübersicht

Nicht zutreffend, da es sich um einen reinen Konstanten-Baustein ohne Zustandslogik handelt.

## Anwendungsszenarien

- Verwendung in ISOBUS-Applikationen, um ein bogenförmiges Balkendiagramm auf einem Terminal zu konfigurieren.
- Zugriff auf Attribute wie Breite, Höhe, Farben, Winkel, Minimal-/Maximalwert und Zielwert.
- Einbindung in Funktionsbausteine, die Objektattribute über die ISOBUS-Schnittstelle setzen oder abfragen.

## Vergleich mit ähnlichen Bausteinen

Es gibt ähnliche GlobalConstants-Bausteine für andere ISOBUS-Objekttypen, z. B. `AID_OP` für Output-Objekte oder `AID_OV` für Variablen. Diese folgen dem gleichen Muster: Sie definieren Konstanten für Attribut-IDs, um eine einheitliche und typsichere Referenzierung zu ermöglichen. `AID_OABG` ist speziell auf das bogenförmige Balkendiagramm zugeschnitten.

## Fazit

`AID_OABG` ist ein einfacher, aber wichtiger GlobalConstants-Baustein, der die Attribut-IDs für ein spezielles ISOBUS-Anzeigeelement bereitstellt. Er erleichtert die Entwicklung von ISOBUS-Anwendungen, indem er symbolische Namen für die numerischen Kennungen liefert und so die Lesbarkeit und Wartbarkeit des Codes verbessert.