# ALR_TO_ALR2

![ALR_TO_ALR2_network](./ALR_TO_ALR2_network.svg)

* * * * * * * * * *

## Einleitung

`ALR_TO_ALR2` ist ein Composite-Funktionsblock zur Konvertierung eines **unidirektionalen** `ALR`-Adapters in einen **bidirektionalen** `ALR2`-Adapter. `ALR_IN` wird auf die ausgehende Seite (`EO1`/`DO1`) von `ALR2_BIDIR` geschrieben; was `ALR2_BIDIR` auf seiner eingehenden Seite (`EI1`/`DI1`) empfängt, wird als einfaches unidirektionales Signal über `ALR_OUT` wieder ausgegeben.

![ALR_TO_ALR2](ALR_TO_ALR2.svg)

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

- **ALR_IN** (Socket): Unidirektionaler Adapter vom Typ `adapter::types::unidirectional::ALR` – unidirektionale Quelle
- **ALR2_BIDIR** (Plug): Bidirektionaler Adapter vom Typ `adapter::types::bidirectional::ALR2` – ausgehende Seite wird von `ALR_IN` getrieben, eingehende Seite wird an `ALR_OUT` weitergeleitet
- **ALR_OUT** (Plug): Unidirektionaler Adapter vom Typ `adapter::types::unidirectional::ALR` – spiegelt die eingehende (Request-)Seite von `ALR2_BIDIR`

## Funktionsweise

1. `ALR_IN.E1` wird an `ALR2_BIDIR.EO1` und `ALR_IN.D1` an `ALR2_BIDIR.DO1` weitergeleitet.
2. `ALR2_BIDIR.EI1` wird an `ALR_OUT.E1` und `ALR2_BIDIR.DI1` an `ALR_OUT.D1` weitergeleitet.

## Technische Besonderheiten

- Reine Ereignis-/Datenverbindungen (`FBNetwork`), keine eigene Logik und kein interner Zustand
- Teil der Konvertierungsfamilie (analog zu `AR2`/`AB2`/`AE2`); der Datentyp der Daten ist `LREAL`

## Anwendungsszenarien

- Anbindung unidirektionaler `ALR`-Quellen und -Senken an einen bidirektionalen `ALR2`-Adapter

## Fazit

`ALR_TO_ALR2` fasst einen unidirektionalen `ALR`-Eingang und -Ausgang zu einem bidirektionalen `ALR2`-Adapter zusammen.
