# AID_OLRL

![AID_OLRL](./AID_OLRL.svg)

* * * * * * * * * *
## Einleitung

Der Baustein `AID_OLRL` (Object Label Reference List Object Attribute IDs) ist eine globale Konstantendeklaration aus dem Paket `isobus::UT::Q::const::AID`. Er definiert eine Konstante, die die Anzahl der beschrifteten Objekte in einer ISO‑bus‑basierten Objektreferenzliste angibt. Diese Konstante wird im Rahmen des ISOBUS‑Protokolls verwendet, um die Struktur von Nachrichten zur Verwaltung von Objektattributen zu beschreiben.

## Schnittstellenstruktur

`AID_OLRL` besitzt keine Ein‑ oder Ausgänge im klassischen Sinne eines Funktionsblocks, da es sich um eine globale Konstantendefinition handelt. Die Struktur beschränkt sich auf die deklarierte Konstante.

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

Die globale Konstante `NUMB_LABELLED_OBJ` vom Typ `USINT` (Unsigned Short Integer) wird mit dem Wert `1` initialisiert. Sie gibt die Anzahl der beschrifteten Objekte an, die in einer Objektreferenzliste folgen. Im ISOBUS‑Kontext wird diese Konstante verwendet, um die Länge oder den Aufbau einer Datenstruktur zu kennzeichnen, die Referenzen auf Objekte mit Attributen (z. B. Beschriftungen) enthält.

Die Deklaration erfolgt als `VAR_GLOBAL CONSTANT` und ist damit innerhalb des gesamten Projekts unveränderlich verfügbar. Auf sie kann über den vollständigen Pfad `isobus::UT::Q::const::AID::AID_OLRL::NUMB_LABELLED_OBJ` zugegriffen werden.

## Technische Besonderheiten

- **Typisierung:** Die Konstante ist explizit als `USINT` typisiert, was eine sichere Verwendung in arithmetischen Operationen gewährleistet.
- **Initialisierung:** Der vorgegebene Wert `1` entspricht der üblichen Anzahl von Beschriftungsobjekten in einer Standard‑Objektreferenzliste.
- **Konstantheit:** Durch die Deklaration als `CONSTANT` ist der Wert zur Laufzeit unveränderlich, was die Robustheit des Systems erhöht.
- **Paketzuordnung:** Die Einbindung in das Paket `isobus::UT::Q::const::AID` stellt sicher, dass die Konstante im ISOBUS‑Namensraum eindeutig identifizierbar ist.

## Zustandsübersicht

Da es sich nicht um einen funktionalen Baustein handelt, existiert kein Zustandsdiagramm. Die Konstante hat stets den gleichen Wert und benötigt keine Zustandslogik.

## Anwendungsszenarien

- **ISOBUS-Nachrichtenaufbau:** In der Kommunikation zwischen Traktor und Anbaugerät kann die Konstante verwendet werden, um die Anzahl der Objektreferenzen in einer Nachricht zu definieren.
- **Datenstrukturdefinition:** Sie dient als Grundlage für die Erstellung von Datenstrukturen, die eine variable Anzahl von Objektbeschriftungen enthalten.
- **Konfiguration:** Entwickler können sie nutzen, um die maximale oder erwartete Anzahl von beschrifteten Objekten in einem System festzulegen.

## Vergleich mit ähnlichen Bausteinen

Im ISOBUS‑Standard existieren weitere Konstantenbausteine, die ähnliche Attribute ID‑Listen definieren (z. B. `AID_OL`, `AID_OLRL` usw.). Diese unterscheiden sich hauptsächlich in der Art der referenzierten Objekte und der spezifischen Bedeutung ihrer Konstanten. `AID_OLRL` fokussiert auf die Objekt‑Referenzliste, während andere Bausteine andere Aspekte (z. B. einzelne Objekte oder deren Attribute) abdecken.

## Fazit

`AID_OLRL` ist ein einfacher, aber wichtiger Konstantenbaustein für die ISOBUS‑Kommunikation. Er stellt eine standardisierte Zahl bereit, die die Struktur von Objektlisten beschreibt. Durch seine klare Typisierung und Konstantheit trägt er zur Wiederverwendbarkeit und Sicherheit in 4diac‑Projekten bei. Für Anwender, die ISOBUS‑basierten Systeme entwickeln, stellt er eine nützliche Ressource dar.