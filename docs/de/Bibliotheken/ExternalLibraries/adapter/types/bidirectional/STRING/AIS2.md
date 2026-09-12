# AIS2

![AIS2](./AIS2.svg)

* * * * * * * * * *

## Einleitung

Der Adapter-Typ **AIS2** stellt eine bidirektionale Schnittstelle zwischen zwei Funktionsblöcken bereit. Er überträgt genau ein Ereignis zusammen mit einem STRING-Wert in beide Richtungen. Die Bezeichnung 'AIS2' steht für 'Adapter STRING 2-Wege'. Der Baustein ist als generischer Steckverbinder (Plug/Socket) innerhalb der 4diac-IDE konzipiert und ermöglicht eine saubere, typsichere Kopplung von Komponenten.

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
| DI1  | STRING    | Request (or Indication) to Socket |

### **Daten-Ausgänge**

| Name | Datentyp | Kommentar                              |
| ---- | -------- | -------------------------------------- |
| DO1  | STRING    | Indication (or Request) Data from Plug |

### **Adapter**

Keine definiert.

## Funktionsweise

Der AIS2-Adapter arbeitet ereignisgesteuert und bidirektional:

- Wird der Ereigniseingang **EI1** ausgelöst, so wird der aktuell an **DI1** anliegende STRING-Wert zum gegenüberliegenden Adapterende übertragen. Dort erscheint das Ereignis **EO1** zusammen mit dem Wert auf **DO1**.
- In der Gegenrichtung gilt das Gleiche: Triggert der verbundene Gegenadapter sein Ereignis, so empfängt dieser AIS2 das Ereignis an **EI1** und stellt den empfangenen Wert an **DI1** bereit.

Die Kommentare 'Request (or Indication)' und 'Indication (or Request)' verdeutlichen, dass der Adapter je nach Einbausituation (Plug oder Socket) die Rolle eines anfordernden oder eines anzeigenden Kanals einnehmen kann.

## Technische Besonderheiten

- **Typisierte Datenübertragung:** Es wird ausschließlich der Datentyp **STRING** (Zeichenkette) unterstützt.
- **Bidirektionalität:** Ein einziger Adapter realisiert den Datenaustausch in beide Richtungen.
- **Compiler-Information:** Das Paket ist unter `adapter::types::bidirectional` abgelegt.

## Fazit

Der AIS2-Adapter ist ein eleganter Baustein für die bidirektionale Übertragung eines STRING-Werts mit zugehörigem Ereignis.
