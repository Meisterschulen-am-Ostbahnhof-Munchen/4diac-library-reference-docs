# Button_IXA_TO_Remote_WRITE_BG_Option_OPC

![Button_IXA_TO_Remote_WRITE_BG_Option_OPC_network](./Button_IXA_TO_Remote_WRITE_BG_Option_OPC_network.svg)

* * * * * * * * * *

## Einleitung

`Button_IXA_TO_Remote_WRITE_BG_Option_OPC` liest einen VT-Button, schreibt per Remote-OPC-UA-Write an ein Zielmodul und setzt die Hintergrundfarbe aus einem Remote-Subscribe – wie [`Button_IXA_TO_Remote_WRITE_BG_OPC`](./Button_IXA_TO_Remote_WRITE_BG_OPC.md), aber per `AOption` aktiviert statt über `QI`/`INIT`. `AOption_OUT` reicht die Aktivierung nach dem eigenen `INIT` weiter (Daisy-Chain wie `OptionalGroup_001a`). Der Baustein ist eine Kopie von `Button_IXA_TO_Remote_WRITE_BG_OPC`, bei der `INIT`/`INITO`/`QI`/`QO` durch `AOption_IN` und `AOption_OUT` ersetzt wurden.

![Button_IXA_TO_Remote_WRITE_BG_Option_OPC](Button_IXA_TO_Remote_WRITE_BG_Option_OPC.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Keine vorhanden.

### **Ereignis-Ausgänge**

Keine vorhanden.

### **Daten-Eingänge**

| Variable | Typ | Vorgabe | Kommentar |
|----------|-----|---------|-----------|
| `u16ObjId` | UINT | `ID_NULL` | Object ID Button/Hintergrund (VT). |
| `ID_SUBSCRIBE` | WSTRING |  | Remote-Subscribe-Adresse (Status/Farbe, `ACTION=SUBSCRIBE`). |
| `ID_WRITE_REMOTE` | WSTRING |  | Remote-Write-Adresse zum Zielmodul (Befehl, `ACTION=WRITE`, CLIENT). |

### **Daten-Ausgänge**

Keine vorhanden.

### **Adapter**

| Adapter | Typ | Richtung | Kommentar |
|---------|-----|----------|-----------|
| `AOption_IN` | `Option::AOption` | Socket | Treibt `INIT`/`QI`; anschließen an `AOption_AUI_DEMUX_N.OUTx` oder `AOption_OUT` einer vorgeschalteten Instanz. |
| `AOption_OUT` | `Option::AOption` | Plug | Reicht die Aktivierung nach dem eigenen INIT an das nächste Glied weiter (Daisy-Chain); muss verbunden sein, Kettenende z. B. `DummyGroup`. |

## Verwendete Funktionsbausteine (FBs)

- **AOption_X_TO_EVENT** (`Option::conversion::AOption_X_TO_EVENT`): wandelt die Option in `INIT_REQ`/`QI_OUT` und nimmt `INIT_CNF` entgegen
- **Button_IXA** (`isobus::UT::io::Button::Button_IXA`): liest den VT-Button
- **AX_CLIENT_1_0** (`adapter::net::AX_CLIENT_1_0`): Remote-Write zum Zielmodul
- **AX_SUBSCRIBE_1** (`adapter::net::AX_SUBSCRIBE_1`): Remote-Subscribe von Status/Farbe
- **GreenWhiteBackground1_AX** (SubApp `MyLib::sys::GreenWhiteBackground1_AX`): Hintergrundfarbe; siehe [`GreenWhiteBackground1_AX`](./GreenWhiteBackground1_AX.md)
- **AOption_EVENT_TO_X** (`Option::conversion::AOption_EVENT_TO_X`): gibt die Aktivierung an `AOption_OUT` weiter

## Programmablauf und Verbindungen

1. Initialisierungskette: `AOption_X_TO_EVENT.INIT_REQ` -> `Button_IXA.INIT`; `Button_IXA.INITO` -> `AX_CLIENT_1_0.INIT`; `AX_CLIENT_1_0.INITO` -> `AX_SUBSCRIBE_1.INIT`; `AX_SUBSCRIBE_1.INITO` -> `GreenWhiteBackground1_AX.INIT`; `GreenWhiteBackground1_AX.INITO` -> `AOption_EVENT_TO_X.INIT_REQ`; `AOption_EVENT_TO_X.INIT_CNF` -> `AOption_X_TO_EVENT.INIT_CNF`.
2. Qualifier: `AOption_X_TO_EVENT.QI_OUT` -> `Button_IXA.QI`, `AX_CLIENT_1_0.QI`, `AX_SUBSCRIBE_1.QI`, `GreenWhiteBackground1_AX.QI` und `AOption_EVENT_TO_X.QI_OUT`.
3. Option: `AOption_IN` -> `AOption_X_TO_EVENT.AOption_IN`; `AOption_EVENT_TO_X.AOption_OUT` -> `AOption_OUT`.
4. Befehl: `Button_IXA.IN` -> `AX_CLIENT_1_0.IN`.
5. Status: `AX_SUBSCRIBE_1.OUT` -> `GreenWhiteBackground1_AX.DI1`.
6. Daten: `u16ObjId` -> `Button_IXA.u16ObjId` und `GreenWhiteBackground1_AX.u16ObjId`; `ID_WRITE_REMOTE` -> `AX_CLIENT_1_0.ID`; `ID_SUBSCRIBE` -> `AX_SUBSCRIBE_1.ID`.

## Technische Besonderheiten

- Der Baustein hat keine eigenen Ereignis- und `QI`/`QO`-Anschlüsse; Aktivierung und Bestätigung laufen ausschließlich über `AOption_IN`/`AOption_OUT`.
- `AOption_OUT` muss weiterverbunden sein (z. B. `DummyGroup` als Kettenende), sonst kommt kein `INIT_CNF` zurück.

## Anwendungsszenarien

- Optionale Bedienstelle mit VT-Button für einen entfernten Aktor samt farbiger Statusanzeige, aktiv nur bei ausgewählter Option.
- Mehrere Instanzen, die dieselbe Option teilen und per `AOption_OUT` verkettet werden.

## Zusammenfassung

`Button_IXA_TO_Remote_WRITE_BG_Option_OPC` bündelt VT-Button, Remote-Write und Hintergrundfarbe aus Remote-Subscribe hinter einem `AOption`-Socket und reicht die Aktivierung per `AOption_OUT` weiter.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
