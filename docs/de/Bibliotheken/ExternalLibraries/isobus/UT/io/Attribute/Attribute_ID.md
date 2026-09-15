# Attribute_ID

* * * * * * * * * *

## Einleitung

Der **Attribute_ID** ist ein Eingabeservice-Interface-Funktionsbaustein für Doppelwort-Attributdaten (DWORD). Er dient als Eingabeschnittstelle für asynchrone Indikationen (`IND`) und Bestätigungen (`CNF`) von VT-Objektattribut-Werten in ISOBUS-Systemen.

![Attribute_ID](Attribute_ID.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- `INIT`: Service-Initialisierung (mit `QI`, `PARAMS`, `u16ObjId`, `u8AID`)
- `REQ`: Service-Anfrage (mit `QI`)

### **Ereignis-Ausgänge**

- `INITO`: Initialisierungsbestätigung (mit `QO`, `STATUS`)
- `CNF`: Bestätigung der angeforderten Service-Anfrage (mit `QO`, `STATUS`, `IN`)
- `IND`: Asynchrone Indikation von der Ressource (mit `QO`, `STATUS`, `IN`)

### **Daten-Eingänge**

- `QI` (BOOL): Ereignis-Eingangsqualifizierer
- `PARAMS` (STRING): Service-Parameter
- `u16ObjId` (UINT): Objekt-ID (16-bit)
- `u8AID` (USINT): Attribut-ID (8-bit)

### **Daten-Ausgänge**

- `QO` (BOOL): Ereignis-Ausgangsqualifizierer
- `STATUS` (STRING): Betriebsstatusmeldung
- `IN` (DWORD): Empfangene Attributdaten von der Ressource (32-bit)

## Funktionsweise

Der Baustein initialisiert sich über das `INIT`-Ereignis. Nach der Initialisierung empfängt er asynchrone Attributwert-Indikationen (`IND`) von der VT-Ressource sowie Bestätigungen (`CNF`) auf gesendete Abfragen. Der jeweilige Attributwert wird am Ausgang `IN` als DWORD (32-Bit) bereitgestellt.

## Technische Besonderheiten

✔ **ISO 11783-6 konform**
✔ **Asynchrones Event-Handling** (Empfang über `IND` / `CNF`)
✔ **Universal einsetzbar** (Für alle VT-Objekt- und Attribut-IDs)
