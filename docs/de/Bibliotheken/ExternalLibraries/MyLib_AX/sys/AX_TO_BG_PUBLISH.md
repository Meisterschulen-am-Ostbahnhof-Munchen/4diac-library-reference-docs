# AX_TO_BG_PUBLISH

![AX_TO_BG_PUBLISH_network](./AX_TO_BG_PUBLISH_network.svg)

* * * * * * * * * *

## Einleitung

`AX_TO_BG_PUBLISH` bringt einen fertigen AX-Zustand auf die Hintergrundfarbe (SoftKey und AUX, `GreenWhiteBackground3_AXC`) und auf einen lokalen State-Publish. Er entspricht [`AX_TO_QXA_BG_PUBLISH`](./AX_TO_QXA_BG_PUBLISH.md), jedoch ohne physischen Ausgang. Die Initialisierung erfolgt per Auto-Verteilung (keine eigenen Ereignisanschlüsse). Der Baustein wurde aus `Baender_EinAus.SUB` (`Background_Alle`/`AX_PUBLISH_Alle`) als wiederverwendbarer Baustein herausgelöst.

![AX_TO_BG_PUBLISH](AX_TO_BG_PUBLISH.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Keine vorhanden.

### **Ereignis-Ausgänge**

Keine vorhanden.

### **Daten-Eingänge**

| Variable | Typ | Vorgabe | Kommentar |
|----------|-----|---------|-----------|
| `u16ObjId` | UINT | `ID_NULL` | Object ID SoftKey; trägt die Hintergrundfarbe. |
| `u16ObjIdA` | UINT | `ID_NULL` | Object ID AuxFunction2 (Joystick); zeigt die Hintergrundfarbe am Aux-Handle mit. |
| `ID_STATE_WRITE` | WSTRING |  | Lokale Publish-Adresse (`ACTION=WRITE`) für den Zustand; wird remote abonniert. |

### **Daten-Ausgänge**

Keine vorhanden.

### **Adapter**

| Adapter | Typ | Richtung | Kommentar |
|---------|-----|----------|-----------|
| `IN` | `adapter::types::unidirectional::AX` | Socket | Fertiger AX-Zustand; wird auf Hintergrund und Publish verteilt. |

## Verwendete Funktionsbausteine (FBs)

- **AX_SPLIT_2** (`adapter::events::unidirectional::AX_SPLIT_2`): Adapter-Fanout (strikt 1:1)
- **Background** (SubApp `MyLib::sys::GreenWhiteBackground3_AXC`): setzt die Hintergrundfarbe von SoftKey und Aux; siehe [`GreenWhiteBackground3_AXC`](./GreenWhiteBackground3_AXC.md)
- **PUBLISH_STATE** (`adapter::net::AX_PUBLISH_1`, `QI = TRUE`): lokaler State-Publish

## Programmablauf und Verbindungen

1. `IN` -> `AX_SPLIT_2.IN`.
2. `AX_SPLIT_2.OUT1` -> `Background.DI1`; `AX_SPLIT_2.OUT2` -> `PUBLISH_STATE.IN`.
3. `u16ObjId` -> `Background.u16ObjId`; `u16ObjIdA` -> `Background.u16ObjIdA`; `ID_STATE_WRITE` -> `PUBLISH_STATE.ID`.
4. Der Baustein hat keine Ereignis- und `QI`/`QO`-Anschlüsse; `PUBLISH_STATE.QI` ist fest auf `TRUE` parametriert.

## Technische Besonderheiten

- Der `AX_SPLIT_2` ist nötig, weil der Adapter-Fanout strikt 1:1 ist.
- Die Initialisierung läuft per Auto-Verteilung statt über einen eigenen `INIT`-Eingang.
- Gegenüber [`AX_TO_QXA_BG_PUBLISH`](./AX_TO_QXA_BG_PUBLISH.md) fehlen der physische Ausgang (`Output`) sowie `QI`/`QO` und `INIT`/`INITO`.

## Anwendungsszenarien

- Reine Anzeige- und Publish-Kanäle ohne eigenen Hardware-Ausgang, z. B. gleich aufgebaute Kanäle mit Hintergrundfarbe am SoftKey und Aux-Handle sowie Zustandsveröffentlichung.

## Zusammenfassung

`AX_TO_BG_PUBLISH` verteilt einen fertigen AX-Zustand auf die Hintergrundfarbe von SoftKey und Aux und einen lokalen State-Publish, ohne physischen Ausgang.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
