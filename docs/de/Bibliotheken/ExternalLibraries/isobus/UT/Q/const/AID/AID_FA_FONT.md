# AID_FA_FONT

![AID_FA_FONT](./AID_FA_FONT.svg)

* * * * * * * * * *

## Einleitung

Der `AID_FA_FONT`-GlobalConstants-Block definiert die Attribut-IDs (Adressen) für das **Font-Attribut-Objekt** im ISOBUS‑Protokoll. Diese Konstanten werden verwendet, um Eigenschaften wie Farbe, Größe, Schrifttyp und Stil eines Textes in der virtuellen Benutzeroberfläche (VT) zu spezifizieren. Der Block stellt eine referenzierbare, zentrale Sammlung dieser IDs bereit und erleichtert so die Konsistenz in der Applikationsentwicklung.

## Schnittstellenstruktur

Der `AID_FA_FONT`-Baustein besitzt **keine** Ereignis‑ oder Dateneingänge/-ausgänge und auch keine Adapter. Es handelt sich um einen globalen Konstantenblock, der über eine Sammlung von `USINT`-Konstanten die Attribut‑IDs für das Font‑Objekt definiert.

| Konstantenname | Typ | Wert | Beschreibung |
|----------------|-----|------|--------------|
| `COLOUR`       | `USINT` | `1` | Attribut‑ID für den Farbindex des Fonts. |
| `SIZE`         | `USINT` | `2` | Attribut‑ID für die Schriftgröße. |
| `FONT_TYPE`    | `USINT` | `3` | Attribut‑ID für den Schrifttyp (Font‑Familie). |
| `STYLE`        | `USINT` | `4` | Attribut‑ID für den Textstil (Bitmaske). |

### **Ereignis-Eingänge**

Keine vorhanden.

### **Ereignis-Ausgänge**

Keine vorhanden.

### **Daten-Eingänge**

Keine vorhanden.

### **Daten-Ausgänge**

Keine vorhanden.

### **Adapter**

Keine vorhanden.

## Funktionsweise

Die Konstanten werden als ganzzahlige Werte deklariert und dienen als Schlüsselwerte, die in ISOBUS‑Nachrichten verwendet werden, um auf bestimmte Felder des Font‑Attribut‑Objekts zuzugreifen. Jeder Wert entspricht einer fest definierten Attribut‑ID der ISO‑11783‑Norm. Bei der Kommunikation mit einem Virtual Terminal werden diese IDs verwendet, um die jeweilige Eigenschaft des Fonts zu setzen oder abzufragen. Der Wert `STYLE` ist dabei als Bitmaske codiert:

- Bit 0: **Bold** (fett)
- Bit 1: **Crossed Out** (durchgestrichen)
- Bit 2: **Underlined** (unterstrichen)
- Bit 3: **Italic** (kursiv)
- Bit 4: **Inverted** (invertierte Darstellung)
- Bit 5: **Flashing inverted** (blinkend invertiert)
- Bit 6: **Flashing hidden** (blinkend unsichtbar)
- Bit 7: **Proportional rendering** (proportionale Darstellung)

Die übrigen Konstanten sind einfache Indizes.

## Technische Besonderheiten

- Der Baustein ist als `GlobalConstants` deklariert, d.h. die Konstanten sind global verfügbar und können in mehreren Funktionsblöcken verwendet werden.
- Die Werte sind als `USINT` (8‑Bit) definiert und mit dem Präfix `USINT#` initialisiert.
- Die Bitmaske für `STYLE` erlaubt die Kombination mehrerer Stile über logische ODER‑Verknüpfung.
- Das Objekt gehört zur Package `isobus::UT::Q::const::AID` und wird im Compiler-Paket `isobus::UT::Q::const::AID` referenziert.

## Zustandsübersicht

Entfällt – es handelt sich um einen statischen Konstantenblock ohne Zustandsmaschine. Die Werte sind während der gesamten Laufzeit konstant.

## Anwendungsszenarien

- **ISOBUS‑Applikationen:** Verwendung in Steuerungssystemen, die mit einem Virtual Terminal (VT) kommunizieren und dort Texte mit unterschiedlichen Formatierungen darstellen müssen.
- **Entwicklung von ISOBUS‑Funktionsblöcken:** In FB‑Implementierungen, die ein Font‑Attribut‑Objekt konfigurieren, können die Konstanten direkt referenziert werden, um die korrekte Attribut‑ID zu verwenden.
- **Test und Simulation:** Zur Implementierung von Testtreibern, die die Funktionalität des VT verifizieren.

## Vergleich mit ähnlichen Bausteinen

Es existieren analoge GlobalConstants‑Blöcke für andere Attribut‑Objekte im ISOBUS‑Standard, z.B. `AID_FA_LINE`, `AID_FA_OUTLINE` oder `AID_FA_FILL`. Diese folgen dem gleichen Muster: Sie definieren Konstanten für Attribut‑IDs der jeweiligen Objektkategorie. Der vorliegende Block spezifiziert speziell die Eigenschaften des Schriftattributs und ist damit ein wichtiger Baustein für alle textbasierten Elemente einer virtuellen Benutzeroberfläche.

## Fazit

Der `AID_FA_FONT`‑GlobalConstants‑Block stellt eine essenzielle Konstantensammlung für die Arbeit mit Schriftformatierungen in ISOBUS‑Systemen bereit. Durch die klare Benennung der Attribute und die Kommentierung wird die Fehleranfälligkeit in der Applikationsentwicklung reduziert und die Lesbarkeit des Codes erhöht. Als zentrale Definitionsquelle trägt er zur Standardisierung und Wartbarkeit von ISOBUS‑Projekten bei.
