# ObjectPointer_Swap_AX

![ObjectPointer_Swap_AX_network](./ObjectPointer_Swap_AX_network.svg)

* * * * * * * * * *

## Einleitung

`ObjectPointer_Swap_AX` schaltet einen ObjectPointer (CPointer) im VT um: Bei `DI1 = TRUE` zeigt er auf `u16ObjId_Show`, bei FALSE auf `ID_NULL` (nichts sichtbar). Die Umschaltung nutzt ISO 11783-6 F.22 "Change Numeric Value" (`Q_NumericValue`); der ObjectPointer liegt dabei explizit in dessen gültigem Object-ID-Bereich (27000-27999). F.38 "Change Attribute" (`Q_Attribute`) darf nicht verwendet werden, weil `AID_OP_PTR.VALUE` dort schreibgeschützt ist.

![ObjectPointer_Swap_AX](ObjectPointer_Swap_AX.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Der Funktionsblock verfügt über keine direkten Ereignis-Eingänge.

### **Ereignis-Ausgänge**

- **CNF**: Execution Confirmation, durchgereicht von `Q_NumericValue.CNF`

### **Daten-Eingänge**

- **u16ObjId_Pointer** (UINT): Object ID des ObjectPointers (CPointer), dessen Ziel geschaltet wird (Default `ID_NULL`)
- **u16ObjId_Show** (UINT): Object ID, die bei `DI1 = TRUE` unter dem Pointer angezeigt wird (Default `ID_NULL`)

### **Daten-Ausgänge**

- **STATUS_1** (STRING): Service-Status von `Q_NumericValue`
- **u32OldValue_1** (UDINT): alter Wert der ID
- **result_1** (INT): Rückgabewert (siehe `Q_NumericValue`)

### **Adapter**

- **DI1** (Socket, `adapter::types::unidirectional::AX`): Selektor

## Funktionsweise

Das Netzwerk besteht aus zwei Bausteinen:

- `AX_SEL` (`adapter::iec61131::selection::AX_SEL`): Binäre Auswahl mit `IN0 = ID_NULL` (Parameter), `IN1` = `u16ObjId_Show` und Selektor `G` = `DI1`. Ein Ausgangsereignis entsteht nur, wenn sich `G` ändert.
- `Q_NumericValue` (`isobus::UT::Q::Q_NumericValue`): `AX_SEL.OUT` → `u32NewValue`, `u16ObjId_Pointer` → `u16ObjId`, ausgelöst durch `AX_SEL.CNF` → `Q_NumericValue.REQ`. `Q_NumericValue.CNF` → `CNF`; `STATUS`, `u32OldValue` und `s16result` werden als `STATUS_1`, `u32OldValue_1` und `result_1` ausgegeben.

Ändert sich `DI1`, wird so der neue Zielwert (`u16ObjId_Show` oder `ID_NULL`) als numerischer Wert an den ObjectPointer geschrieben.

## Technische Besonderheiten

- Aufbau analog zu `GreenWhiteBackground1_AX` (AX_SEL-Wrapper um einen `Q_`-FB)
- `Q_NumericValue` (F.22) ist der richtige Weg für ObjectPointer; `Q_Attribute` (F.38) wurde verworfen
- Muster siehe Übung `Uebung_015` im Workspace `test_B`

## Anwendungsszenarien

- Ein- und Ausblenden eines VT-Objekts (z. B. Warnsymbol) an einem festen Platz über einen AX-Zustand
- Umschalten zwischen einem leeren Zustand und einem Inhalt per ObjectPointer

## Fazit

`ObjectPointer_Swap_AX` übersetzt einen AX-Zustand in eine ObjectPointer-Umschaltung zwischen `u16ObjId_Show` und `ID_NULL` über `Q_NumericValue`.
