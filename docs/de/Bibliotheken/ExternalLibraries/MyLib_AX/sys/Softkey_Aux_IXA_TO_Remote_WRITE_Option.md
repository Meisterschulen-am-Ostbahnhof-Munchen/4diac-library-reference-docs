# Softkey_Aux_IXA_TO_Remote_WRITE_Option

![Softkey_Aux_IXA_TO_Remote_WRITE_Option_network](./Softkey_Aux_IXA_TO_Remote_WRITE_Option_network.svg)

* * * * * * * * * *

## Einleitung

`Softkey_Aux_IXA_TO_Remote_WRITE_Option` liest einen VT-SoftKey ODER eine AUX-Zuweisung (Joystick) ODER einen lokalen Web-Override (z. B. `vt-ui-mirror` auf demselben Modul) und schreibt das Ergebnis per Remote-OPC-UA-Write an ein Zielmodul (generisch, ein Kanal, ohne Status-Feedback). Es ist die Option-Lib-Variante von [`Softkey_Aux_IXA_TO_Remote_WRITE`](./Softkey_Aux_IXA_TO_Remote_WRITE.md): Alle vier Bausteine mit eigenem `QI` (`Softkey_IXA`, `Aux_IXA`, `SUBSCRIBE_WEB`, `AX_CLIENT_1_0`) werden von einem einzigen [`Option::AOption`](../../Option/AOption.md)-Socket sequenziell initialisiert und gated. Der Plug `PLUG1` reicht die Aktivierung an eine nachgeschaltete Gruppe weiter (Daisy-Chain).

