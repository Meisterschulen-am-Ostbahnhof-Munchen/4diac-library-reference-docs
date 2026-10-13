# AIS2_TO_AIS

![AIS2_TO_AIS_network](./AIS2_TO_AIS_network.svg)

* * * * * * * * * *

## Einleitung

`AIS2_TO_AIS` ist ein Composite-Funktionsblock zur Konvertierung eines **bidirektionalen** `AIS2`-Adapters in einen **unidirektionalen** `AIS`-Adapter. Die eingehende Seite (`EO1`/`DO1`) des bidirektionalen Adapters wird als einfaches unidirektionales Signal über `AIS_OUT` ausgegeben; über `AIS_IN` wird ein Wert auf die ausgehende Seite (`EI1`/`DI1`) des bidirektionalen Adapters geschrieben.

![AIS2_TO_AIS](AIS2_TO_AIS.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

*Keine direkten Ereignis-Eingänge vorhanden – Ereignisse kommen über die Adapter-Sockets/-Plugs*

### **Ereignis-Ausgänge**

*Keine direkten Ereignis-Ausgänge vorhanden*

### **Daten-Eingänge**

*Keine Daten-Eingänge vorhanden*

### **Daten-Ausgänge**

*Keine Daten-Ausgänge vorhanden*

### **Adapter**

- **AIS2_BIDIR** (Socket): Bidirektionaler Adapter vom Typ `adapter::types::bidirectional::AIS2` – die eingehende Seite wird an `AIS_OUT` weitergeleitet, die ausgehende Seite wird von `AIS_IN` getrieben
- **AIS_IN** (Socket): Unidirektionaler Adapter vom Typ `adapter::types::unidirectional::AIS` – unidirektionale Quelle
- **AIS_OUT** (Plug): Unidirektionaler Adapter vom Typ `adapter::types::unidirectional::AIS` – spiegelt die eingehende (Indikations-)Seite von `AIS2_BIDIR`

## Funktionsweise

1. `AIS2_BIDIR.EO1` wird an `AIS_OUT.E1` und `AIS2_BIDIR.DO1` an `AIS_OUT.D1` weitergeleitet.
2. `AIS_IN.E1` wird an `AIS2_BIDIR.EI1` und `AIS_IN.D1` an `AIS2_BIDIR.DI1` weitergeleitet.

## Technische Besonderheiten

- Reine Ereignis-/Datenverbindungen (`FBNetwork`), keine eigene Logik und kein interner Zustand
- Teil der Konvertierungsfamilie (analog zu `AR2`/`AB2`/`AE2`); der Datentyp der Daten ist `STRING`

## Anwendungsszenarien

- Anbindung eines unidirektionalen `AIS`-Verbrauchers bzw. -Erzeugers an einen bidirektionalen `AIS2`-Adapter

## Fazit

`AIS2_TO_AIS` trennt die beiden Richtungen eines bidirektionalen `AIS2`-Adapters in einen unidirektionalen Ausgang (`AIS_OUT`) und einen unidirektionalen Eingang (`AIS_IN`).
