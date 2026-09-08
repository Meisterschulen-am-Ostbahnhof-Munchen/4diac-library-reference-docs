# Button_IXA_TO_logiBUS_QXA_BG_OPC_LATCHING

![Button_IXA_TO_logiBUS_QXA_BG_OPC_LATCHING_network](./Button_IXA_TO_logiBUS_QXA_BG_OPC_LATCHING_network.svg)

* * * * * * * * * *

## Einleitung

`Button_IXA_TO_logiBUS_QXA_BG_OPC_LATCHING` ersetzt bei [`Button_IXA_TO_logiBUS_QXA_BG_OPC`](./Button_IXA_TO_logiBUS_QXA_BG_OPC.md) die einfache ODER-Verknüpfung (`AX_OR_2`) durch eine Flankenerkennungs-/Merge-/Latch-Kette (`AX_ASR_RF_TRIG` × 2, `ASR_MERGE_2`, `ASR_AX_SR`). Das ist **kein** echtes Klick-Toggle: `AX_ASR_RF_TRIG` bildet die steigende Flanke (Drücken) auf `SET` und die fallende Flanke (Loslassen) auf `RESET` ab, `ASR_AX_SR` setzt/rücksetzt entsprechend — bei nur einer aktiven Quelle verhält sich der Baustein also tastend, genau wie die `AX_OR_2`-Variante (EIN nur solange gedrückt). Der Unterschied zeigt sich erst, wenn sich VT-Taste und OPC-UA-Kommando zeitlich überlappen: Anders als bei einer echten ODER-Verknüpfung (die EIN bleibt, solange irgendeine Quelle aktiv ist) gewinnt hier immer die zuletzt eingetroffene Flanke, unabhängig davon, von welcher Quelle sie stammt — löst z. B. die OPC-UA-Seite ein Loslassen aus, während die VT-Taste noch gehalten wird, schaltet der Ausgang trotzdem sofort AUS. Der Baustein ist aufbewahrt für Anwendungsfälle, in denen genau dieses Last-Wins-Verhalten zwischen zwei Quellen gebraucht wird. Beide Bausteine teilen dasselbe Interface (`u16ObjId`/`Output`/`ID_READ`/`ID_WRITE`) und sind schnittstellenkompatibel — ihr Schaltverhalten unterscheidet sich jedoch in genau diesem Überlappungsfall.

## Verwendete Funktionsbausteine (FBs)

### Sub-Bausteine: Button_IXA_TO_logiBUS_QXA_BG_OPC_LATCHING

- **Typ**: SubAppType
- **Verwendete interne FBs**:
    - **Button_IXA**: `isobus::UT::io::Button::Button_IXA` (`QI=TRUE`) — VT-Taster, `u16ObjId` identifiziert die VT-Taste.
    - **AX_ASR_RF_TRIG_BT** / **AX_ASR_RF_TRIG_OPC**: je `adapter::events::unidirectional::AX_ASR_RF_TRIG` — wandeln steigende/fallende Flanke der VT-Taste bzw. des OPC-UA-Subscribe-Werts in ein Set/Reset-Ereignispaar (ASR) um.
    - **ASR_MERGE_2**: `adapter::events::unidirectional::ASR_MERGE_2` — führt die beiden ASR-Quellen (Button, OPC-UA) zusammen.
    - **ASR_AX_SR**: `adapter::events::unidirectional::ASR_AX_SR` — Set/Reset-Latch, das aus dem zusammengeführten ASR-Signal den gelatchten Ausgangszustand bildet (Last-Wins).
    - **AX_SPLIT_3**: `adapter::events::unidirectional::AX_SPLIT_3` — verteilt den Latch-Ausgang auf drei Ziele.
    - **logiBUS_QXA**: `logiBUS::io::DQ::logiBUS_QXA` (`QI=TRUE`) — physischer digitaler Ausgang.
    - **GreenWhiteBackground1_AX** (SubApp, `MyLib::sys`): VT-Hintergrundfarbe passend zum Ausgangszustand.
    - **AX_SUBSCRIBE_1** / **AX_PUBLISH_1**: je `adapter::net::AX_SUBSCRIBE_1`/`AX_PUBLISH_1` (`QI=TRUE`) — OPC-UA-Lese-/Schreibzugriff.
- **Funktionsweise**: Beide Schaltquellen (VT-Taster, OPC-UA-Kommando) werden je über `AX_ASR_RF_TRIG` in Set/Reset-Ereignisse gewandelt, per `ASR_MERGE_2` zusammengeführt und in `ASR_AX_SR` gelatcht — der Latch-Zustand speist gleichzeitig Hardware, VT-Statusfarbe und OPC-UA-Echo.

