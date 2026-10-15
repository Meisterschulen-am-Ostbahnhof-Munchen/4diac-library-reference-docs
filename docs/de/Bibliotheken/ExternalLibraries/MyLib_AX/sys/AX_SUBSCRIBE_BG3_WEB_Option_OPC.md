# AX_SUBSCRIBE_BG3_WEB_Option_OPC

![AX_SUBSCRIBE_BG3_WEB_Option_OPC_network](./AX_SUBSCRIBE_BG3_WEB_Option_OPC_network.svg)

* * * * * * * * * *

## Einleitung

`AX_SUBSCRIBE_BG3_WEB_Option_OPC` ist die Option-Lib-Variante von [`AX_SUBSCRIBE_BG3_WEB_OPC`](./AX_SUBSCRIBE_BG3_WEB_OPC.md): OPC-UA-Remote-Subscribe mit VT-Hintergrundfarbe auf SoftKey UND AuxFunction2 sowie lokalem Web-Republish. `AX_SUBSCRIBE_1`, `STATUS_WEB_PUBLISH` und `GreenWhiteBackground3_AX` (hat seit dem QI/QO-Rollout echtes `QI`/`QO`) werden hier vom Adapter-Socket [`Option::AOption`](../../Option/AOption.md) initialisiert und gated: Subscribe/Publish sequenziell, `GreenWhiteBackground3_AX` parallel dazu; `E_REND_2_INITO` wartet auf beide Zweige (siehe Technische Besonderheiten). Der Plug `PLUG1` reicht die Aktivierung an eine nachgeschaltete Gruppe weiter (Daisy-Chain).

