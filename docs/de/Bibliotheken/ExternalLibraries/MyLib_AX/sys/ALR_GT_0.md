# ALR_GT_0

![ALR_GT_0_network](./ALR_GT_0_network.svg)

* * * * * * * * * *

## Einleitung

`ALR_GT_0` prüft, ob ein LREAL-Adapter-Wert größer als 0 ist, und gibt das Ergebnis als boolesches AX-Adapter-Signal aus. Typischer Eingangswert ist ein Gleitkommawert (LREAL). Der Baustein ist für jeden Zahlentyp gleich aufgebaut (`AS`, `AI`, `ADI`, `ALI`, `AUS`, `AUI`, `AUDI`, `AULI`, `AR`, `ALR`).

![ALR_GT_0](ALR_GT_0.svg)

## Schnittstelle

- **Ereignis-Eingang**: `INIT` (Initialisierung)
- **Ereignis-Ausgang**: `INITO` (Initialisierung abgeschlossen, kommt vom internen `initval_ALR`)
- **Sockel**: `IN` (`ALR`) — der Wert, der gegen 0 verglichen wird
- **Stecker**: `AX_OUT` (`AX`) — TRUE, wenn `IN > 0`

## Verwendete Funktionsbausteine (FBs)

### Sub-Bausteine: ALR_GT_0

- **Typ**: SubAppType
- **Verwendete interne FBs**:
    - **initval_ALR**: `adapter::types::unidirectional::ALR::initval::initval_ALR` — liefert die feste Vergleichskonstante `LREAL#0.0` als Adapter-Wert (`INIT_VAL=LREAL#0.0`). Ein Adapter-Sockel kann die Konstante nicht als Parameter bekommen, deshalb wird sie über diesen Baustein eingespeist.
    - **ALR_GT**: `adapter::iec61131::comparison::ALR_GT` — adapter-nativer Vergleich `IN1 > IN2`.
- **Funktionsweise**: `IN` → `ALR_GT.IN1`; Konstante 0 → `ALR_GT.IN2`; Ergebnis → `AX_OUT`.

## Programmablauf und Verbindungen

1. `INIT` → `initval_ALR.INIT`; `initval_ALR.INITO` → `INITO`.
2. `IN` (Adapter) → `ALR_GT.IN1`.
3. `initval_ALR.OUT` (Konstante `LREAL#0.0`) → `ALR_GT.IN2`.
4. `ALR_GT.OUT` → `AX_OUT` (Adapter).

## Anwendungsszenarien

- Umwandlung von ein Gleitkommawert (LREAL) in ein einfaches boolesches Adapter-Signal für VT-Statusanzeigen oder Freigabelogik.

## Zusammenfassung

`ALR_GT_0` ist ein kompakter Wrapper um den adapter-nativen Vergleich `ALR_GT` — wandelt „LREAL > 0“ direkt in ein AX-Adapter-Signal um, ohne dass der Aufrufer die Null-Konstante selbst anlegen und verdrahten muss.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