## Programmablauf und Verbindungen

1. `Button_IXA.IN` (Adapter) → `AX_ASR_RF_TRIG_BT.QI` → `AX_ASR_RF_TRIG_BT.Q` → `ASR_MERGE_2.IN2`.
2. `AX_SUBSCRIBE_1.OUT` → `AX_ASR_RF_TRIG_OPC.QI` → `AX_ASR_RF_TRIG_OPC.Q` → `ASR_MERGE_2.IN1`.
3. `ASR_MERGE_2.OUT` → `ASR_AX_SR.S_R` (Set/Reset-Eingang des Latch).
4. `ASR_AX_SR.Q` → `AX_SPLIT_3.IN` → `AX_SPLIT_3.OUT1` → `logiBUS_QXA.OUT`, `AX_SPLIT_3.OUT2` → `GreenWhiteBackground1_AX.DI1`, `AX_SPLIT_3.OUT3` → `AX_PUBLISH_1.IN`.
5. Initialisierung: `AX_SUBSCRIBE_1.INITO` → `AX_PUBLISH_1.INIT` (ausgeblendete Verbindung).
6. Parameter: `u16ObjId` → `Button_IXA.u16ObjId` und `GreenWhiteBackground1_AX.u16ObjId`; `Output` → `logiBUS_QXA.Output`; `ID_READ` → `AX_SUBSCRIBE_1.ID`; `ID_WRITE` → `AX_PUBLISH_1.ID`.

## Technische Besonderheiten

- **Last-Wins über ASR-Latch, kein echtes Toggle**: Jede Betätigung erzeugt je Quelle ein Set- (Drücken) bzw. Reset-Ereignis (Loslassen); `ASR_AX_SR` latcht daraus den gemeinsamen Zustand. Bei nur einer aktiven Quelle ist das Verhalten rein tastend (EIN nur solange gedrückt) — identisch zur `AX_OR_2`-Variante. Erst wenn sich beide Quellen überlappen, zeigt sich der Unterschied: die zuletzt eingetroffene Flanke (Set oder Reset, unabhängig von welcher Quelle) bestimmt den Zustand, nicht eine kontinuierliche ODER-Verknüpfung.
- **Zwei unabhängige Last-Wins-Quellen**: VT-Button und OPC-UA-Kommando liefern je eigenständig Set-/Reset-Ereignisse; `ASR_MERGE_2` führt beide zu einem gemeinsamen Latch-Eingang zusammen, ohne eine Quelle zu priorisieren.
- **Schnittstellenkompatibles Außeninterface**: Die Schnittstelle ist identisch zu `Button_IXA_TO_logiBUS_QXA_BG_OPC`, sodass beide Varianten austauschbar instanziiert werden können. Das Schaltverhalten ist aber nur bei einer einzelnen aktiven Quelle gleich — bei überlappender Nutzung beider Quellen unterscheidet es sich (Last-Wins vs. echte ODER-Verknüpfung).

## Anwendungsszenarien

- Ausgänge mit zwei unabhängigen Schaltquellen (z. B. lokale VT-Taste UND Remote-OPC-UA-Kommando), bei denen die jeweils zuletzt eingetroffene Aktion (Drücken oder Loslassen, von welcher Quelle auch immer) gewinnen soll, statt dass eine dauerhaft aktive Quelle die andere blockiert — genau das würde eine echte ODER-Verknüpfung tun.

## Vergleich mit ähnlichen Bausteinen

Gegenüber [`Button_IXA_TO_logiBUS_QXA_BG_OPC`](./Button_IXA_TO_logiBUS_QXA_BG_OPC.md) (aktuell: tastendes `AX_OR_2`) ersetzt dieser Baustein die einfache ODER-Verknüpfung durch eine Flankenerkennungs-/Merge-/Latch-Kette (`AX_ASR_RF_TRIG` × 2, `ASR_MERGE_2`, `ASR_AX_SR`). Bei nur einer aktiven Quelle ist das Verhalten identisch (tastend); der Unterschied zeigt sich nur, wenn sich beide Quellen überlappen (Last-Wins statt ODER).

## Zusammenfassung

`Button_IXA_TO_logiBUS_QXA_BG_OPC_LATCHING` bietet bei nur einer aktiven Quelle dieselbe tastende Grundfunktion wie `Button_IXA_TO_logiBUS_QXA_BG_OPC` — der Unterschied liegt im Last-Wins-Verhalten bei zeitlich überlappenden VT-/OPC-UA-Kommandos, nicht in einem Klick-Toggle. Beide Bausteine sind schnittstellenkompatibel, aber nicht in jedem Szenario verhaltensgleich.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