![Softkey_Aux_IXA_TO_Remote_WRITE_Option](Softkey_Aux_IXA_TO_Remote_WRITE_Option.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge / -Ausgänge**

Keine vorhanden (die Initialisierung läuft über den Adapter `OPTION`).

### **Daten-Eingänge**

| Variable | Typ | Vorgabe | Kommentar |
|----------|-----|---------|-----------|
| `u16ObjId` | UINT | `ID_NULL` | Object ID SoftKey (VT). |
| `u16ObjIdA` | UINT | `ID_NULL` | Object ID AuxFunction2 (VT). |
| `ID_WRITE_REMOTE` | WSTRING | | Remote-Write-Adresse zum Zielmodul (Befehl, `ACTION=WRITE`, CLIENT). |
| `ID_WEB_READ` | WSTRING | | Lokale Subscribe-Adresse für einen Web-Client (z. B. `vt-ui-mirror`) auf demselben Modul, ODER-verknüpft mit SoftKey und AUX. |

### **Daten-Ausgänge**

Keine vorhanden.

### **Adapter**

| Adapter | Typ | Richtung | Kommentar |
|---------|-----|----------|-----------|
| `OPTION` | `Option::AOption` | Socket | Treibt `INIT`/`QI` von `Softkey_IXA`, `Aux_IXA`, `SUBSCRIBE_WEB` und `AX_CLIENT_1_0`; anschließen z. B. an `AOption_AUI_DEMUX_N.OUTx`. |
| `PLUG1` | `Option::AOption` | Plug | Reicht die Aktivierung an eine nachgeschaltete Gruppe weiter (Daisy-Chain); unbenutzt lassen, wenn nichts folgt. |

## Verwendete Funktionsbausteine (FBs)

### Sub-Bausteine: Softkey_Aux_IXA_TO_Remote_WRITE_Option

- **Typ**: SubAppType
- **Verwendete interne FBs**:
    - **AOption_X_TO_EVENT** (`Option::conversion::AOption_X_TO_EVENT`): setzt den Socket `OPTION` in `INIT_REQ`/`QI_OUT` um, siehe [`AOption_X_TO_EVENT`](../../Option/conversion/AOption_X_TO_EVENT.md).
    - **Softkey_IXA** (`isobus::UT::io::Softkey::Softkey_IXA`): liest den SoftKey (`u16ObjId`).
    - **Aux_IXA** (`isobus::UT::io::Auxiliary::IN::Aux_IXA`): liest die AUX-Zuweisung (`u16ObjIdA`).
    - **SUBSCRIBE_WEB** (`adapter::net::AX_SUBSCRIBE_1`): liest den lokalen Web-Override (`ID_WEB_READ`).
    - **OR_MERGE** (`adapter::booleanOperators::AX_OR_3`): ODER-Verknüpfung der drei Quellen.
    - **AX_CLIENT_1_0** (`adapter::net::AX_CLIENT_1_0`): Remote-Write auf `ID_WRITE_REMOTE`.
    - **AOption_EVENT_TO_X** (`Option::conversion::AOption_EVENT_TO_X`): gibt die Aktivierung an `PLUG1` weiter.

## Programmablauf und Verbindungen

1. Sequenzielle Initialisierung: `AOption_X_TO_EVENT.INIT_REQ` -> `Softkey_IXA.INIT`; `Softkey_IXA.INITO` -> `Aux_IXA.INIT`; `Aux_IXA.INITO` -> `SUBSCRIBE_WEB.INIT`; `SUBSCRIBE_WEB.INITO` -> `AX_CLIENT_1_0.INIT`; `AX_CLIENT_1_0.INITO` -> `AOption_EVENT_TO_X.INIT_REQ`; `AOption_EVENT_TO_X.INIT_CNF` -> `AOption_X_TO_EVENT.INIT_CNF`.
2. `AOption_X_TO_EVENT.QI_OUT` -> `QI` von `Softkey_IXA`, `Aux_IXA`, `SUBSCRIBE_WEB`, `AX_CLIENT_1_0` und `AOption_EVENT_TO_X.QI_OUT`.
3. Daten: `Softkey_IXA.IN` -> `OR_MERGE.IN1`; `Aux_IXA.IN` -> `OR_MERGE.IN2`; `SUBSCRIBE_WEB.OUT` -> `OR_MERGE.IN3`; `OR_MERGE.OUT` -> `AX_CLIENT_1_0.IN`.
4. Parameter: `u16ObjId` -> `Softkey_IXA.u16ObjId`; `u16ObjIdA` -> `Aux_IXA.u16ObjId`; `ID_WEB_READ` -> `SUBSCRIBE_WEB.ID`; `ID_WRITE_REMOTE` -> `AX_CLIENT_1_0.ID`.
5. Adapter: `OPTION` -> `AOption_X_TO_EVENT.AOption_IN`; `AOption_EVENT_TO_X.AOption_OUT` -> `PLUG1`.

## Technische Besonderheiten

- **Echtes `INIT_REQ`/`INIT_CNF` statt nur eines `QI`-Bools:** Ist die Option nicht ausgewählt, bleiben alle vier Bausteine inaktiv.
- **Sequenziell statt parallel:** Die Initialisierung läuft als Daisy-Chain (`Softkey_IXA` -> `Aux_IXA` -> `SUBSCRIBE_WEB` -> `AX_CLIENT_1_0`); `PLUG1` erlaubt, weitere Instanzen mit derselben Option anzuhängen (z. B. eine zweite SoftKey-Instanz) oder, wie in [`Softkey_Aux_IXA_TO_Remote_WRITE_BG_Option_OPC`](./Softkey_Aux_IXA_TO_Remote_WRITE_BG_Option_OPC.md), die Status-Hälfte.
- Das Gegenstück ohne Option ist [`Softkey_Aux_IXA_TO_Remote_WRITE`](./Softkey_Aux_IXA_TO_Remote_WRITE.md).

## Anwendungsszenarien

- Optionale Bedienfunktion, die nur bei ausgewählter Option SoftKey, Aux und Web-Override liest und per Remote-Write an ein Zielmodul schreibt.
- Mehrere SoftKeys, die dieselbe Option teilen, per `PLUG1` verkettet.

## Zusammenfassung

`Softkey_Aux_IXA_TO_Remote_WRITE_Option` ist die Option-gated Variante des Remote-Write-Kommandos mit SoftKey, Aux und Web-Override, deren vier `QI`-Bausteine über einen `AOption`-Socket aktiviert werden und die die Aktivierung per `PLUG1` weiterreicht.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
