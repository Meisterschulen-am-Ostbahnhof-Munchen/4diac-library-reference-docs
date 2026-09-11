# AID_OLA

![AID_OLA](./AID_OLA.svg)

* * * * * * * * * *
## Einleitung

Der GlobalConstants-Baustein **AID_OLA** definiert die Attribut-IDs (Attribute Identifiers) für ein Output List Object (OLA) im ISOBUS-Protokoll. Diese Konstanten sind integraler Bestandteil der Kommunikation zwischen landwirtschaftlichen Maschinen und Geräten, wobei sie die spezifischen Attribute eines Ausgabelisten-Objekts identifizieren. Jede Konstante repräsentiert eine feste numerische ID, die im Rahmen der ISOBUS-Datenübertragung verwendet wird, um auf das jeweilige Attribut des Objekts zu referenzieren.

## Schnittstellenstruktur

Da es sich bei **AID_OLA** um eine GlobalConstants-Definition handelt, besitzt der Baustein keine klassische Schnittstelle mit Ereignis- oder Datenein-/ausgängen. Stattdessen werden vier Konstanten bereitgestellt, die als eindeutige Identifikatoren für die Attribute des Output List Objects dienen. Diese Konstanten stehen global zur Verfügung und können in anderen Bausteinen direkt referenziert werden.

### **Ereignis-Eingänge**
- Keine

### **Ereignis-Ausgänge**
- Keine

### **Daten-Eingänge**
- Keine (Die Konstanten sind implizit als globale Definitionen verfügbar.)

### **Daten-Ausgänge**
- Keine (Die Konstanten sind als feste Werte in den Baustein integriert und werden nicht über eine Schnittstelle ausgegeben.)

### **Adapter**
- Keine

**Hinweis:** Die vier definierten Konstanten sind:

| Konstante     | Wert | Beschreibung                                                                 |
|---------------|------|------------------------------------------------------------------------------|
| `WIDTH`       | 1    | Breite des Objekts in Pixeln (`AID_OLA_WIDTH`)                               |
| `HEIGHT`      | 2    | Höhe des Objekts in Pixeln (`AID_OLA_HEIGHT`)                                |
| `VARIABLE_REF`| 3    | Objekt-ID einer Zahlenvariablen (`AID_OLA_VARIABLE_REF`)                     |
| `VALUE`       | 4    | Gewählter Listenindex (0 bis 254) oder 255, wenn kein Element ausgewählt (`AID_OLA_VALUE`) |

## Funktionsweise

Der Baustein stellt die Attribut-IDs als Konstanten bereit, die in der ISOBUS-Kommunikation für das Output List Object verwendet werden. Ein Output List Object ist ein Element der virtuellen Endgeräte (VT), das eine Liste von Auswahlmöglichkeiten (z.B. Menüpunkte) darstellt. Die Konstanten dienen dazu, die Attribute dieses Objekts (Breite, Höhe, referenzierte Variable und aktueller Wert) eindeutig zu identifizieren, wenn sie über das ISOBUS-Protokoll angesprochen werden. Durch die Verwendung dieser Konstanten wird eine konsistente und fehlerfreie Zuordnung zwischen den physischen Objekten und den übertragenen Daten sichergestellt.

## Technische Besonderheiten

- Die Konstanten sind als `USINT` (Unsigned Short Integer) deklariert, was eine kompakte und effiziente Darstellung im ISOBUS-Rahmen ermöglicht.
- Die Initialwerte entsprechen den definierten IDs gemäß ISO 11783-6 (ISOBUS – Virtual Terminal).
- Der Baustein enthält einen Copyright-Hinweis gemäß Eclipse Public License 2.0 (EPL-2.0).
- Er ist in der `packageName`-Struktur `isobus::UT::Q::const::AID` organisiert, was eine klare Modulstruktur fördert.
- Die Werte sind als Konstanten im globalen Kontext definiert und können ohne weitere Instanziierung in anderen Bausteinen verwendet werden.

## Zustandsübersicht

Da **AID_OLA** ausschließlich Konstanten definiert und keine ausführbare Logik enthält, existiert kein Zustandsmodell. Der Baustein hat keine transitions- oder zustandsbasierten Abläufe.

## Anwendungsszenarien

- **Erstellung eines VT (Virtual Terminal) – Objekts:** Bei der Definition eines neuen Output List Object in einem ISOBUS-Terminal können die Konstanten verwendet werden, um Attribute wie Breite und Höhe festzulegen.
- **Referenzierung von Variablen:** Mittels `VARIABLE_REF` kann eine Verknüpfung zu einer vorhandenen Zahlenvariablen hergestellt werden, deren Wert dann als aktueller Listenindex (`VALUE`) dargestellt wird.
- **Programmierung von Bedienelementen:** In Steuerungsanwendungen, die auf ISOBUS-Terminals zugreifen, können diese Konstanten helfen, Eingaben (z.B. Auswahl eines Menüpunkts) korrekt zu interpretieren.
- **Debugging und Tests:** Durch die klar definierten IDs wird die Fehleranalyse bei Kommunikationsproblemen mit dem VT vereinfacht.

## Vergleich mit ähnlichen Bausteinen

In der ISOBUS-Umgebung gibt es für jedes Objekt (z.B. Number Variable, Output String, List Object) eigene GlobalConstants-Bausteine, die die jeweiligen Attribut-IDs definieren. Beispiele sind `AID_NVS` (Number Variable Selection) oder `AID_OS` (Output String). Diese Bausteine folgen demselben Muster: Sie stellen numerische Konstanten für die Attribute des jeweiligen Objekttyps bereit. Der Aufbau ist stets ähnlich, unterscheidet sich jedoch in der Anzahl und Bedeutung der Konstanten. **AID_OLA** ist speziell auf die Anforderungen eines Output List Objects zugeschnitten.

## Fazit

**AID_OLA** ist ein bedeutender Baustein für die ISOBUS-Kommunikation, der eine saubere und standardisierte Definition der Attribut-IDs für Output List Objects bereitstellt. Durch die klare Struktur und die einfache Integration in andere Bausteine erleichtert er die Entwicklung von Anwendungen für landwirtschaftliche Terminals und trägt zur Interoperabilität zwischen verschiedenen Herstellern bei. Seine Verwendung reduziert potenzielle Fehlerquellen und erhöht die Wartbarkeit des Gesamtsystems.