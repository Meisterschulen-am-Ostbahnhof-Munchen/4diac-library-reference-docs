# AX_TO_QXA_Option

![AX_TO_QXA_Option_network](./AX_TO_QXA_Option_network.svg)

* * * * * * * * * *

## Einleitung

`AX_TO_QXA_Option` ist ein reiner physischer Ausgang: Er bringt einen fertigen AX-Zustand auf `logiBUS_QXA`. Aktiviert wird er per `AOption` (`INIT`/`QI` über `AOption_X_TO_EVENT`, `INIT_CNF` echt von `DigitalOutput.INITO`). Er ist der Ausgangs-Teil von [`AX_TO_QXA_BG_PUBLISH_SWITCHPIC_OPT`](./AX_TO_QXA_BG_PUBLISH_SWITCHPIC_OPT.md) als eigener Baustein, nach dem Muster von `OptionalGroup_001`.

![AX_TO_QXA_Option](AX_TO_QXA_Option.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Keine vorhanden.

### **Ereignis-Ausgänge**

Keine vorhanden.

### **Daten-Eingänge**

| Variable | Typ | Vorgabe | Kommentar |
|----------|-----|---------|-----------|
| `Output` | `logiBUS::io::DQ::logiBUS_DO_S` | `logiBUS_DO::Invalid` | Auswahl des Ausgangs `Output_Q1`..`Output_Q12`. |

### **Daten-Ausgänge**

Keine vorhanden.

### **Adapter**

| Adapter | Typ | Richtung | Kommentar |
|---------|-----|----------|-----------|
| `AOption_IN` | `Option::AOption` | Socket | Aktiviert/deaktiviert den Ausgang. |
| `IN` | `adapter::types::unidirectional::AX` | Socket | Fertiger AX-Zustand für den Ausgang. |

## Verwendete Funktionsbausteine (FBs)

- **AOption_X_TO_EVENT** (`Option::conversion::AOption_X_TO_EVENT`): wandelt die Option in `INIT_REQ`/`QI_OUT` und nimmt `INIT_CNF` entgegen
- **DigitalOutput** (`logiBUS::io::DQ::logiBUS_QXA`): physischer Ausgang

## Programmablauf und Verbindungen

1. Option: `AOption_IN` -> `AOption_X_TO_EVENT.AOption_IN`.
2. Initialisierung: `AOption_X_TO_EVENT.INIT_REQ` -> `DigitalOutput.INIT`; `DigitalOutput.INITO` -> `AOption_X_TO_EVENT.INIT_CNF`.
3. Qualifier: `AOption_X_TO_EVENT.QI_OUT` -> `DigitalOutput.QI`.
4. AX-Zustand: `IN` -> `DigitalOutput.OUT`.
5. Daten: `Output` -> `DigitalOutput.Output`.
6. Der Baustein hat keine eigenen Ereignis- und `QI`/`QO`-Anschlüsse und keinen Plug.

## Technische Besonderheiten

- Keine eigene Logik: nur Option-Anbindung und `logiBUS_QXA`.
- Das `INIT_CNF` kommt echt von `DigitalOutput.INITO`.
- Es gibt keinen `AOption_OUT`-Plug; der Baustein kann daher nicht daisy-chained werden.

## Anwendungsszenarien

- Physischer Ausgang, der nur bei ausgewählter Option (z. B. Klassisch) geschaltet werden soll, getrennt von Anzeige und Publish.

## Zusammenfassung

`AX_TO_QXA_Option` schaltet einen physischen Ausgang aus einem fertigen AX-Zustand und wird per `AOption` aktiviert.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
