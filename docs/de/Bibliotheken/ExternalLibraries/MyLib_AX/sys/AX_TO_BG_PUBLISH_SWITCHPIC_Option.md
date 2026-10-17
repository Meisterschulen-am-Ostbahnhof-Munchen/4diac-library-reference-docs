# AX_TO_BG_PUBLISH_SWITCHPIC_Option

![AX_TO_BG_PUBLISH_SWITCHPIC_Option_network](./AX_TO_BG_PUBLISH_SWITCHPIC_Option_network.svg)

* * * * * * * * * *

## Einleitung

`AX_TO_BG_PUBLISH_SWITCHPIC_Option` ist eine reine Anzeige: Er bringt einen fertigen AX-Zustand auf die Hintergrundfarbe (SoftKey und AUX), einen lokalen State-Publish und die ObjectPointer-Statusanzeige ([`AX_SwitchPic_2_1`](./AX_SwitchPic_2_1.md)). Aktiviert wird er per `AOption`: `INIT` und `QI` kommen über `AOption_X_TO_EVENT`, das `INIT_CNF` kommt echt nach `ObjPtr.INITO` zurück. Er ist der Anzeige-Teil von [`AX_TO_QXA_BG_PUBLISH_SWITCHPIC_OPT`](./AX_TO_QXA_BG_PUBLISH_SWITCHPIC_OPT.md) als eigener Baustein und entspricht [`AX_TO_QXA_BG_PUBLISH_SWITCHPIC`](./AX_TO_QXA_BG_PUBLISH_SWITCHPIC.md) ohne physischen Ausgang.

![AX_TO_BG_PUBLISH_SWITCHPIC_Option](AX_TO_BG_PUBLISH_SWITCHPIC_Option.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Keine vorhanden.

### **Ereignis-Ausgänge**

Keine vorhanden.

### **Daten-Eingänge**

| Variable | Typ | Vorgabe | Kommentar |
|----------|-----|---------|-----------|
| `u16ObjId` | UINT | `ID_NULL` | Object ID SoftKey; trägt die Hintergrundfarbe. |
| `u16ObjIdA` | UINT | `ID_NULL` | Object ID AuxFunction2 (Joystick); zeigt die Hintergrundfarbe am Aux-Handle mit. |
| `ID_STATE_WRITE` | WSTRING |  | Lokale Publish-Adresse (`ACTION=WRITE`) für den Zustand; wird remote abonniert. |
| `Picture` | UINT |  | Object ID des ObjectPointers, der den Status-Kästchen-Inhalt trägt. |
| `pictures` | `isobus::UT::Q::types::s2x1ObjectIDs` | `(up := (u16ObjId := ID_NULL), down := (u16ObjId := ID_NULL))` | Die beiden Ziel-Object-IDs (`up` = aus, `down` = an). |

### **Daten-Ausgänge**

Keine vorhanden.

### **Adapter**

| Adapter | Typ | Richtung | Kommentar |
|---------|-----|----------|-----------|
| `AOption_IN` | `Option::AOption` | Socket | Aktiviert/deaktiviert die Anzeige. |
| `IN` | `adapter::types::unidirectional::AX` | Socket | Fertiger AX-Zustand für Hintergrund, Publish und ObjectPointer. |

## Verwendete Funktionsbausteine (FBs)

- **AOption_X_TO_EVENT** (`Option::conversion::AOption_X_TO_EVENT`): wandelt die Option in `INIT_REQ`/`QI_OUT` und nimmt `INIT_CNF` entgegen
- **AX_SPLIT_3** (`adapter::events::unidirectional::AX_SPLIT_3`): Adapter-Fanout (strikt 1:1)
- **Background** (SubApp `MyLib::sys::GreenWhiteBackground3_AX`): Hintergrundfarbe von SoftKey und Aux; siehe [`GreenWhiteBackground3_AX`](./GreenWhiteBackground3_AX.md)
- **PUBLISH_STATE** (`adapter::net::AX_PUBLISH_1`): lokaler State-Publish
- **ObjPtr** (SubApp `MyLib::sys::AX_SwitchPic_2_1`): ObjectPointer-Statusanzeige; siehe [`AX_SwitchPic_2_1`](./AX_SwitchPic_2_1.md)

## Programmablauf und Verbindungen

1. Initialisierungskette: `AOption_X_TO_EVENT.INIT_REQ` -> `Background.INIT`; `Background.INITO` -> `PUBLISH_STATE.INIT`; `PUBLISH_STATE.INITO` -> `ObjPtr.INIT`; `ObjPtr.INITO` -> `AOption_X_TO_EVENT.INIT_CNF`.
2. Qualifier: `AOption_X_TO_EVENT.QI_OUT` -> `Background.QI`, `PUBLISH_STATE.QI` und `ObjPtr.QI`.
3. Option: `AOption_IN` -> `AOption_X_TO_EVENT.AOption_IN`.
4. AX-Zustand: `IN` -> `AX_SPLIT_3.IN`; `OUT1` -> `Background.DI1`; `OUT2` -> `PUBLISH_STATE.IN`; `OUT3` -> `ObjPtr.DI1`.
5. Daten: `u16ObjId` -> `Background.u16ObjId`; `u16ObjIdA` -> `Background.u16ObjIdA`; `ID_STATE_WRITE` -> `PUBLISH_STATE.ID`; `Picture` -> `ObjPtr.Picture`; `pictures` -> `ObjPtr.pictures`.
6. Der Baustein hat keine eigenen Ereignis- und `QI`/`QO`-Anschlüsse und keinen Plug; Aktivierung und Bestätigung laufen über `AOption_IN`.

## Technische Besonderheiten

- Der `AX_SPLIT_3` ist nötig, weil der Adapter-Fanout strikt 1:1 ist.
- Das `INIT_CNF` kommt echt vom Ende der Kette (`ObjPtr.INITO`), nicht aus einer Kurzschlussverbindung.
- Es gibt keinen `AOption_OUT`-Plug; der Baustein kann daher nicht daisy-chained werden.

## Anwendungsszenarien

- Reine Anzeigeseite eines Kanals (Hintergrundfarbe, State-Publish, Status-Bild), die nur bei ausgewählter Option aktiv ist, z. B. an einer `AOption_OR_2`-Verknüpfung mehrerer Varianten.

## Zusammenfassung

`AX_TO_BG_PUBLISH_SWITCHPIC_Option` zeigt einen fertigen AX-Zustand als Hintergrundfarbe, State-Publish und Status-Bild an und wird per `AOption` aktiviert.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
