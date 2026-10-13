# logiBUS_IXA_BG_OPC_AX

![logiBUS_IXA_BG_OPC_AX_network](./logiBUS_IXA_BG_OPC_AX_network.svg)

* * * * * * * * * *

## Einleitung

`logiBUS_IXA_BG_OPC_AX` ist ein generischer Baustein für **einen logiBUS-DI-Kanal** mit VT-Hintergrundfarbe und OPC-UA-Publish. Gegenüber [`logiBUS_IXA_BG_OPC`](./logiBUS_IXA_BG_OPC.md) reicht er den Eingangszustand zusätzlich als `AX`-Plug `OUT` nach außen durch, sodass weitere Bausteine den Pegel verwenden können.

![logiBUS_IXA_BG_OPC_AX](logiBUS_IXA_BG_OPC_AX.svg)

## Verwendete Funktionsbausteine (FBs)

### Sub-Bausteine: logiBUS_IXA_BG_OPC_AX

- **Typ**: SubAppType
- **Verwendete interne Bausteine**:
    - **logiBUS_IXA**: `logiBUS::io::DI::logiBUS_IXA` (`QI=TRUE`) – digitaler Eingang; `Input` identifiziert `Input_I1..I8`
    - **AX_SPLIT_2_UNGATED**: `adapter::events::unidirectional::AX_SPLIT_2_UNGATED` – verzweigt den Eingangszustand in den Plug `OUT` und in den zweiten Zweig
    - **AX_SPLIT_2**: `adapter::events::unidirectional::AX_SPLIT_2` – verzweigt den zweiten Zweig in VT-Anzeige und Publish
    - **GreenWhiteBackground1_AX**: SubApp [`MyLib::sys::GreenWhiteBackground1_AX`](./GreenWhiteBackground1_AX.md) – setzt die Hintergrundfarbe (Grün/Weiß)
    - **AX_PUBLISH_1**: `adapter::net::AX_PUBLISH_1` (`QI=TRUE`) – OPC-UA-Publish
- **Importe**: `ID_NULL`, `logiBUS_DI`, `logiBUS_DI_S`

## Schnittstellenstruktur

### **Ereignis-Eingänge / Ereignis-Ausgänge**

Keine – Ereignisse laufen über den Adapter.

### **Daten-Eingänge**

- **u16ObjId** (`UINT`, Vorgabe `ID_NULL`): Object-ID des Hintergrund-Rechtecks (VT); `ID_NULL` = noch kein VT-Objekt
- **Input** (`logiBUS::io::DI::logiBUS_DI_S`, Vorgabe `logiBUS_DI::Invalid`): Identifiziert den Eingang `Input_I1..I8`
- **ID_WRITE** (`WSTRING`): OPC-UA-Publish-Schlüssel (lesbar)

### **Daten-Ausgänge**

Keine.

### **Adapter**

- **OUT** (Plug): `adapter::types::unidirectional::AX` – Eingangszustand als Pegel

## Programmablauf und Verbindungen

- `Input` → `logiBUS_IXA.Input`; `logiBUS_IXA.INITO` → `logiBUS_IXA.REQ` (erste Abfrage nach der Initialisierung)
- `logiBUS_IXA.IN` → `AX_SPLIT_2_UNGATED.IN`
- `AX_SPLIT_2_UNGATED.OUT1` → `OUT`
- `AX_SPLIT_2_UNGATED.OUT2` → `AX_SPLIT_2.IN`
- `AX_SPLIT_2.OUT1` → `GreenWhiteBackground1_AX.DI1`; `u16ObjId` → `GreenWhiteBackground1_AX.u16ObjId`
- `AX_SPLIT_2.OUT2` → `AX_PUBLISH_1.IN`; `ID_WRITE` → `AX_PUBLISH_1.ID`

## Technische Besonderheiten

- Zwei Splitter hintereinander, weil ein Adapter-Plug nur an ein Ziel angeschlossen werden kann und hier drei Ziele (`OUT`, VT-Anzeige, Publish) versorgt werden
- Einweg-Datenfluss: ein digitaler Eingang wird nicht von außen geschrieben
- Version 1.0 (2023-10-23)

## Anwendungsszenarien

- Digitaler Eingang mit VT-Statusfarbe und OPC-UA-Publish, dessen Zustand zusätzlich von weiteren Bausteinen genutzt werden soll

## Zusammenfassung

`logiBUS_IXA_BG_OPC_AX` erweitert `logiBUS_IXA_BG_OPC` um einen durchgereichten `AX`-Ausgang `OUT` für den Eingangszustand.
