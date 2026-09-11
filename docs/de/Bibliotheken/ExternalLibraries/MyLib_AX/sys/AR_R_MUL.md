# AR_R_MUL


![AR_R_MUL_network](./AR_R_MUL_network.svg)

![AR_R_MUL](./AR_R_MUL.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock **AR_R_MUL** ist eine Subapplikation zur Multiplikation eines analogen Rohwertes (`AR`-Adapter) mit einem festen, bei der Instanziierung vorgegebenen REAL-Faktor. Er wurde entwickelt, um eine Skalierung von Analogwerten (z. B. Umrechnung von Rohwerten in physikalische Einheiten) ohne zusätzliche Verdrahtung eines zweiten variablen AR-Sockets zu ermöglichen. Der Faktor wird als Konfigurationsparameter in die Subapp eingebettet und bleibt während der Laufzeit konstant.

Die Subapp verwendet intern zwei Standard-Funktionsblöcke: einen `initval_AR`-Baustein, der den Faktor als konstanten AR-Wert erzeugt, und einen `AR_MUL_2`-Baustein, der die Multiplikation zweier AR-Werte ausführt. Somit wird eine einfache und effiziente Lösung für fixe Skalierungen bereitgestellt.

## Schnittstellenstruktur

### **Ereignis-Eingänge**
Keine.

### **Ereignis-Ausgänge**
Keine.

### **Daten-Eingänge**
- **FACTOR** (`REAL`): Der feste Multiplikationsfaktor. Er wird als Parameter bei der Instanziierung gesetzt und intern als Initialisierungswert für den `initval_AR`-Baustein verwendet. Standardwert: `REAL#1.0`.

### **Daten-Ausgänge**
Keine.

### **Adapter**
- **IN1** (`adapter::types::unidirectional::AR`, Socket): Der zu skalierende analoge Rohwert.
- **OUT** (`adapter::types::unidirectional::AR`, Plug): Das Ergebnis der Multiplikation (`IN1 * FACTOR`).

## Funktionsweise

Die Subapp besitzt keinen internen Ereignis- oder Zustandsablauf; sie arbeitet rein datenflussorientiert. Beim Instanziieren wird der Wert des Daten-Eingangs `FACTOR` an den Initialisierungs-Eingang `INIT_VAL` des internen `initval_AR`-Bausteins übergeben. Dieser erzeugt einen konstanten AR-Wert mit dem übergebenen REAL-Faktor. Der Adapter-Eingang `IN1` (der Rohwert) wird direkt an den ersten Eingang `IN1` des Multiplizierers `AR_MUL_2` geführt. Der zweite Eingang `IN2` des Multiplizierers wird mit dem konstanten AR-Wert vom `initval_AR` gespeist. Das Ergebnis der Multiplikation wird am Ausgang `AR_MUL_2.OUT` abgegriffen und über den Adapter `OUT` nach außen geführt.

Die Verschaltung gewährleistet, dass immer `OUT = IN1 * FACTOR` gilt, wobei `FACTOR` fest in der Subapp-Konfiguration verankert ist.

## Technische Besonderheiten

- **Feste Skalierung ohne zweites AR-Socket:** Im Gegensatz zu einem allgemeinen `AR_MUL` mit zwei variablen AR-Eingängen benötigt dieser Baustein nur einen AR-Eingang. Der zweite Faktor wird über einen REAL-Parameter gesetzt, wodurch die Verdrahtung im Gesamtsystem vereinfacht wird.
- **Verwendung von `initval_AR`:** Der interne `initval_AR`-Baustein wandelt den REAL-Wert in einen permanenten AR-Wert um, der als Multiplikator für den Multiplizierer dient.
- **Keine Laufzeitänderung des Faktors:** Der Multiplikationsfaktor kann während des Betriebs nicht dynamisch verändert werden; er ist bereits bei der Konfiguration der Instanz festgelegt.
- **Kompatibel mit IEC 61499-2:** Die Subapp folgt dem Standard für verteilte Automatisierungssysteme und ist in der 4diac-IDE einbindbar.

## Zustandsübersicht

Da die Subapp keine ereignisgesteuerten Transitionen oder internen Zustände besitzt, existiert keine explizite Zustandsmaschine. Die Ausgangswerte hängen ausschließlich von den aktuellen Eingangswerten ab (kombinatorische Logik). Somit kann der Funktionsblock als **zustandslos** betrachtet werden.

## Anwendungsszenarien

- **Skalierung von Analogsignalen:** Ein 4–20 mA-Signal wird in einen physikalischen Wert (z. B. Temperatur in °C) umgerechnet, indem ein konstanter Umrechnungsfaktor angewendet wird.
- **Einheitenumrechnung:** Umwandlung von Rohwerten in normierte Größen (z. B. von mV in V) mit festem Faktor.
- **Kalibrierung:** Kompensation einer bekannten, konstanten Verstärkungsabweichung eines Sensors.
- **Parameterierung in Fertigungsanlagen:** Festlegung einer produktspezifischen Skalierung, die bei der Projektierung einmalig konfiguriert wird.

## Vergleich mit ähnlichen Bausteinen

- **`AR_MUL` (Standard-Baustein):** Dieser besitzt zwei variable AR-Eingänge und erlaubt die Multiplikation zweier dynamischer analoger Werte. `AR_R_MUL` reduziert dies auf einen AR-Eingang und einen festen REAL-Faktor, was die Verdrahtung vereinfacht, aber die Flexibilität einschränkt.
- **`AR_SCALE` (falls vorhanden):** Ein Baustein, der oft eine Skalierung mit Offset und Bereichsumrechnung vornimmt; `AR_R_MUL` fokussiert sich auf eine reine Multiplikation ohne Offset.
- **Direkte Verwendung von `AR_MUL_2` mit konstantem Wert:** Dies erfordert eine zusätzliche Verdrahtung eines konstanten AR-Werts (z. B. über einen `initval_AR`), was der Benutzer manuell vornehmen müsste. Die Subapp kapselt diese Funktionalität und macht sie als eigenständigen Baustein verfügbar.

## Fazit

Der Funktionsblock **AR_R_MUL** stellt eine kompakte und wiederverwendbare Lösung für feste Multiplikationen von analogen Rohwerten dar. Durch die Integration des konstanten Faktors in die Instanzparameter entfällt die Notwendigkeit eines zweiten adaptiven AR-Sockets, was die Projektierung vereinfacht und die Übersichtlichkeit erhöht. Die Verwendung standardisierter Bausteine im Inneren gewährleistet eine robuste und nach IEC 61499 konforme Funktionsweise. Damit eignet sich der Baustein hervorragend für alle Anwendungen, bei denen eine einmalige, unveränderliche Skalierung von Analogwerten benötigt wird.