# ADI_GT_0

![ADI_GT_0_network](./ADI_GT_0_network.svg)

* * * * * * * * * *

## Einleitung

`ADI_GT_0` prüft, ob ein DINT-Adapter-Wert größer als 0 ist, und gibt das Ergebnis als boolesches AX-Adapter-Signal aus. Typischer Eingangswert ist ein vorzeichenbehafteter 32-Bit-Wert. Der Baustein ist für jeden Zahlentyp gleich aufgebaut (`AS`, `AI`, `ADI`, `ALI`, `AUS`, `AUI`, `AUDI`, `AULI`, `AR`, `ALR`).

![ADI_GT_0](ADI_GT_0.svg)

## Schnittstelle

- **Ereignis-Eingang**: `INIT` (Initialisierung)
- **Ereignis-Ausgang**: `INITO` (Initialisierung abgeschlossen, kommt vom internen `initval_ADI`)
- **Sockel**: `IN` (`ADI`) — der Wert, der gegen 0 verglichen wird
- **Stecker**: `AX_OUT` (`AX`) — TRUE, wenn `IN > 0`

## Verwendete Funktionsbausteine (FBs)

### Sub-Bausteine: ADI_GT_0

- **Typ**: SubAppType
- **Verwendete interne FBs**:
    - **initval_ADI**: `adapter::types::unidirectional::ADI::initval::initval_ADI` — liefert die feste Vergleichskonstante `DINT#0` als Adapter-Wert (`INIT_VAL=DINT#0`). Ein Adapter-Sockel kann die Konstante nicht als Parameter bekommen, deshalb wird sie über diesen Baustein eingespeist.
    - **ADI_GT**: `adapter::iec61131::comparison::ADI_GT` — adapter-nativer Vergleich `IN1 > IN2`.
- **Funktionsweise**: `IN` → `ADI_GT.IN1`; Konstante 0 → `ADI_GT.IN2`; Ergebnis → `AX_OUT`.

## Programmablauf und Verbindungen

1. `INIT` → `initval_ADI.INIT`; `initval_ADI.INITO` → `INITO`.
2. `IN` (Adapter) → `ADI_GT.IN1`.
3. `initval_ADI.OUT` (Konstante `DINT#0`) → `ADI_GT.IN2`.
4. `ADI_GT.OUT` → `AX_OUT` (Adapter).

## Anwendungsszenarien

- Umwandlung von ein vorzeichenbehafteter 32-Bit-Wert in ein einfaches boolesches Adapter-Signal für VT-Statusanzeigen oder Freigabelogik.

## Zusammenfassung

`ADI_GT_0` ist ein kompakter Wrapper um den adapter-nativen Vergleich `ADI_GT` — wandelt „DINT > 0“ direkt in ein AX-Adapter-Signal um, ohne dass der Aufrufer die Null-Konstante selbst anlegen und verdrahten muss.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
