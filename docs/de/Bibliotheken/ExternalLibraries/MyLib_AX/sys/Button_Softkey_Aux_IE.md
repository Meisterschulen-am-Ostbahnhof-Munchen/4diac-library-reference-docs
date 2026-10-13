# Button_Softkey_Aux_IE

![Button_Softkey_Aux_IE_network](./Button_Softkey_Aux_IE_network.svg)

* * * * * * * * * *

## Einleitung

`Button_Softkey_Aux_IE` fasst einen nicht-latchenden VT-Button **oder** einen SoftKey **oder** eine AUX-Zuweisung (Joystick) zu einem gemeinsamen `IND`-Ereignis zusammen. Die Aktivierungsarten sind fest verdrahtet (`BT_RELEASED_UNLATCHED`, `SK_RELEASED` bzw. `AuxEnabled`). Der Baustein entspricht `Button_Aux_IE` und `Softkey_Aux_IE`, bedient aber alle drei Quellen gleichzeitig. `Softkey_IE` und `Button_IE` sind getrennte FORTE-Service-Interface-Bausteine für unterschiedliche VT-Activation-Messages; ein Button liefert nie ein `SK_PRESSED`.

Der Button **muss** non-latching sein (`Latchable=0` im Pool); ein latchender Button ist sinnvoll nur über `Button_IXA` (Pegel) lesbar.

![Button_Softkey_Aux_IE](Button_Softkey_Aux_IE.svg)

## Verwendete Funktionsbausteine (FBs)

### Sub-Bausteine: Button_Softkey_Aux_IE

- **Typ**: SubAppType
- **Verwendete interne Bausteine**:
    - **Button**: SubApp [`Button_IE_1`](./Button_IE_1.md) mit `InputEvent=BT_RELEASED_UNLATCHED`
    - **Softkey**: SubApp [`Softkey_IE_1`](./Softkey_IE_1.md) mit `InputEvent=SK_RELEASED`
    - **Aux**: SubApp [`Aux_IE_1`](./Aux_IE_1.md) mit `InputEvent=AuxDisabled_START`
    - **MERGE**: `iec61499::events::E_MERGE_3` – führt die drei `IND`-Ereignisse zusammen
- **Importe**: `BT_RELEASED_UNLATCHED`, `AuxDisabled_START`, `SK_RELEASED`

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Keine.

### **Ereignis-Ausgänge**

- **IND**: Feuert, wenn Button (`BT_RELEASED_UNLATCHED`), Softkey (`SK_RELEASED`) oder Aux (`AuxEnabled`) aktiviert wird

### **Daten-Eingänge**

- **u16ObjId** (`UINT`): Objekt-ID des Softkeys (`CSoftkey`, non-latching)
- **u16ObjIdA** (`UINT`): Objekt-ID der `AuxFunction2`
- **u16ObjIdB** (`UINT`): Objekt-ID des Buttons (`CButton`, non-latching)

### **Daten-Ausgänge**

Keine.

## Programmablauf und Verbindungen

- `u16ObjId` → `Softkey.u16ObjId`; `Softkey.IND` → `MERGE.EI1`
- `u16ObjIdA` → `Aux.u16ObjId`; `Aux.IND` → `MERGE.EI2`
- `u16ObjIdB` → `Button.u16ObjId`; `Button.IND` → `MERGE.EI3`
- `MERGE.EO` → `IND`

## Technische Besonderheiten

- Version 1.1 (2026-09-29): `packageName`-Tippfehler behoben (`MyLib_AX::sys` → `MyLib::sys`), Instanz `Softkey_IE_1` in `Softkey` umbenannt, Kommentar korrigiert (beschrieb nur 2 von 3 Quellen)
- Version 1.0 (2026-09-20): Bugfix für `Automatik_Toggle_Button`, der bisher `Softkey_Aux_IE` mit einer Button-Objekt-ID nutzte

## Anwendungsszenarien

- Ein Bedienelement, das wahlweise über Button, SoftKey oder AUX-Eingang ausgelöst werden kann

## Zusammenfassung

`Button_Softkey_Aux_IE` bündelt `Button_IE_1`, `Softkey_IE_1` und `Aux_IE_1` über einen `E_MERGE_3` zu einem einzigen `IND`-Ereignis.
