# Softkey_Aux_IE

![Softkey_Aux_IE_network](./Softkey_Aux_IE_network.svg)

* * * * * * * * * *

## Einleitung

`Softkey_Aux_IE` löst ein gemeinsames `IND`-Ereignis aus, wenn ein **SoftKey** ODER eine **AUX-Zuweisung (Joystick)** aktiviert wird. `SK_PRESSED` bzw. `AuxEnabled` sind fest verdrahtet, da dies die einzig sinnvolle "aktiviert"-Bedeutung für eine Merge-Anwendung ist. Der Baustein ist analog zu [`Softkey_Aux_IXA_TO_Remote_WRITE`](./Softkey_Aux_IXA_TO_Remote_WRITE.md) (Dauerzustand-ODER), aber für den diskreten Event-Kanal. Er kollidiert daher nicht mit einer bereits bestehenden `Softkey_IXA`/`Aux_IXA`-Dauerzustand-Abonnierung auf demselben Objekt (zwei verschiedene Subscription-Typen).

![Softkey_Aux_IE](Softkey_Aux_IE.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Keine.

### **Ereignis-Ausgänge**

- **IND**: feuert, wenn der SoftKey (`SK_PRESSED`) ODER Aux (`AuxEnabled`) aktiviert wird

### **Daten-Eingänge**

- **u16ObjId** (`UINT`): Object ID SoftKey
- **u16ObjIdA** (`UINT`): Object ID AuxFunction2

### **Daten-Ausgänge**

Keine.

### **Adapter**

Keine.

## Verwendete Funktionsbausteine (FBs)

- **`Softkey`** (SubApp `MyLib::sys::Softkey_IE_1`, [Softkey_IE_1](./Softkey_IE_1.md)): Parameter `InputEvent = SK_PRESSED`
- **`Aux`** (SubApp `MyLib::sys::Aux_IE_1`, [Aux_IE_1](./Aux_IE_1.md)): Parameter `InputEvent = AuxEnabled`
- **`MERGE`** (`iec61499::events::E_MERGE_2`): führt beide Ereignisse zusammen

## Funktionsweise

- `u16ObjId` → `Softkey.u16ObjId`; `u16ObjIdA` → `Aux.u16ObjId`
- `Softkey.IND` → `MERGE.EI1`; `Aux.IND` → `MERGE.EI2`
- `MERGE.EO` → `IND`

## Technische Besonderheiten

- Importiert `isobus::UT::io::Softkey::SoftKeyActivationCode::SK_PRESSED` und `isobus::UT::io::Auxiliary::IN::AuxiliaryState::AuxEnabled`
- Arbeitet über den diskreten Event-Kanal, nicht über den Dauerzustand

## Anwendungsszenarien

- Ein Ereignis aus SoftKey oder Joystick-Zuweisung, z. B. für Toggle- oder Trigger-Funktionen

## Zusammenfassung

`Softkey_Aux_IE` vereint SoftKey- und AUX-Aktivierung zu einem gemeinsamen `IND`-Ereignis.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
