# AID_AM

![AID_AM](./AID_AM.svg)

* * * * * * * * * *

## Einleitung

Der Baustein `AID_AM` ist eine globale Konstantenliste, die die Attribut-IDs für ein **Alarm Mask**-Objekt im ISOBUS-Kontext definiert. Diese Konstanten werden verwendet, um eindeutige Kennungen für verschiedene Eigenschaften einer Alarmmaske (z. B. Hintergrundfarbe, Softkey-Maske, Alarmpriorität, akustisches Signal) zu referenzieren. Der Baustein ist als Teil des Pakets `isobus::UT::Q::const::AID` verfügbar und dient als wiederverwendbare Definitionssammlung für die Erstellung und Verwaltung von ISOBUS-basierten Alarmmasken in Steuerungssystemen.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Nicht vorhanden – es handelt sich um eine reine Konstantendefinition ohne prozessbezogene Eingaben.

### **Ereignis-Ausgänge**

Nicht vorhanden – es werden keine Ereignisse oder Signale ausgegeben.

### **Daten-Eingänge**

Nicht vorhanden – die Konstanten sind fest definiert und können zur Laufzeit nicht verändert werden.

### **Daten-Ausgänge**

Nicht vorhanden – die Werte sind als globale Konstanten für andere Bausteine direkt verfügbar, ohne dass ein Datenausgang des Bausteins verwendet werden muss.

### **Adapter**

Nicht vorhanden.

## Funktionsweise

`AID_AM` stellt vier symbolische Konstanten bereit, die jeweils eine ganzzahlige Kennung (Typ `USINT`) für ein Attribut eines ISOBUS-Alarmmasken-Objekts repräsentieren. Die Zuordnung ist wie folgt:

| Konstante               | Wert | Beschreibung                                                                 |
|-------------------------|------|------------------------------------------------------------------------------|
| `BACKGROUND_COLOUR`     | 1    | Index für die Hintergrundfarbe der Alarmmaske.                               |
| `SOFT_KEY_MASK`         | 2    | Objekt-ID einer Softkey-Maske, die mit der Alarmmaske verknüpft ist.         |
| `ALARM_PRIORITY`        | 3    | Prioritätsstufe des Alarms (0 = Hoch, 1 = Mittel, 2 = Niedrig).              |
| `ACOUSTIC`              | 4    | Akustisches Signalverhalten (0 = höchste Priorität, 1 = mittel, 2 = niedrig, 3 = still). |

Diese Konstanten werden typischerweise in anderen Funktionsbausteinen verwendet, um auf die entsprechenden Attribute eines Alarmmasken-Objekts im ISOBUS-Netzwerk zuzugreifen (z. B. beim Lesen oder Setzen von Objekteigenschaften über den ISOBUS-Dienst).

## Technische Besonderheiten

- **Datentyp:** Alle Konstanten sind als `USINT` (Unsigned Short Integer, 8 Bit) definiert.
- **Initialwerte:** Die Werte sind fest mit `USINT#1` bis `USINT#4` belegt und entsprechen den offiziellen ISOBUS-Attribut-IDs.
- **Paketierung:** Die Konstanten sind im Paket `isobus::UT::Q::const::AID` gekapselt, wodurch eine übersichtliche und wiederverwendbare Struktur innerhalb der 4diac-IDE ermöglicht wird.
- **Keine Laufzeitänderung:** Da es sich um `GLOBAL CONSTANT` handelt, sind die Werte während der gesamten Anwendungsdauer unveränderlich.

## Zustandsübersicht

Nicht zutreffend – der Baustein besitzt keinen internen Zustandsautomaten und keine Zustandsübergänge.

## Anwendungsszenarien

- **ISOBUS-Terminal-Programmierung:** Einsatz in Funktionen zur Konfiguration von Alarmmasken, z. B. beim Ändern der Hintergrundfarbe oder beim Zuordnen einer Softkey-Maske.
- **Alarmmanagement:** Verwendung zur Unterscheidung der Alarmprioritäten in der Steuerungslogik, um unterschiedliche Reaktionen oder Darstellungen auszulösen.
- **Akustische Signalisierung:** Steuerung des akustischen Verhaltens von Alarmen durch die entsprechende Attribut-ID.
- **Basis für weitere Konstanten:** Der Baustein kann als Vorlage für andere AID-Konstantenlisten (z. B. für Softkey-Masken oder Objekteigenschaften) dienen.

## Vergleich mit ähnlichen Bausteinen

Es existieren in ISOBUS-Implementierungen weitere AID-Konstanten für andere Objekttypen, z. B. `AID_SKM` für Softkey-Masken oder `AID_BGM` für Background-Masken. `AID_AM` ist spezifisch auf Alarmmasken zugeschnitten und enthält nur die hierfür relevanten Attribut-IDs. Andere Bausteine könnten mehr oder andere Konstanten enthalten, je nach den geforderten Objekteigenschaften. Im Gegensatz zu Funktionsbausteinen mit Ein-/Ausgängen bietet `AID_AM` keine prozessbezogene Logik, sondern dient ausschließlich als Konstantenquelle.

## Fazit

`AID_AM` ist ein einfacher, aber essenzieller Baustein für die Entwicklung von ISOBUS-konformen Anwendungen, die Alarmmasken verwenden. Durch die klare Definition der Attribut-IDs als globale Konstanten wird die Lesbarkeit und Wartbarkeit des Codes erhöht und die Gefahr von Tippfehlern minimiert. Die Konzentration auf eine spezifische Objektart macht den Baustein schlank und leicht einsetzbar. Für Entwickler, die ISOBUS-Terminals steuern, bietet `AID_AM` eine verlässliche Grundlage für die korrekte Adressierung von Alarmmasken-Eigenschaften.
