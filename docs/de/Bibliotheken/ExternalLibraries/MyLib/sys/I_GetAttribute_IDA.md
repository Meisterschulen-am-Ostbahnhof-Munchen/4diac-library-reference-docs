# I_GetAttribute_IDA

## Einleitung

Die SubApp `I_GetAttribute_IDA` bündelt das ISO 11783-6 GetAttribute-Kommando (`I_GetAttribute`, F.58) und den asynchronen Adapter-Empfänger (`Attribute_IDA`) in einer wiederverwendbaren SubApp-Einheit (`MyLib::sys`). Sie exponiert `u16ObjId` und `u8AID` jeweils einmalig als Eingangs-Parameter und stellt die empfangenen Attributdaten über den Adapter-Plug `IN` bereit.

## Schnittstellenstruktur

### **SubAppEventInputs**

- `REQ`: Anforderung der Attribut-Abfrage (sendet Kommando F.58 an das VT)

### **InputVars**

- `u16ObjId` (UINT): Objekt-ID des abzufragenden VT-Objekts (z. B. `InputNumber_I1`)
- `u8AID` (USINT): Attribut-ID (z. B. `AID_IN.VALUE`)

### **Plugs**

- `IN` (`adapter::types::unidirectional::AD`): Asynchron empfangene Attributdaten als Adapter-Plug

## Funktionsweise

1. Bei Ankunft von `REQ` sendet der interne Baustein `GetAttribute` das GetAttribute-Kommando (F.58) an das VT.
2. Sobald das VT asynchron mit dem Attributwert antwortet, empfängt `Attribute_IDA` das Ergebnis und leitet es direkt über den Adapter-Plug `IN` (z. B. weiter an `AD_TO_AR_NUM`) weiter.
