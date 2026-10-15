# Softkey_Aux_IXA_TO_Remote_WRITE_BG_Option_OPC

![Softkey_Aux_IXA_TO_Remote_WRITE_BG_Option_OPC_network](./Softkey_Aux_IXA_TO_Remote_WRITE_BG_Option_OPC_network.svg)

* * * * * * * * * *

## Einleitung

`Softkey_Aux_IXA_TO_Remote_WRITE_BG_Option_OPC` ist die Option-Lib-Variante von [`Softkey_Aux_IXA_TO_Remote_WRITE_BG_OPC`](./Softkey_Aux_IXA_TO_Remote_WRITE_BG_OPC.md): VT-SoftKey lesen ODER AUX-Zuweisung (Joystick) lesen ODER lokaler Web-Override, Remote-OPC-UA-Write an ein Zielmodul und VT-Hintergrundfarbe (SoftKey und AUX) aus einem Remote-Subscribe. Ist die Option nicht ausgewählt, sind sowohl die Kommando-Seite als auch die Status-Seite inaktiv (echtes `INIT_REQ`/`INIT_CNF`, kein VT-Object-Binding). Der Baustein ist ein dünnwandiger Wrapper aus zwei Teilbausteinen, die über den `AOption`-Adapter sequenziell verkettet werden.

![Softkey_Aux_IXA_TO_Remote_WRITE_BG_Option_OPC](Softkey_Aux_IXA_TO_Remote_WRITE_BG_Option_OPC.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Keine vorhanden (die Initialisierung läuft über den Adapter `AOption_IN`).

### **Ereignis-Ausgänge**

Keine vorhanden.

### **Daten-Eingänge**

| Variable | Typ | Vorgabe | Kommentar |
|----------|-----|---------|-----------|
| `u16ObjId` | UINT | | Object ID SoftKey/Hintergrund (VT). |
| `u16ObjIdA` | UINT | | Object ID AuxFunction2/Hintergrund (VT). |
| `ID_SUBSCRIBE` | WSTRING | | Remote-Subscribe-Adresse (Status/Farbe, `ACTION=SUBSCRIBE`). |
| `ID_WRITE_REMOTE` | WSTRING | | Remote-Write-Adresse zum Zielmodul (Befehl, `ACTION=WRITE`, CLIENT). |
| `ID_WEB_READ` | WSTRING | | Lokale Subscribe-Adresse für einen Web-Client auf demselben Modul, ODER-verknüpft mit SoftKey und AUX. |
| `ID_STATUS_WEB` | WSTRING | | Lokale Publish-Adresse für den vom Zielmodul zurückgemeldeten Zustand. |

### **Daten-Ausgänge**

Keine vorhanden.

### **Adapter**

| Adapter | Typ | Richtung | Kommentar |
|---------|-----|----------|-----------|
| `AOption_IN` | `Option::AOption` | Socket | Treibt `INIT`/`QI` von `Command` UND `Status` sequenziell; anschließen z. B. an `AOption_AUI_DEMUX_N.OUTx` oder das `AOption_OUT` einer vorgeschalteten Instanz. |
| `AOption_OUT` | `Option::AOption` | Plug | Reicht die Aktivierung an eine nachgeschaltete Instanz weiter (Daisy-Chain, z. B. eine zweite Instanz mit derselben Option); unbenutzt lassen, wenn nichts folgt. |

## Verwendete Funktionsbausteine (FBs)

### Sub-Bausteine: Softkey_Aux_IXA_TO_Remote_WRITE_BG_Option_OPC

- **Typ**: SubAppType
- **Verwendete interne FBs**:
    - **Command** (SubApp, `MyLib::sys::Softkey_Aux_IXA_TO_Remote_WRITE_Option`): SoftKey ODER Aux ODER Web-Override -> Remote-Write; siehe [`Softkey_Aux_IXA_TO_Remote_WRITE_Option`](./Softkey_Aux_IXA_TO_Remote_WRITE_Option.md).
    - **Status** (SubApp, `MyLib::sys::AX_SUBSCRIBE_BG3_WEB_Option_OPC`): Remote-Subscribe -> Hintergrundfarbe auf SoftKey und Aux plus lokaler Web-Republish; siehe [`AX_SUBSCRIBE_BG3_WEB_Option_OPC`](./AX_SUBSCRIBE_BG3_WEB_Option_OPC.md).

## Programmablauf und Verbindungen

1. Adapterkette: `AOption_IN` -> `Command.AOption_IN`; `Command.AOption_OUT` -> `Status.AOption_IN`; `Status.AOption_OUT` -> `AOption_OUT`. Die Option aktiviert damit zuerst die Kommando-Seite, dann die Status-Seite, anschließend eine nachgeschaltete Instanz.
2. `u16ObjId` -> `Command.u16ObjId` und `Status.u16ObjId`; `u16ObjIdA` -> `Command.u16ObjIdA` und `Status.u16ObjIdA`.
3. `ID_WRITE_REMOTE` -> `Command.ID_WRITE_REMOTE`; `ID_WEB_READ` -> `Command.ID_WEB_READ`; `ID_SUBSCRIBE` -> `Status.ID_SUBSCRIBE`; `ID_STATUS_WEB` -> `Status.ID_STATUS_WEB`.
4. Der Baustein hat keine eigenen Ereignis- und `QI`/`QO`-Anschlüsse; Aktivierung und Bestätigung laufen ausschließlich über die Adapter `AOption_IN`/`AOption_OUT`.

## Technische Besonderheiten

- **Command und Status sind beide Option-gated:** Alle Bausteine mit echtem `QI` sind über den `AOption`-Socket abschaltbar.
- **Sequenzielle Kette statt Socket-Split:** `Command.AOption_OUT` -> `Status.AOption_IN` (strikt 1:1), analog zum Daisy-Chain-Stil von [`OptionalGroup_001a`](../../MyLib_AX_Option/sys/OptionalGroup_001a.md). Der eigene `AOption_OUT` gibt die Kette an weitere Instanzen weiter, die dieselbe Option teilen.
- Wie im Original gilt: `ID_WEB_READ` (von `Command`) und `ID_STATUS_WEB` (von `Status`) müssen auf unterschiedliche lokale OPC-UA-Knoten zeigen, sonst entsteht eine Rückkopplung (siehe [`Softkey_Aux_IXA_TO_Remote_WRITE_BG_OPC`](./Softkey_Aux_IXA_TO_Remote_WRITE_BG_OPC.md)).

## Anwendungsszenarien

- Optionale Bedienstelle mit SoftKey und Aux-Zuweisung für einen entfernten Aktor samt farbiger Statusanzeige, die nur bei ausgewählter Option aktiv ist.
- Mehrere Instanzen, die dieselbe Option teilen und per `AOption_OUT` verkettet werden.

## Zusammenfassung

`Softkey_Aux_IXA_TO_Remote_WRITE_BG_Option_OPC` bündelt Kommando- und Status-Seite eines Remote-Kanals hinter einem gemeinsamen `AOption`-Socket und reicht die Aktivierung per `AOption_OUT` weiter.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
