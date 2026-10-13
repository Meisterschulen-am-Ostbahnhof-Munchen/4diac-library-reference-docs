# ALR2_TO_ALR

![ALR2_TO_ALR_network](./ALR2_TO_ALR_network.svg)

* * * * * * * * * *

## Einleitung

`ALR2_TO_ALR` ist ein Composite-Funktionsblock, der einen bidirektionalen [ALR2](../../../types/bidirectional/REAL/ALR2.md)-Abzweig mit unidirektionalen [ALR](../../../types/unidirectional/LREAL/ALR.md)-Adaptern (LREAL) verbindet: Die eingehende Seite von `ALR2_BIDIR` wird als plain unidirektionales Signal über `ALR_OUT` ausgegeben, und `ALR_IN` wird auf die ausgehende Seite von `ALR2_BIDIR` veröffentlicht.

![ALR2_TO_ALR](ALR2_TO_ALR.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Der Funktionsblock verfügt über keine direkten Ereignis-Eingänge – die Kommunikation läuft ausschließlich über die Adapter.

### **Ereignis-Ausgänge**

Der Funktionsblock verfügt über keine direkten Ereignis-Ausgänge.

### **Daten-Eingänge**

Der Funktionsblock verfügt über keine direkten Daten-Eingänge.

### **Daten-Ausgänge**

Der Funktionsblock verfügt über keine direkten Daten-Ausgänge.

### **Adapter**

- **ALR2_BIDIR** (Socket): bidirektionaler Abzweig, dessen eingehende Seite an `ALR_OUT` weitergereicht wird und dessen ausgehende Seite von `ALR_IN` gespeist wird, Typ `adapter::types::bidirectional::ALR2`
- **ALR_IN** (Socket): unidirektionale Quelle, Typ `adapter::types::unidirectional::ALR`
- **ALR_OUT** (Plug): spiegelt die eingehende (Indikations-)Seite von `ALR2_BIDIR`, Typ `adapter::types::unidirectional::ALR`

## Funktionsweise

Der Baustein besteht nur aus vier Verbindungen:

- Ereignis: `ALR2_BIDIR.EO1` → `ALR_OUT.E1`; Daten: `ALR2_BIDIR.DO1` → `ALR_OUT.D1`
- Ereignis: `ALR_IN.E1` → `ALR2_BIDIR.EI1`; Daten: `ALR_IN.D1` → `ALR2_BIDIR.DI1`

Was ein an `ALR2_BIDIR` angeschlossener bidirektionaler Teilnehmer meldet, erscheint also unverändert an `ALR_OUT`; was an `ALR_IN` eintrifft, wird an den bidirektionalen Teilnehmer weitergegeben.

## Technische Besonderheiten

- Reine Verdrahtung ohne Logik oder Zustand
- Jede Zielvariable hat genau einen Schreiber
- Ohne angeschlossenen bidirektionalen Teilnehmer bleibt `ALR_OUT` inaktiv

## Anwendungsszenarien

- Auslesen eines bidirektionalen ALR2-Teilnehmers als einfaches unidirektionales ALR-Signal bei gleichzeitigem Einspeisen eines separaten unidirektionalen Signals
- Brücke zwischen einer bidirektionalen ALR2-Zone und einer unidirektionalen ALR-Zone

## Fazit

`ALR2_TO_ALR` ist ein Abzweigpunkt ohne Logik: Er macht die eingehende Seite eines bidirektionalen ALR2-Teilnehmers als unidirektionales ALR-Signal sichtbar und speist gleichzeitig ein unidirektionales Signal in die ausgehende Seite ein.
