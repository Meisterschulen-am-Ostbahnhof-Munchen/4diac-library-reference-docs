# Attribute_IDA

* * * * * * * * * *

## Einleitung

Der **Attribute_IDA** ist ein Composite-Adapter-Wrapper-Funktionsbaustein um `Attribute_ID`. Er stellt die empfangenen Objektattribut-Daten über einen unidirektionalen `AD`-Adapter-Plug (`IN`) für adapter-native Netzwerke bereit.

![Attribute_IDA](Attribute_IDA.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- `INIT`: Service-Initialisierung (mit `QI`, `PARAMS`, `u16ObjId`, `u8AID`)
- `REQ`: Service-Anfrage (mit `QI`)

### **Ereignis-Ausgänge**

- `INITO`: Initialisierungsbestätigung (mit `QO`, `STATUS`)

### **Daten-Eingänge**

- `QI` (BOOL): Ereignis-Eingangsqualifizierer
- `PARAMS` (STRING): Service-Parameter
- `u16ObjId` (UINT): Objekt-ID
- `u8AID` (USINT): Attribut-ID

### **Daten-Ausgänge**

- `QO` (BOOL): Ereignis-Ausgangsqualifizierer
- `STATUS` (STRING): Betriebsstatusmeldung

### **Adapter**

- `IN` (AD Plug): Unidirektionaler DWORD-Adapter-Plug zur Bereitstellung der Attributdaten

## Funktionsweise

`Attribute_IDA` kapselt den `Attribute_ID`-Baustein intern. Bei eintreffenden `IND`- oder `CNF`-Events leitet er die Ereignisse und die 32-Bit-Attributdaten automatisch auf die Schnittstelle des `AD`-Adapters (`IN.E1` und `IN.D1`) weiter.

## Technische Besonderheiten

✔ **Adapter-native Schnittstelle** (`AD` Plug)
✔ **Asynchrones Event-Routing** (`IND`/`CNF` -> `IN.E1`)
✔ **Einfache Einbindung in Adapternetzwerke**
