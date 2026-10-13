# PUBLISH_24

* * * * * * * * * *

## Einleitung

Der Funktionsblock `PUBLISH_24` verteilt Daten an einen oder mehrere [`SUBSCRIBE_24`](SUBSCRIBE_24.md)-Blöcke. Er ermöglicht die unbestätigte Übertragung von bis zu 24 Datenwerten über ein Publish-Subscribe-Kommunikationsmuster.

![PUBLISH_24](PUBLISH_24.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **INIT** (`EInit`): Initialisierung. Verknüpft mit `QI`, `ID`.
- **REQ**: Daten senden, unbestätigt. Verknüpft mit `QI`, `SD_1` bis `SD_24`.

### **Ereignis-Ausgänge**

- **INITO** (`EInit`): Initialisierungsbestätigung. Verknüpft mit `QO`, `STATUS`.
- **CNF**: Daten wurden gesendet. Verknüpft mit `QO`, `STATUS`.

### **Daten-Eingänge**

- **QI** (`BOOL`): Ereignis-Eingangsqualifizierer
- **ID** (`WSTRING`): Identifikationsstring des Kommunikationskanals
- **SD_1** bis **SD_24** (`ANY`): zu sendende Datenwerte beliebigen Typs

### **Daten-Ausgänge**

- **QO** (`BOOL`): Ereignis-Ausgangsqualifizierer
- **STATUS** (`WSTRING`): Statusinformation

### **Adapter**

Keine Adapter-Schnittstellen vorhanden.

## Funktionsweise

Nach der Initialisierung mit `INIT` werden mit `REQ` bis zu 24 Datenwerte an alle angemeldeten Subscriber verteilt. Die Übertragung ist unbestätigt; `CNF` meldet nur, dass die Daten gesendet wurden.

## Technische Besonderheiten

- Generischer Baustein (Generic Class `GEN_PUBLISH`): Die Anzahl der Datenports ist hier 24, ihre Typen (`ANY`) werden bei der Instanziierung festgelegt
- Unicode-Strings (`WSTRING`) für `ID` und `STATUS`
- Gegenstück: `SUBSCRIBE_24` mit `RD_1` bis `RD_24`

## Anwendungsszenarien

- Verteilung vieler zusammengehöriger Werte (z. B. Messwerte oder Zustände) mit einem einzigen Ereignis an andere Applikationen oder Geräte

## ⚖️ Vergleich mit ähnlichen Bausteinen

[`PUBLISH_10`](PUBLISH_10.md) überträgt bis zu 10 Datenwerte, `PUBLISH_24` bis zu 24.

## Fazit

`PUBLISH_24` ist die unbestätigte Publish-Variante für bis zu 24 Datenwerte.
