# SoftKeyT_AE_AX

![SoftKeyT_AE_AX_network](./SoftKeyT_AE_AX_network.svg)

* * * * * * * * * *

## Einleitung

`SoftKeyT_AE_AX` buendelt 1 SoftKey (Trigger) und ein `GreenWhiteBackground1_AX` hinter einem einzigen `AE_AX`-Plug (bidirektional: Trigger geht raus, Zustand kommt rein). Reiner HMI-Baustein ohne jeden OPC-UA-Bezug - wiederverwendbar in jedem Netzwerk, das einen einzelnen Trigger-SoftKey mit Zustandsanzeige braucht.

![SoftKeyT_AE_AX](SoftKeyT_AE_AX.svg)

## Verwendete Funktionsbausteine (FBs)

- **SoftKey_UP_F1** (`isobus::UT::io::Softkey::Softkey_IE`, `InputEvent=SK_RELEASED`): physischer SoftKey.
- **AE_EVENT_TO_E** (`adapter::conversion::unidirectional::AE_EVENT_TO_E`): wandelt den SoftKey-Event in ein AE-Adapter-Ereignis.
- **GreenWhiteBackground_AX** (SubApp, Typ `MyLib::sys::GreenWhiteBackground1_AX`): Statusfarbe direkt auf demselben Objekt.
- **SPLIT** (`adapter::events::bidirectional::AE_TO_AE_AX_SPLIT`): verzweigt den bidirektionalen `AE_AX`-Verkehr auf den externen Plug `OUT` und intern auf das Background-Element.
- **AND_QO** (`iec61131::booleanOperators::AND_BOOL_2`): verknüpft die `QO`-Ausgänge der inneren Bausteine zu `QO`.

## Programmablauf und Verbindungen

`SoftKey_UP_F1.IND` -> `AE_EVENT_TO_E.REQ` -> `SPLIT.IN` -> `OUT` (nach aussen) und `SPLIT.AX_OUT` -> `GreenWhiteBackground_AX.DI1` (Zustandsanzeige).

## Initialisierung und Qualität

Schnittstelle (Quality-Muster): Ereignis-Eingang `INIT` und Ereignis-Ausgang `INITO` (je `EInit`), Dateneingang `QI` (`BOOL`, Vorgabe `TRUE`) und Datenausgang `QO` (`BOOL`). Initialisierung: `INIT` → `SoftKey_UP_F1.INIT`; `SoftKey_UP_F1.INITO` → `GreenWhiteBackground_AX.INIT`; `GreenWhiteBackground_AX.INITO` → `INITO` und → `AND_QO.REQ`. Qualität: `QI` → `SoftKey_UP_F1.QI`, `GreenWhiteBackground_AX.QI`; `SoftKey_UP_F1.QO`, `GreenWhiteBackground_AX.QO` → `AND_QO.IN1` bis `AND_QO.IN2` (in dieser Reihenfolge); `AND_QO.OUT` → `QO` (`AND_QO`: `iec61131::booleanOperators::AND_BOOL_2`).

## Zusammenfassung

Wiederverwendbarer Ein-Tasten-Trigger-mit-Statusanzeige-Baustein, gebuendelt hinter einem `AE_AX`-Adapter - das HMI-Gegenstueck zu den PC_A/PC_B-OPC-Netzwerkbausteinen wie [`SoftKeyT_PC_A_OPC_Adapter`](./SoftKeyT_PC_A_OPC_Adapter.md).

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
