# AX_TO_QXA_BG_PUBLISH_SWITCHPIC

![AX_TO_QXA_BG_PUBLISH_SWITCHPIC_network](./AX_TO_QXA_BG_PUBLISH_SWITCHPIC_network.svg)

* * * * * * * * * *

## Einleitung

`AX_TO_QXA_BG_PUBLISH_SWITCHPIC` speist [`AX_TO_QXA_BG_PUBLISH`](./AX_TO_QXA_BG_PUBLISH.md) (Ausgang + Hintergrund + Publish) **und** [`AX_SwitchPic_2_1`](./AX_SwitchPic_2_1.md) (ObjectPointer-Statusanzeige) aus demselben fertigen AX-Zustand. Ein `AX_SPLIT_2` davor ist nötig, da beide Sub-Bausteine den Zustand brauchen (Adapter-Fanout-Regel). Herausgelöst aus `Baender_EinAus`, wo diese Kombination dreimal identisch (Igel, Elevator, Querband) vorhanden war.

![AX_TO_QXA_BG_PUBLISH_SWITCHPIC](AX_TO_QXA_BG_PUBLISH_SWITCHPIC.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **INIT** (`EInit`): Initialisierung; wird an `Publish.INIT` durchgereicht statt per Auto-Verteilung

### **Ereignis-Ausgänge**

Keine.

### **Daten-Eingänge**

- **QI** (`BOOL`): Qualifier für den physischen Ausgang; FALSE hält diese Instanz komplett inaktiv (Default `TRUE`)
- **u16ObjId** (`UINT`): Object ID des SoftKeys, trägt die Hintergrundfarbe (Default `ID_NULL`)
- **u16ObjIdA** (`UINT`): Object ID der AuxFunction2 (Joystick), zeigt die Hintergrundfarbe am Aux-Handle mit (Default `ID_NULL`)
- **Output** (`logiBUS::io::DQ::logiBUS_DO_S`): Auswahl des Ausgangs `Output_Q1`..`Output_Q12` (Default `logiBUS_DO::Invalid`)
- **ID_STATE_WRITE** (`WSTRING`): lokale Publish-Adresse (`ACTION=WRITE`) für den tatsächlichen Zustand, wird von Web-Client/Dashboard remote abonniert
- **Picture** (`UINT`): Object ID des ObjectPointers, der den Status-Kästchen-Inhalt trägt
- **pictures** (`isobus::UT::Q::types::s2x1ObjectIDs`): die beiden Ziel-Object-IDs (`up` = aus, `down` = an); Default beide `ID_NULL`

### **Daten-Ausgänge**

Keine.

### **Adapter**

- **IN** (Socket, `adapter::types::unidirectional::AX`): fertiger, bereits zusammengeführter AX-Zustand, wird auf Ausgang + Hintergrund + Publish und die ObjectPointer-Statusanzeige verteilt

## Verwendete Funktionsbausteine (FBs)

- **`AX_SPLIT_2`** (`adapter::events::unidirectional::AX_SPLIT_2`)
- **`Publish`** (SubApp `MyLib::sys::AX_TO_QXA_BG_PUBLISH`)
- **`ObjPtr`** (SubApp `MyLib::sys::AX_SwitchPic_2_1`)

## Funktionsweise

- `IN` → `AX_SPLIT_2.IN`
- `AX_SPLIT_2.OUT1` → `Publish.IN`; `u16ObjId`, `u16ObjIdA`, `Output`, `ID_STATE_WRITE`, `QI` → gleichnamige Eingänge von `Publish`
- `INIT` → `Publish.INIT`
- `AX_SPLIT_2.OUT2` → `ObjPtr.DI1`; `Picture` → `ObjPtr.Picture`, `pictures` → `ObjPtr.pictures`

## Technische Besonderheiten

- Der `AX_SPLIT_2` ist notwendig, weil ein Adapter nicht auf zwei Sub-Bausteine verzweigen darf
- Keine eigene Logik, nur Zusammenfassung zweier vorhandener SubApps
- Version 1.1: `QI` und `INIT` werden an `Publish` (`AX_TO_QXA_BG_PUBLISH` v1.1) durchgereicht (QI-Gate Klassisch-vs-PVEA, Initialisierung vom Aufrufer sequenziert)

## Anwendungsszenarien

- Mehrere gleich aufgebaute Kanäle (z. B. Förderbänder), die je einen Ausgang schalten, Softkey-Farbe zeigen, publizieren und zusätzlich eine Status-Bildanzeige umschalten

## Zusammenfassung

`AX_TO_QXA_BG_PUBLISH_SWITCHPIC` verteilt einen fertigen AX-Zustand auf Ausgang/Hintergrund/Publish und eine ObjectPointer-Statusanzeige.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
