# ButtonT_AE_AX

![ButtonT_AE_AX_network](./ButtonT_AE_AX_network.svg)

* * * * * * * * * *

## Einleitung

`ButtonT_AE_AX` bündelt **einen Button (Trigger)** und eine `GreenWhiteBackground` hinter einem einzigen bidirektionalen `AE_AX`-Plug (Trigger raus, Zustand rein). Es ist der reine HMI-Teil ohne jeden OPC-UA-Bezug und wiederverwendbar. Der Baustein ist das Button-Pendant zu [`SoftKeyT_AE_AX`](./SoftKeyT_AE_AX.md): `Softkey_IE` wurde durch `Button_IE` ersetzt, `SK_RELEASED` durch `BT_PRESSED_LATCHED`.

![ButtonT_AE_AX](ButtonT_AE_AX.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **INIT** (`EInit`): Initialisierungsanforderung; geht an `Button_UP_F1.INIT`

### **Ereignis-Ausgänge**

- **INITO** (`EInit`): Initialisierung abgeschlossen; kommt von `GreenWhiteBackground_AX.INITO`

### **Daten-Eingänge**

- **QI** (`BOOL`): Quality Input - TRUE aktiviert den Dienst (Default `TRUE`); geht an `Button_UP_F1.QI` und `GreenWhiteBackground_AX.QI`
- **u16ObjId** (`UINT`): Object ID Button/Background (Default `ID_NULL`)

### **Daten-Ausgänge**

- **QO** (`BOOL`): Quality Output - TRUE, wenn der Dienst erfolgreich ausgeführt wurde (AND aus `Button_UP_F1.QO` und `GreenWhiteBackground_AX.QO`)

### **Adapter**

- **OUT** (Plug, `adapter::types::bidirectional::AE_AX`): Trigger (raus), Zustand (rein)

## Verwendete Funktionsbausteine (FBs)

- **`Button_UP_F1`** (`isobus::UT::io::Button::Button_IE`): Parameter `InputEvent = BT_PRESSED_LATCHED` (`QI` kommt von außen)
- **`AE_EVENT_TO_E`** (`adapter::conversion::unidirectional::AE_EVENT_TO_E`): wandelt das Button-Ereignis in einen `AE`-Adapter
- **`SPLIT`** (`adapter::events::bidirectional::AE_TO_AE_AX_SPLIT`): trennt den bidirektionalen `AE_AX`-Plug in Trigger-Richtung und Zustands-Richtung
- **`GreenWhiteBackground_AX`** (SubApp `MyLib::sys::GreenWhiteBackground1_AX`): Hintergrundfarbe grün/weiß
- **`AND_QO`** (`iec61131::booleanOperators::AND_BOOL_2`): verknüpft die `QO` von `Button_UP_F1` und `GreenWhiteBackground_AX`

## Funktionsweise

- `Button_UP_F1.IND` → `AE_EVENT_TO_E.REQ`
- `AE_EVENT_TO_E.AE_OUT` → `SPLIT.IN`
- `SPLIT.OUT` → `OUT` (Trigger nach außen, Zustand von außen)
- `SPLIT.AX_OUT` → `GreenWhiteBackground_AX.DI1` (Zustand färbt den Hintergrund)
- `u16ObjId` → `Button_UP_F1.u16ObjId` und `GreenWhiteBackground_AX.u16ObjId`
- Initialisierung: `INIT` → `Button_UP_F1.INIT`, `Button_UP_F1.INITO` → `GreenWhiteBackground_AX.INIT`, `GreenWhiteBackground_AX.INITO` → `INITO` und → `AND_QO.REQ`
- Qualität: `QI` → `Button_UP_F1.QI` und `GreenWhiteBackground_AX.QI`; `Button_UP_F1.QO` → `AND_QO.IN1`, `GreenWhiteBackground_AX.QO` → `AND_QO.IN2`, `AND_QO.OUT` → `QO`

## Technische Besonderheiten

- Dieselbe Objekt-ID dient dem Button und der Hintergrundfarbe
- `BT_PRESSED_LATCHED` statt `SK_RELEASED` des Softkey-Pendants
- Kein OPC-UA-Bezug, damit in beliebigen Szenarien wiederverwendbar

## Anwendungsszenarien

- Button im VT als Trigger, dessen Zustand (z. B. von einem Toggle) als Hintergrundfarbe zurückgemeldet wird, über einen einzigen `AE_AX`-Anschluss

## Zusammenfassung

`ButtonT_AE_AX` bündelt Button-Trigger und Zustandsfarbe hinter einem `AE_AX`-Plug.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
