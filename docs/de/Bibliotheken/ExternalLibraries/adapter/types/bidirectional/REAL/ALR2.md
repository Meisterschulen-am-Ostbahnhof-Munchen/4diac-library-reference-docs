# ALR2

![ALR2](./ALR2.svg)

* * * * * * * * * *

## Einleitung

Der Adapter-Typ **ALR2** stellt eine bidirektionale Schnittstelle zwischen zwei Funktionsblöcken bereit. Er überträgt genau ein Ereignis zusammen mit einem LREAL-Wert in beide Richtungen. Die Bezeichnung 'ALR2' steht für 'Adapter LREAL 2-Wege'. Der Baustein ist als generischer Steckverbinder (Plug/Socket) innerhalb der 4diac-IDE konzipiert und ermöglicht eine saubere, typsichere Kopplung von Komponenten.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Name | Typ   | Kommentar                     | mit Daten |
| ---- | ----- | ----------------------------- | --------- |
| EI1  | Event | Anforderung (oder Indikation) | DI1       |

### **Ereignis-Ausgänge**

| Name | Typ   | Kommentar                     | mit Daten |
| ---- | ----- | ----------------------------- | --------- |
| EO1  | Event | Indikation (oder Anforderung) | DO1       |

### **Daten-Eingänge**

| Name | Datentyp | Kommentar                               |
| ---- | -------- | --------------------------------------- |
| DI1  | LREAL    | Anforderung (oder Indikation) an Socket |

### **Daten-Ausgänge**

| Name | Datentyp | Kommentar                                    |
| ---- | -------- | -------------------------------------------- |
| DO1  | LREAL    | Indikation (oder Anforderung) Daten vom Plug |

### **Adapter**

Keine definiert.

## Funktionsweise

Der ALR2-Adapter arbeitet ereignisgesteuert und bidirektional:

- Wird der Ereigniseingang **EI1** ausgelöst, so wird der aktuell an **DI1** anliegende LREAL-Wert zum gegenüberliegenden Adapterende übertragen. Dort erscheint das Ereignis **EO1** zusammen mit dem Wert auf **DO1**.
- In der Gegenrichtung gilt das Gleiche: Triggert der verbundene Gegenadapter sein Ereignis, so empfängt dieser ALR2 das Ereignis an **EI1** und stellt den empfangenen Wert an **DI1** bereit.

Die Kommentare „Anforderung (oder Indikation)“ und „Indikation (oder Anforderung)“ verdeutlichen, dass der Adapter je nach Einbausituation (Plug oder Socket) die Rolle eines anfordernden oder eines anzeigenden Kanals einnehmen kann.

## Technische Besonderheiten

- **Typisierte Datenübertragung:** Es wird ausschließlich der Datentyp **LREAL** (64-Bit-Gleitkommazahl) unterstützt.
- **Bidirektionalität:** Ein einziger Adapter realisiert den Datenaustausch in beide Richtungen.
- **Compiler-Information:** Das Paket ist unter `adapter::types::bidirectional` abgelegt.

## Fazit

Der ALR2-Adapter ist ein eleganter Baustein für die bidirektionale Übertragung eines LREAL-Werts mit zugehörigem Ereignis.
