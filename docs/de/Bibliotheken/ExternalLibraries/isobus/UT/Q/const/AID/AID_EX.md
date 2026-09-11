# AID_EX

![AID_EX](./AID_EX.svg)

* * * * * * * * * *
## Einleitung

Der Baustein **AID_EX** ist ein Global-Constants-Objekt in der 4diac-IDE, das Konstanten für Attribut-IDs externer Objekte im ISOBUS-Kontext definiert. Diese Konstanten werden typischerweise in anderen Funktionsblöcken oder Anwendungen verwendet, um auf bestimmte Attribute eines externen Objekts zu verweisen. Der Baustein stellt eine zentrale, klar benannte Sammlung von Zahlenwerten als `USINT` (Unsigned Short Integer) bereit, was die Lesbarkeit und Wartbarkeit von Code verbessert, der auf diese IDs zugreift.

Die drei definierten Konstanten sind:
- **OPTIONS** (Wert 1) – eine Bitmaske, die Steueroptionen für referenzierte Objekte beschreibt.
- **NAME_0** (Wert 2) – die ID für das erste Namen-Attribut.
- **NAME_1** (Wert 3) – die ID für das zweite Namen-Attribut.

Diese Konstanten sind als globale Konstanten deklariert und können daher projektspezifisch oder systemweit referenziert werden.

## Schnittstellenstruktur

Da es sich um ein Global-Constants-Objekt und nicht um einen Funktionsblock handelt, besitzt der Baustein **keine ereignis- oder datenbasierten Ein-/Ausgänge** und auch keine Adapter. Die Komponente dient ausschließlich der Bereitstellung von Konstantenwerten.

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

Der Baustein definiert drei globale Konstanten, die als feste numerische Werte (`USINT`) zur Verfügung stehen. Diese Werte werden zur Laufzeit nicht verändert und dienen als symbolische Referenzen für Attribute im ISOBUS-Protokoll. Die Konstanten werden im Paket `isobus::UT::Q::const::AID` bereitgestellt und sind über die Compiler-Info diesem Paket zugeordnet.

Jede Konstante besitzt einen Kommentar, der ihre Bedeutung beschreibt:
- `OPTIONS` (`USINT#1`): Bitmaske, wobei Bit 0 angibt, ob eine referenzierte NAME-WS auf gelistete Objekte verweisen darf.
- `NAME_0` (`USINT#2`) und `NAME_1` (`USINT#3`): IDs für unterschiedliche Namensfelder.

Da es sich um Konstanten handelt, sind sie zur Compile-Zeit festgelegt und können in anderen Bausteinen über den qualifizierten Namen `AID_EX.OPTIONS`, `AID_EX.NAME_0` oder `AID_EX.NAME_1` referenziert werden.

## Technische Besonderheiten

- **Paketzuordnung**: Der Baustein ist im Paket `isobus::UT::Q::const::AID` enthalten, was eine organisierte und hierarchische Struktur für ISOBUS-bezogene Konstanten bietet.
- **CompilerInfo**: Die definierten Konstanten werden in das Paket `isobus::UT::Q::const::AID` kompiliert, sodass sie in den erzeugten C/C++-Code als Symbole eingebunden werden.
- **Lizenz**: Das Objekt wird unter der Eclipse Public License 2.0 bereitgestellt (SPDX-Lizenz-Identifier: EPL-2.0), was die Nutzung und Weiterverbreitung regelt.
- **Typ**: Alle Konstanten sind vom Typ `USINT` (8-Bit unsigned integer), was den Wertebereich 0–255 abdeckt und für Attribut-IDs ausreichend ist.
- **Initialisierung**: Die initialen Werte sind direkt als `USINT#1`, `USINT#2` und `USINT#3` gesetzt, sodass die Konstanten ohne weitere Initialisierung sofort verwendbar sind.

## Zustandsübersicht

Nicht anwendbar, da es sich um ein reines Konstanten-Objekt handelt und keine Zustandsautomaten oder Laufzeitlogik vorhanden sind.

## Anwendungsszenarien

- **ISOBUS-Kommunikation**: Referenzierung von Attribut-IDs in ISOBUS-Nachrichten (z. B. Objektattribute wie Namen).
- **Konfiguration von externen Objekten**: Verwenden von `AID_EX.OPTIONS`, um Berechtigungen oder Optionen für referenzierte Objekte zu setzen.
- **Code-Wartung**: Zentrale Definition von IDs verbessert die Lesbarkeit und vermeidet „magic numbers“ in der Anwendungslogik.
- **Integration in andere Funktionsblöcke**: Andere FB können diese Konstanten in ihren eigenen Berechnungen oder Konfigurationsdaten verwenden.

## Vergleich mit ähnlichen Bausteinen

Es gibt keine direkt vergleichbaren Funktionsblöcke, da `AID_EX` ein Global-Constants-Objekt ist. Ähnliche Konstanten-Bausteine könnten global definierte Werte wie Schwellwerte, Protokollkennungen oder andere Attribut-IDs enthalten. Im Gegensatz zu typischen Funktionsblöcken besitzt es keine Eingangs-/Ausgangslogik und keine Zustände. Es dient ausschließlich der Datenhaltung auf Konstanten-Ebene.

## Fazit

`AID_EX` ist ein kompakter und klar strukturierter Global-Constants-Baustein, der wichtige Attribut-IDs für ISOBUS-Externe-Objekte bereitstellt. Durch die symbolische Benennung wird die Verständlichkeit des Programms erhöht und die Fehleranfälligkeit reduziert. Die Verwendung von USINT-Typen und die Einbettung in ein Paket machen den Baustein effizient und in der 4diac-Umgebung integrierbar. Er eignet sich hervorragend als Basis für gemeinsame Konstanten in ISOBUS-basierten Projekten.