# Softkey_Aux_IXA_TO_Remote_WRITE_BG_Option_OPC

![Softkey_Aux_IXA_TO_Remote_WRITE_BG_Option_OPC_network](./Softkey_Aux_IXA_TO_Remote_WRITE_BG_Option_OPC_network.svg)

* * * * * * * * * *

## Einleitung

`Softkey_Aux_IXA_TO_Remote_WRITE_BG_Option_OPC` ist die Option-Lib-Variante von [`Softkey_Aux_IXA_TO_Remote_WRITE_BG_OPC`](./Softkey_Aux_IXA_TO_Remote_WRITE_BG_OPC.md): VT-SoftKey lesen ODER AUX-Zuweisung (Joystick) lesen ODER lokaler Web-Override, Remote-OPC-UA-Write an ein Zielmodul und VT-Hintergrundfarbe (SoftKey und AUX) aus einem Remote-Subscribe. Ist die Option nicht ausgewählt, sind sowohl die Kommando-Seite als auch die Status-Seite inaktiv (echtes `INIT_REQ`/`INIT_CNF`, kein VT-Object-Binding). Der Baustein ist ein dünnwandiger Wrapper aus zwei Teilbausteinen, die über den `AOption`-Adapter sequenziell verkettet werden.

![Softkey_Aux_IXA_TO_Remote_WRITE_BG_Option_OPC](Softkey_Aux_IXA_TO_Remote_WRITE_BG_Option_OPC.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Ereignis | Typ | Kommentar |
|----------|-----|-----------|
| `INIT` | EInit | Initialization Request (an `Status.INIT`). |

### **Ereignis-Ausgänge**

| Ereignis | Typ | Kommentar |
|----------|-----|-----------|
| `INITO` | EInit | Initialization Confirm (von `Status.INITO`). |

### **Daten-Eingänge**

| Variable | Typ | Vorgabe | Kommentar |
|----------|-----|---------|-----------|
| `QI` | BOOL | `TRUE` | Quality Input, geht an `Status.QI`. |
| `u16ObjId` | UINT | | Object ID SoftKey/Hintergrund (VT). |
| `u16ObjIdA` | UINT | | Object ID AuxFunction2/Hintergrund (VT). |
| `ID_SUBSCRIBE` | WSTRING | | Remote-Subscribe-Adresse (Status/Farbe, `ACTION=SUBSCRIBE`). |
| `ID_WRITE_REMOTE` | WSTRING | | Remote-Write-Adresse zum Zielmodul (Befehl, `ACTION=WRITE`, CLIENT). |
| `ID_WEB_READ` | WSTRING | | Lokale Subscribe-Adresse für einen Web-Client auf demselben Modul, ODER-verknüpft mit SoftKey und AUX. |
| `ID_STATUS_WEB` | WSTRING | | Lokale Publish-Adresse für den vom Zielmodul zurückgemeldeten Zustand. |

### **Daten-Ausgänge**

| Variable | Typ | Kommentar |
|----------|-----|-----------|
| `QO` | BOOL | Quality Output (von `Status.QO`). |

### **Adapter**

| Adapter | Typ | Richtung | Kommentar |
|---------|-----|----------|-----------|
| `OPTION` | `Option::AOption` | Socket | Treibt `INIT`/`QI` von `Command` UND `Status` sequenziell; anschließen z. B. an `AOption_AUI_DEMUX_N.OUTx` oder das `PLUG1` einer vorgeschalteten Instanz. |
| `PLUG1` | `Option::AOption` | Plug | Reicht die Aktivierung an eine nachgeschaltete Instanz weiter (Daisy-Chain, z. B. eine zweite Instanz mit derselben Option); unbenutzt lassen, wenn nichts folgt. |

## Verwendete Funktionsbausteine (FBs)

### Sub-Bausteine: Softkey_Aux_IXA_TO_Remote_WRITE_BG_Option_OPC

- **Typ**: SubAppType
- **Verwendete interne FBs**:
    - **Command** (SubApp, `MyLib::sys::Softkey_Aux_IXA_TO_Remote_WRITE_Option`): SoftKey ODER Aux ODER Web-Override -> Remote-Write; siehe [`Softkey_Aux_IXA_TO_Remote_WRITE_Option`](./Softkey_Aux_IXA_TO_Remote_WRITE_Option.md).
    - **Status** (SubApp, `MyLib::sys::AX_SUBSCRIBE_BG3_WEB_Option_OPC`): Remote-Subscribe -> Hintergrundfarbe auf SoftKey und Aux plus lokaler Web-Republish; siehe [`AX_SUBSCRIBE_BG3_WEB_Option_OPC`](./AX_SUBSCRIBE_BG3_WEB_Option_OPC.md).

## Programmablauf und Verbindungen

1. Adapterkette: `OPTION` -> `Command.OPTION`; `Command.PLUG1` -> `Status.OPTION`; `Status.PLUG1` -> `PLUG1`. Die Option aktiviert damit zuerst die Kommando-Seite, dann die Status-Seite, anschließend eine nachgeschaltete Instanz.
2. Ereignisse: `INIT` -> `Status.INIT`; `Status.INITO` -> `INITO`.
3. `u16ObjId` -> `Command.u16ObjId` und `Status.u16ObjId`; `u16ObjIdA` -> `Command.u16ObjIdA` und `Status.u16ObjIdA`.
4. `ID_WRITE_REMOTE` -> `Command.ID_WRITE_REMOTE`; `ID_WEB_READ` -> `Command.ID_WEB_READ`; `ID_SUBSCRIBE` -> `Status.ID_SUBSCRIBE`; `ID_STATUS_WEB` -> `Status.ID_STATUS_WEB`.
5. `QI` -> `Status.QI`; `Status.QO` -> `QO`.

## Technische Besonderheiten

- **Command und Status sind beide Option-gated:** Alle Bausteine mit echtem `QI` sind über den `AOption`-Socket abschaltbar.
- **Sequenzielle Kette statt Socket-Split:** `Command.PLUG1` -> `Status.OPTION` (strikt 1:1), analog zum Daisy-Chain-Stil von [`OptionalGroup_001a`](../../MyLib_AX_Option/sys/OptionalGroup_001a.md). Der eigene `PLUG1` gibt die Kette an weitere Instanzen weiter, die dieselbe Option teilen.
- Wie im Original gilt: `ID_WEB_READ` (von `Command`) und `ID_STATUS_WEB` (von `Status`) müssen auf unterschiedliche lokale OPC-UA-Knoten zeigen, sonst entsteht eine Rückkopplung (siehe [`Softkey_Aux_IXA_TO_Remote_WRITE_BG_OPC`](./Softkey_Aux_IXA_TO_Remote_WRITE_BG_OPC.md)).

## Anwendungsszenarien

- Optionale Bedienstelle mit SoftKey und Aux-Zuweisung für einen entfernten Aktor samt farbiger Statusanzeige, die nur bei ausgewählter Option aktiv ist.
- Mehrere Instanzen, die dieselbe Option teilen und per `PLUG1` verkettet werden.

## Zusammenfassung

`Softkey_Aux_IXA_TO_Remote_WRITE_BG_Option_OPC` bündelt Kommando- und Status-Seite eines Remote-Kanals hinter einem gemeinsamen `AOption`-Socket und reicht die Aktivierung per `PLUG1` weiter.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
