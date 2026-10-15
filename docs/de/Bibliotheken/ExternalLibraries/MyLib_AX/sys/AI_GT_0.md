# AI_GT_0

![AI_GT_0_network](./AI_GT_0_network.svg)

* * * * * * * * * *

## Einleitung

`AI_GT_0` prüft, ob ein INT-Adapter-Wert größer als 0 ist, und gibt das Ergebnis als boolesches AX-Adapter-Signal aus. Typischer Eingangswert ist ein vorzeichenbehafteter 16-Bit-Wert. Der Baustein ist für jeden Zahlentyp gleich aufgebaut (`AS`, `AI`, `ADI`, `ALI`, `AUS`, `AUI`, `AUDI`, `AULI`, `AR`, `ALR`).

![AI_GT_0](AI_GT_0.svg)

## Schnittstelle

- **Ereignis-Eingang**: `INIT` (Initialisierung)
- **Ereignis-Ausgang**: `INITO` (Initialisierung abgeschlossen, kommt vom internen `initval_AI`)
- **Sockel**: `IN` (`AI`) — der Wert, der gegen 0 verglichen wird
- **Stecker**: `AX_OUT` (`AX`) — TRUE, wenn `IN > 0`

## Verwendete Funktionsbausteine (FBs)

### Sub-Bausteine: AI_GT_0

- **Typ**: SubAppType
- **Verwendete interne FBs**:
    - **initval_AI**: `adapter::types::unidirectional::AI::initval::initval_AI` — liefert die feste Vergleichskonstante `INT#0` als Adapter-Wert (`INIT_VAL=INT#0`). Ein Adapter-Sockel kann die Konstante nicht als Parameter bekommen, deshalb wird sie über diesen Baustein eingespeist.
    - **AI_GT**: `adapter::iec61131::comparison::AI_GT` — adapter-nativer Vergleich `IN1 > IN2`.
- **Funktionsweise**: `IN` → `AI_GT.IN1`; Konstante 0 → `AI_GT.IN2`; Ergebnis → `AX_OUT`.

## Programmablauf und Verbindungen

1. `INIT` → `initval_AI.INIT`; `initval_AI.INITO` → `INITO`.
2. `IN` (Adapter) → `AI_GT.IN1`.
3. `initval_AI.OUT` (Konstante `INT#0`) → `AI_GT.IN2`.
4. `AI_GT.OUT` → `AX_OUT` (Adapter).

## Anwendungsszenarien

- Umwandlung von ein vorzeichenbehafteter 16-Bit-Wert in ein einfaches boolesches Adapter-Signal für VT-Statusanzeigen oder Freigabelogik.

## Zusammenfassung

`AI_GT_0` ist ein kompakter Wrapper um den adapter-nativen Vergleich `AI_GT` — wandelt „INT > 0“ direkt in ein AX-Adapter-Signal um, ohne dass der Aufrufer die Null-Konstante selbst anlegen und verdrahten muss.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
