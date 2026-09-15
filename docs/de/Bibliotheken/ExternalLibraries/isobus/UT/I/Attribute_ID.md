# Attribute_ID

* * * * * * * * * *

## Einleitung

Der **Attribute_ID** ist ein Eingabeservice-Interface-Funktionsbaustein für Objektattribut-Daten (UDINT). Er dient als Eingabeschnittstelle für asynchrone Indikationen (`IND`) und Bestätigungen (`CNF`) von Objektattribut-Werten in ISOBUS VT Systemen.

![Attribute_ID](Attribute_ID.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- `INIT`: Service-Initialisierung
  - Verknüpft mit: `QI`, `PARAMS`, `u16ObjId`, `u8AID`
- `REQ`: Service-Anfrage
  - Verknüpft mit: `QI`

### **Ereignis-Ausgänge**

- `INITO`: Initialisierungsbestätigung
  - Verknüpft mit: `QO`, `STATUS`
- `CNF`: Bestätigung der angeforderten Service-Anfrage
  - Verknüpft mit: `QO`, `STATUS`, `u32ValueAttribute`, `s16result`
- `IND`: Asynchrone Indikation von der Ressource
  - Verknüpft mit: `QO`, `STATUS`, `u32ValueAttribute`, `s16result`

### **Daten-Eingänge**

- `QI` (BOOL): Ereignis-Eingangsqualifizierer
- `PARAMS` (STRING): Service-Parameter
- `u16ObjId` (UINT): Objekt-ID (16-bit)
- `u8AID` (USINT): Attribut-ID (8-bit)

### **Daten-Ausgänge**

- `QO` (BOOL): Ereignis-Ausgangsqualifizierer
- `STATUS` (STRING): Betriebsstatusmeldung
- `u32ValueAttribute` (UDINT): Empfangener Attributwert (32-bit)
- `s16result` (INT): ISO-konformer Ergebniscode

## Funktionsweise

Der Baustein initialisiert sich über das `INIT`-Ereignis. Nach der Initialisierung empfängt er asynchrone Attributwert-Indikationen (`IND`) von der VT-Ressource sowie Bestätigungen (`CNF`) auf gesendete Abfragen. Der jeweilige Attributwert wird am Ausgang `u32ValueAttribute` als UDINT (32-Bit) bereitgestellt.

## Technische Besonderheiten

✔ **ISO 11783-6 konform**
✔ **Asynchrones Event-Handling** (Empfang über `IND` / `CNF`)
✔ **Universal einsetzbar** (Für alle VT-Objekt- und Attribut-IDs)
