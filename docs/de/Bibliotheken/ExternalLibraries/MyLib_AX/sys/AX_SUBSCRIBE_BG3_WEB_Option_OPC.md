# AX_SUBSCRIBE_BG3_WEB_Option_OPC

![AX_SUBSCRIBE_BG3_WEB_Option_OPC_network](./AX_SUBSCRIBE_BG3_WEB_Option_OPC_network.svg)

* * * * * * * * * *

## Einleitung

`AX_SUBSCRIBE_BG3_WEB_Option_OPC` ist die Option-Lib-Variante von [`AX_SUBSCRIBE_BG3_WEB_OPC`](./AX_SUBSCRIBE_BG3_WEB_OPC.md): OPC-UA-Remote-Subscribe mit VT-Hintergrundfarbe auf SoftKey UND AuxFunction2 sowie lokalem Web-Republish. Die Bausteine mit echtem `QI` (`AX_SUBSCRIBE_1` und `STATUS_WEB_PUBLISH`) werden hier vom Adapter-Socket [`Option::AOption`](../../Option/AOption.md) sequenziell initialisiert und gated. `GreenWhiteBackground3_AX` bleibt unverändert und wird wie im Original verteilt (siehe Technische Besonderheiten). Der Plug `PLUG1` reicht die Aktivierung an eine nachgeschaltete Gruppe weiter (Daisy-Chain).