![AX_SUBSCRIBE_BG3_WEB_Option_OPC](AX_SUBSCRIBE_BG3_WEB_Option_OPC.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Ereignis | Typ | Kommentar |
|----------|-----|-----------|
| `INIT` | EInit | Initialization Request (nicht intern verdrahtet; die Initialisierung läuft über den Socket `OPTION`). |

### **Ereignis-Ausgänge**

| Ereignis | Typ | Kommentar |
|----------|-----|-----------|
| `INITO` | EInit | Initialization Confirm (von `AOption_EVENT_TO_X.INIT_CNF`, also nach dem Rendezvous beider Zweige). |

### **Daten-Eingänge**

| Variable | Typ | Vorgabe | Kommentar |
|----------|-----|---------|-----------|
| `QI` | BOOL | `TRUE` | Quality Input (nicht intern verdrahtet; `GreenWhiteBackground3_AX.QI` kommt über `AOption_X_TO_EVENT.QI_OUT`). |
| `u16ObjId` | UINT | `ID_NULL` | Objekt-ID SoftKey/Hintergrund (VT). |
| `u16ObjIdA` | UINT | `ID_NULL` | Objekt-ID AuxFunction2/Hintergrund (VT). |
| `ID_SUBSCRIBE` | WSTRING | | Remote-Subscribe-Adresse (Status/Farbe, `ACTION=SUBSCRIBE`). |
| `ID_STATUS_WEB` | WSTRING | | Lokale Publish-Adresse für den vom Zielmodul zurückgemeldeten Zustand, damit ein Web-Client (z. B. `vt-ui-mirror`) dieselbe Hintergrundfarbe wie das echte VT zeigen kann. |

### **Daten-Ausgänge**

| Variable | Typ | Kommentar |
|----------|-----|-----------|
| `QO` | BOOL | Quality Output (von `GreenWhiteBackground3_AX.QO`). |

### **Adapter**

| Adapter | Typ | Richtung | Kommentar |
|---------|-----|----------|-----------|
| `OPTION` | `Option::AOption` | Socket | Treibt `INIT`/`QI` von `AX_SUBSCRIBE_1`, `STATUS_WEB_PUBLISH` und `GreenWhiteBackground3_AX`; typischerweise vom `PLUG1` von [`Softkey_Aux_IXA_TO_Remote_WRITE_Option`](./Softkey_Aux_IXA_TO_Remote_WRITE_Option.md) gespeist. |
| `PLUG1` | `Option::AOption` | Plug | Reicht die Aktivierung an eine nachgeschaltete Gruppe weiter (Daisy-Chain); unbenutzt lassen, wenn nichts folgt. |

## Verwendete Funktionsbausteine (FBs)

### Sub-Bausteine: AX_SUBSCRIBE_BG3_WEB_Option_OPC

- **Typ**: SubAppType
- **Verwendete interne FBs**:
    - **AOption_X_TO_EVENT** (`Option::conversion::AOption_X_TO_EVENT`): setzt den Socket `OPTION` in `INIT_REQ`/`QI_OUT` um, siehe [`AOption_X_TO_EVENT`](../../Option/conversion/AOption_X_TO_EVENT.md).
    - **AX_SUBSCRIBE_1** (`adapter::net::AX_SUBSCRIBE_1`): OPC-UA-Subscribe auf `ID_SUBSCRIBE`.
    - **AX_SPLIT_2** (`adapter::events::unidirectional::AX_SPLIT_2`): verteilt den Subscribe-Wert auf zwei Zweige.
    - **GreenWhiteBackground3_AX** (SubApp `MyLib::sys::GreenWhiteBackground3_AX`): setzt die Hintergrundfarbe auf SoftKey und AuxFunction2; wird vom Option-Socket parallel zu Subscribe/Publish initialisiert und gated.
    - **E_REND_2_INITO** (`iec61499::events::E_REND_2`): Rendezvous; wartet auf `STATUS_WEB_PUBLISH.INITO` UND `GreenWhiteBackground3_AX.INITO`, bevor der Abschluss der Option-Kette feuert; wird von `AOption_X_TO_EVENT.INIT_REQ` für jeden neuen Umschaltvorgang zurückgesetzt.
    - **STATUS_WEB_PUBLISH** (`adapter::net::AX_PUBLISH_1`): lokaler Publish des Status auf `ID_STATUS_WEB`.
    - **AOption_EVENT_TO_X** (`Option::conversion::AOption_EVENT_TO_X`): gibt die Aktivierung an `PLUG1` weiter.

## Programmablauf und Verbindungen

1. Option-Kette: `AOption_X_TO_EVENT.INIT_REQ` -> `AX_SUBSCRIBE_1.INIT`, `GreenWhiteBackground3_AX.INIT` und `E_REND_2_INITO.R`; `AX_SUBSCRIBE_1.INITO` -> `STATUS_WEB_PUBLISH.INIT`; `STATUS_WEB_PUBLISH.INITO` -> `E_REND_2_INITO.EI1`; `GreenWhiteBackground3_AX.INITO` -> `E_REND_2_INITO.EI2`; `E_REND_2_INITO.EO` -> `AOption_EVENT_TO_X.INIT_REQ`; `AOption_EVENT_TO_X.INIT_CNF` -> `AOption_X_TO_EVENT.INIT_CNF` und -> `INITO`.
2. `AOption_X_TO_EVENT.QI_OUT` -> `AX_SUBSCRIBE_1.QI`, `STATUS_WEB_PUBLISH.QI`, `GreenWhiteBackground3_AX.QI` und `AOption_EVENT_TO_X.QI_OUT`.
3. Adapter: `OPTION` -> `AOption_X_TO_EVENT.AOption_IN`; `AOption_EVENT_TO_X.AOption_OUT` -> `PLUG1`.
4. Daten: `AX_SUBSCRIBE_1.OUT` -> `AX_SPLIT_2.IN`; `AX_SPLIT_2.OUT1` -> `GreenWhiteBackground3_AX.DI1`; `AX_SPLIT_2.OUT2` -> `STATUS_WEB_PUBLISH.IN`.
5. Parameter: `ID_SUBSCRIBE` -> `AX_SUBSCRIBE_1.ID`; `ID_STATUS_WEB` -> `STATUS_WEB_PUBLISH.ID`; `u16ObjId`/`u16ObjIdA` -> `GreenWhiteBackground3_AX.u16ObjId`/`u16ObjIdA`.
6. Schnittstelle: `AOption_EVENT_TO_X.INIT_CNF` -> `INITO`, `GreenWhiteBackground3_AX.QO` -> `QO`. Die Schnittstellen-Eingänge `INIT` und `QI` sind intern nicht verdrahtet.

## Technische Besonderheiten

- **Alle drei Bausteine gated:** `AX_SUBSCRIBE_1`, `STATUS_WEB_PUBLISH` und `GreenWhiteBackground3_AX` hängen am Option-Socket (Version 1.1; `GreenWhiteBackground3_AX` hat seit dem QI/QO-Rollout echtes `QI`/`QO`, die frühere Annahme ohne Gating war veraltet).
- **Parallele Initialisierung mit Rendezvous:** `GreenWhiteBackground3_AX.INIT` wird parallel statt sequenziell nach `STATUS_WEB_PUBLISH` ausgelöst; `E_REND_2_INITO` wartet auf beide Zweige, sonst verliert der erste eintreffende Remote-Wert das Rennen gegen die eigene `Q_BackgroundColour`-Initialisierung.
- **Option-Initialisierung statt `INIT`:** Der Schnittstellen-Eingang `INIT` ist intern nicht verdrahtet; die Initialisierung läuft über den `INIT_REQ`/`INIT_CNF`-Handshake der Option-Kette, `INITO` wird aus `AOption_EVENT_TO_X.INIT_CNF` gespeist.
- **Daisy-Chain:** `PLUG1` erlaubt, weitere Instanzen oder Gruppen an dieselbe Option zu hängen.
- Das Gegenstück ohne Option ist [`AX_SUBSCRIBE_BG3_WEB_OPC`](./AX_SUBSCRIBE_BG3_WEB_OPC.md).

## Anwendungsszenarien

- Status-Hälfte von [`Softkey_Aux_IXA_TO_Remote_WRITE_BG_Option_OPC`](./Softkey_Aux_IXA_TO_Remote_WRITE_BG_Option_OPC.md): Hintergrundfarbe auf SoftKey und Aux aus einem Remote-Subscribe, nur aktiv, wenn die Option ausgewählt ist.

## Zusammenfassung

`AX_SUBSCRIBE_BG3_WEB_Option_OPC` kombiniert Remote-Subscribe, Hintergrundfarbe auf SoftKey und Aux sowie lokalen Web-Republish und gated Subscribe, Publish und Hintergrundfarbe über einen `AOption`-Socket mit Weiterreichung per `PLUG1`.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
