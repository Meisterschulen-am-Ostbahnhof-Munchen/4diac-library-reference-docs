# Button_Aux_IE

![Button_Aux_IE_network](./Button_Aux_IE_network.svg)

* * * * * * * * * *

## Einleitung

`Button_Aux_IE` fasst einen nicht-latchenden VT-Button **oder** eine AUX-Zuweisung (Joystick) zu einem gemeinsamen `IND`-Ereignis zusammen. Die Aktivierungsart ist fest verdrahtet (`BT_RELEASED_UNLATCHED` bzw. `AuxEnabled`). Der Baustein entspricht `Softkey_Aux_IE`, jedoch für einen echten VT-Button (Klasse `CButton`) statt eines SoftKeys: `Softkey_IE` und `Button_IE` sind zwei getrennte FORTE-Service-Interface-Bausteine für unterschiedliche VT-Activation-Messages, ein Button liefert nie ein `SK_PRESSED`.

Der Button **muss** non-latching sein (`Latchable=0` im Pool). Ein latchender Button kann sinnvoll nur über `Button_IXA` (Dauerzustand/Pegel) gelesen werden, nicht über dieses Event-/`InputEvent`-Muster.

![Button_Aux_IE](Button_Aux_IE.svg)

## Verwendete Funktionsbausteine (FBs)

### Sub-Bausteine: Button_Aux_IE

- **Typ**: SubAppType
- **Verwendete interne Bausteine**:
    - **Button**: SubApp [`Button_IE_1`](./Button_IE_1.md) mit `InputEvent=BT_RELEASED_UNLATCHED`
    - **Aux**: SubApp [`Aux_IE_1`](./Aux_IE_1.md) mit `InputEvent=AuxDisabled_START`
    - **MERGE**: `iec61499::events::E_MERGE_2` – führt beide `IND`-Ereignisse zusammen
    - **AND_QO**: `iec61131::booleanOperators::AND_BOOL_2` – verknüpft `Button.QO` und `Aux.QO` zum `QO`
- **Importe**: `BT_RELEASED_UNLATCHED` (`ButtonActivationCode`), `AuxDisabled_START` (`AuxiliaryState`)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **INIT** (`EInit`): Initialisierungsanforderung

### **Ereignis-Ausgänge**

- **INITO** (`EInit`): Initialisierungsbestätigung
- **IND**: Feuert, wenn der Button (`BT_RELEASED_UNLATCHED`) oder die Aux-Zuweisung (`AuxEnabled`) aktiviert wird

### **Daten-Eingänge**

- **QI** (`BOOL`, Vorgabe `TRUE`): Quality Input – TRUE gibt den Dienst frei
- **u16ObjId** (`UINT`): Objekt-ID des Buttons (`CButton`, non-latching)
- **u16ObjIdA** (`UINT`): Objekt-ID der `AuxFunction2`

### **Daten-Ausgänge**

- **QO** (`BOOL`): Quality Output – UND aus `Button.QO` und `Aux.QO`

## Programmablauf und Verbindungen

- `u16ObjId` → `Button.u16ObjId`
- `u16ObjIdA` → `Aux.u16ObjId`
- `Button.IND` → `MERGE.EI1`
- `Aux.IND` → `MERGE.EI2`
- `MERGE.EO` → `IND`
- `INIT` → `Button.INIT`; `Button.INITO` → `Aux.INIT`; `Aux.INITO` → `INITO` (und → `AND_QO.REQ`)
- `QI` → `Button.QI` und `QI` → `Aux.QI`
- `Button.QO` → `AND_QO.IN1`; `Aux.QO` → `AND_QO.IN2`; `AND_QO.OUT` → `QO`

## Technische Besonderheiten

- Bugfix für `Automatik_Toggle_Button`: dort wurde zuvor `Softkey_Aux_IE` mit einer Button-Objekt-ID verwendet, `Softkey_IE` empfängt aber keine Button-Activation-Message (bestätigt vom Autor der FORTE-Implementierung)
- Der `E_MERGE_2` leitet das Ereignis weiter, welche der beiden Quellen auch feuert
- Version 1.0 (2026-09-20)

## Anwendungsszenarien

- Ein Bedienelement, das wahlweise über einen VT-Button oder einen AUX-Joystick-Eingang ausgelöst werden soll
- Toggle-Logik (z. B. `Automatik_Toggle_Button`) mit zwei gleichwertigen Auslösern

## Zusammenfassung

`Button_Aux_IE` kombiniert `Button_IE_1` und `Aux_IE_1` über einen `E_MERGE_2` zu einem einzigen `IND`-Ereignis für Button und AUX.
