# SoftKeySRT_ASRT_AX

![SoftKeySRT_ASRT_AX_network](./SoftKeySRT_ASRT_AX_network.svg)

* * * * * * * * * *

## Einleitung

`SoftKeySRT_ASRT_AX` buendelt 3 SoftKeys (Set/Reset/Toggle) und ein `GreenWhiteBackground1_AX` (am Toggle-SoftKey) hinter einem einzigen `ASRT_AX`-Plug (bidirektional: Set/Reset/Toggle gehen raus, Zustand kommt rein). Reiner HMI-Baustein ohne jeden OPC-UA-Bezug - das SR+Toggle-Gegenstueck zu [`SoftKeySR_ASR_AX`](./SoftKeySR_ASR_AX.md).

![SoftKeySRT_ASRT_AX](SoftKeySRT_ASRT_AX.svg)

## Verwendete Funktionsbausteine (FBs)

- **SoftKey_SET / SoftKey_RESET / SoftKey_TOGGLE** (`isobus::UT::io::Softkey::Softkey_IE`, `InputEvent=SK_RELEASED`): die 3 physischen SoftKeys.
- **ASRT_3EVENTS_TO_SRT** (`adapter::conversion::unidirectional::ASRT_3EVENTS_TO_SRT`): wandelt die 3 SoftKey-Events in ein ASRT-Adapter-Ereignistripel.
- **SPLIT** (`adapter::events::bidirectional::ASRT_TO_ASRT_AX_SPLIT`): verzweigt den bidirektionalen `ASRT_AX`-Verkehr auf `OUT` und auf das Background-Element.
- **GreenWhiteBackground_AX** (SubApp, Typ `MyLib::sys::GreenWhiteBackground1_AX`): Statusfarbe am Toggle-SoftKey.
- **AND_QO** (`iec61131::booleanOperators::AND_BOOL_4`): verknüpft die `QO`-Ausgänge der inneren Bausteine zu `QO`.

## Initialisierung und Qualität

Schnittstelle (Quality-Muster): Ereignis-Eingang `INIT` und Ereignis-Ausgang `INITO` (je `EInit`), Dateneingang `QI` (`BOOL`, Vorgabe `TRUE`) und Datenausgang `QO` (`BOOL`). Initialisierung: `INIT` → `SoftKey_SET.INIT`; `SoftKey_SET.INITO` → `SoftKey_RESET.INIT`; `SoftKey_RESET.INITO` → `SoftKey_TOGGLE.INIT`; `SoftKey_TOGGLE.INITO` → `GreenWhiteBackground_AX.INIT`; `GreenWhiteBackground_AX.INITO` → `INITO` und → `AND_QO.REQ`. Qualität: `QI` → `SoftKey_SET.QI`, `SoftKey_RESET.QI`, `SoftKey_TOGGLE.QI`, `GreenWhiteBackground_AX.QI`; `SoftKey_SET.QO`, `SoftKey_RESET.QO`, `SoftKey_TOGGLE.QO`, `GreenWhiteBackground_AX.QO` → `AND_QO.IN1` bis `AND_QO.IN4` (in dieser Reihenfolge); `AND_QO.OUT` → `QO` (`AND_QO`: `iec61131::booleanOperators::AND_BOOL_4`).

## Zusammenfassung

Wiederverwendbarer Set/Reset/Toggle-mit-Statusanzeige-Baustein, gebuendelt hinter einem `ASRT_AX`-Adapter - das HMI-Gegenstueck zu [`Uebung_010e_PC_A_OPC_Adapter`](./Uebung_010e_PC_A_OPC_Adapter.md)/[`Uebung_010e_PC_B_OPC_Adapter`](./Uebung_010e_PC_B_OPC_Adapter.md).

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
