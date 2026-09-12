# AS2

![AS2](./AS2.svg)

* * * * * * * * * *

## Einleitung

Der Adapter-Typ **AS2** stellt eine bidirektionale Schnittstelle zwischen zwei Funktionsblöcken bereit. Er überträgt genau ein Ereignis zusammen mit einem SINT-Wert in beide Richtungen. Die Bezeichnung 'AS2' steht für 'Adapter SINT 2-Wege'. Der Baustein ist als generischer Steckverbinder (Plug/Socket) innerhalb der 4diac-IDE konzipiert und ermöglicht eine saubere, typsichere Kopplung von Komponenten.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Name | Typ   | Kommentar               | mit Daten |
| ---- | ----- | ----------------------- | --------- |
| EI1  | Event | Request (or Indication) | DI1       |

### **Ereignis-Ausgänge**

| Name | Typ   | Kommentar               | mit Daten |
| ---- | ----- | ----------------------- | --------- |
| EO1  | Event | Indication (or Request) | DO1       |

### **Daten-Eingänge**

| Name | Datentyp | Kommentar                         |
| ---- | -------- | --------------------------------- |
| DI1  | SINT    | Request (or Indication) to Socket |

### **Daten-Ausgänge**

| Name | Datentyp | Kommentar                              |
| ---- | -------- | -------------------------------------- |
| DO1  | SINT    | Indication (or Request) Data from Plug |

### **Adapter**

Keine definiert.

## Funktionsweise

Der AS2-Adapter arbeitet ereignisgesteuert und bidirektional:

- Wird der Ereigniseingang **EI1** ausgelöst, so wird der aktuell an **DI1** anliegende SINT-Wert zum gegenüberliegenden Adapterende übertragen. Dort erscheint das Ereignis **EO1** zusammen mit dem Wert auf **DO1**.
- In der Gegenrichtung gilt das Gleiche: Triggert der verbundene Gegenadapter sein Ereignis, so empfängt dieser AS2 das Ereignis an **EI1** und stellt den empfangenen Wert an **DI1** bereit.

Die Kommentare 'Request (or Indication)' und 'Indication (or Request)' verdeutlichen, dass der Adapter je nach Einbausituation (Plug oder Socket) die Rolle eines anfordernden oder eines anzeigenden Kanals einnehmen kann.

## Technische Besonderheiten

- **Typisierte Datenübertragung:** Es wird ausschließlich der Datentyp **SINT** (vorzeichenbehaftete 8-Bit-Ganzzahl) unterstützt.
- **Bidirektionalität:** Ein einziger Adapter realisiert den Datenaustausch in beide Richtungen.
- **Compiler-Information:** Das Paket ist unter `adapter::types::bidirectional` abgelegt.

## Fazit

Der AS2-Adapter ist ein eleganter Baustein für die bidirektionale Übertragung eines SINT-Werts mit zugehörigem Ereignis.
