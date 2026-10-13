# AIS2_S_TO_STRING

![AIS2_S_TO_STRING_network](./AIS2_S_TO_STRING_network.svg)

* * * * * * * * * *

## Einleitung

`AIS2_S_TO_STRING` ist ein Composite-Funktionsblock, der den bidirektionalen Adapter [AIS2](../../../types/bidirectional/STRING/AIS2.md) auf einfache Ereignis- und STRING-Datenports abbildet. Er gehört zur Konvertierungsfamilie analog zu AR2/AB2/AE2.

![AIS2_S_TO_STRING](AIS2_S_TO_STRING.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **REQ** (Request to Adapter): Sendet die Daten `OUT` über den Adapter (assoziiert mit `OUT`)

### **Ereignis-Ausgänge**

- **CNF** (Confirmation from Adapter): Wird ausgelöst, wenn der Adapter ein Ereignis auf seiner eingehenden Seite liefert (assoziiert mit `IN`)

### **Daten-Eingänge**

- **OUT** (STRING): Daten, die an den Adapter gesendet werden

### **Daten-Ausgänge**

- **IN** (STRING): Vom Adapter empfangene Daten

### **Adapter**

- **AIS2_IN** (Socket): Adapter vom Typ `adapter::types::bidirectional::AIS2`

## Funktionsweise

Der Baustein besteht nur aus Verdrahtung ohne eigene Logik:

- `REQ` → `AIS2_IN.EI1` und `OUT` → `AIS2_IN.DI1`: Der Anfrage-Ereignisstrom samt Daten wird auf die ausgehende Seite des Adapters veröffentlicht.
- `AIS2_IN.EO1` → `CNF` und `AIS2_IN.DO1` → `IN`: Was auf der eingehenden Seite des Adapters gemeldet wird, erscheint als Ereignis `CNF` mit den Daten `IN`.

## Technische Besonderheiten

- Zustandsloser Composite-Baustein ohne Algorithmen
- Jede Zielvariable hat genau einen Schreiber
- Die Adapterdaten werden unverändert durchgereicht, es findet keine Wertumwandlung statt

## Anwendungsszenarien

- Anbindung klassischer Ereignis-/Datenports an einen bidirektionalen AIS2-Adapter
- Einbindung von Bausteinen mit STRING-Ports in adapterbasierte Netzwerke

## Fazit

`AIS2_S_TO_STRING` setzt den bidirektionalen AIS2-Adapter ohne Logik auf die Ports `REQ`/`OUT` und `CNF`/`IN` um.