![AX_SUBSCRIBE_BG3_WEB_Option_OPC](AX_SUBSCRIBE_BG3_WEB_Option_OPC.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Ereignis | Typ | Kommentar |
|----------|-----|-----------|
| `INIT` | EInit | Initialization Request (an `GreenWhiteBackground3_AX.INIT`). |

### **Ereignis-Ausgänge**

| Ereignis | Typ | Kommentar |
|----------|-----|-----------|
| `INITO` | EInit | Initialization Confirm (von `GreenWhiteBackground3_AX.INITO`). |

### **Daten-Eingänge**

| Variable | Typ | Vorgabe | Kommentar |
|----------|-----|---------|-----------|
| `QI` | BOOL | `TRUE` | Quality Input, geht an `GreenWhiteBackground3_AX.QI`. |
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
| `OPTION` | `Option::AOption` | Socket | Treibt `INIT`/`QI` von `AX_SUBSCRIBE_1` und `STATUS_WEB_PUBLISH`; typischerweise vom `PLUG1` von [`Softkey_Aux_IXA_TO_Remote_WRITE_Option`](./Softkey_Aux_IXA_TO_Remote_WRITE_Option.md) gespeist. |
| `PLUG1` | `Option::AOption` | Plug | Reicht die Aktivierung an eine nachgeschaltete Gruppe weiter (Daisy-Chain); unbenutzt lassen, wenn nichts folgt. |

## Verwendete Funktionsbausteine (FBs)

### Sub-Bausteine: AX_SUBSCRIBE_BG3_WEB_Option_OPC

- **Typ**: SubAppType
- **Verwendete interne FBs**:
    - **AOption_X_TO_EVENT** (`Option::conversion::AOption_X_TO_EVENT`): setzt den Socket `OPTION` in `INIT_REQ`/`QI_OUT` um, siehe [`AOption_X_TO_EVENT`](../../Option/conversion/AOption_X_TO_EVENT.md).
    - **AX_SUBSCRIBE_1** (`adapter::net::AX_SUBSCRIBE_1`): OPC-UA-Subscribe auf `ID_SUBSCRIBE`.
    - **AX_SPLIT_2** (`adapter::events::unidirectional::AX_SPLIT_2`): verteilt den Subscribe-Wert auf zwei Zweige.
    - **GreenWhiteBackground3_AX** (SubApp `MyLib::sys::GreenWhiteBackground3_AX`): setzt die Hintergrundfarbe auf SoftKey und AuxFunction2.
    - **STATUS_WEB_PUBLISH** (`adapter::net::AX_PUBLISH_1`): lokaler Publish des Status auf `ID_STATUS_WEB`.
    - **AOption_EVENT_TO_X** (`Option::conversion::AOption_EVENT_TO_X`): gibt die Aktivierung an `PLUG1` weiter.

## Programmablauf und Verbindungen

1. Option-Kette: `AOption_X_TO_EVENT.INIT_REQ` -> `AX_SUBSCRIBE_1.INIT`; `AX_SUBSCRIBE_1.INITO` -> `STATUS_WEB_PUBLISH.INIT`; `STATUS_WEB_PUBLISH.INITO` -> `AOption_EVENT_TO_X.INIT_REQ`; `AOption_EVENT_TO_X.INIT_CNF` -> `AOption_X_TO_EVENT.INIT_CNF`.
2. `AOption_X_TO_EVENT.QI_OUT` -> `AX_SUBSCRIBE_1.QI`, `STATUS_WEB_PUBLISH.QI` und `AOption_EVENT_TO_X.QI_OUT`.
3. Adapter: `OPTION` -> `AOption_X_TO_EVENT.AOption_IN`; `AOption_EVENT_TO_X.AOption_OUT` -> `PLUG1`.
4. Daten: `AX_SUBSCRIBE_1.OUT` -> `AX_SPLIT_2.IN`; `AX_SPLIT_2.OUT1` -> `GreenWhiteBackground3_AX.DI1`; `AX_SPLIT_2.OUT2` -> `STATUS_WEB_PUBLISH.IN`.
5. Parameter: `ID_SUBSCRIBE` -> `AX_SUBSCRIBE_1.ID`; `ID_STATUS_WEB` -> `STATUS_WEB_PUBLISH.ID`; `u16ObjId`/`u16ObjIdA` -> `GreenWhiteBackground3_AX.u16ObjId`/`u16ObjIdA`.
6. Außerhalb der Option-Kette: `INIT` -> `GreenWhiteBackground3_AX.INIT`, `GreenWhiteBackground3_AX.INITO` -> `INITO`, `QI` -> `GreenWhiteBackground3_AX.QI`, `GreenWhiteBackground3_AX.QO` -> `QO`.

## Technische Besonderheiten

- **Nur QI-Bausteine gated:** Nur `AX_SUBSCRIBE_1` und `STATUS_WEB_PUBLISH` (beide haben `QI`) hängen am Option-Socket. `GreenWhiteBackground3_AX` hat kein `QI`/`QO`-Gating über die Option; die Aktivierung läuft dort über `u16ObjId` gegen `ID_NULL` im eigenen INIT, und es wird ohne aktive Subscription davor ohnehin nicht mit frischen Daten angesteuert.
- **Option-Initialisierung getrennt von `INIT`/`INITO`:** Der Eingang `INIT` und der Ausgang `INITO` gehören zu `GreenWhiteBackground3_AX`; die Option-Kette hat ihren eigenen `INIT_REQ`/`INIT_CNF`-Handshake.
- **Daisy-Chain:** `PLUG1` erlaubt, weitere Instanzen oder Gruppen an dieselbe Option zu hängen.
- Das Gegenstück ohne Option ist [`AX_SUBSCRIBE_BG3_WEB_OPC`](./AX_SUBSCRIBE_BG3_WEB_OPC.md).

## Anwendungsszenarien

- Status-Hälfte von [`Softkey_Aux_IXA_TO_Remote_WRITE_BG_Option_OPC`](./Softkey_Aux_IXA_TO_Remote_WRITE_BG_Option_OPC.md): Hintergrundfarbe auf SoftKey und Aux aus einem Remote-Subscribe, nur aktiv, wenn die Option ausgewählt ist.

## Zusammenfassung

`AX_SUBSCRIBE_BG3_WEB_Option_OPC` kombiniert Remote-Subscribe, Hintergrundfarbe auf SoftKey und Aux sowie lokalen Web-Republish und gated die Bausteine mit `QI` über einen `AOption`-Socket mit Weiterreichung per `PLUG1`.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
