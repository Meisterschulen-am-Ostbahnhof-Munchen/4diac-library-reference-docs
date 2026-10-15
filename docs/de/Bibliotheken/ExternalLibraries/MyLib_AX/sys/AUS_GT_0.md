# AUS_GT_0

![AUS_GT_0_network](./AUS_GT_0_network.svg)

* * * * * * * * * *

## Einleitung

`AUS_GT_0` prüft, ob ein USINT-Adapter-Wert größer als 0 ist, und gibt das Ergebnis als boolesches AX-Adapter-Signal aus. Typischer Eingangswert ist ein vorzeichenloser 8-Bit-Wert. Der Baustein ist für jeden Zahlentyp gleich aufgebaut (`AS`, `AI`, `ADI`, `ALI`, `AUS`, `AUI`, `AUDI`, `AULI`, `AR`, `ALR`).

![AUS_GT_0](AUS_GT_0.svg)

## Schnittstelle

- **Ereignis-Eingang**: `INIT` (Initialisierung)
- **Ereignis-Ausgang**: `INITO` (Initialisierung abgeschlossen, kommt vom internen `initval_AUS`)
- **Sockel**: `IN` (`AUS`) — der Wert, der gegen 0 verglichen wird
- **Stecker**: `AX_OUT` (`AX`) — TRUE, wenn `IN > 0`

## Verwendete Funktionsbausteine (FBs)

### Sub-Bausteine: AUS_GT_0

- **Typ**: SubAppType
- **Verwendete interne FBs**:
    - **initval_AUS**: `adapter::types::unidirectional::AUS::initval::initval_AUS` — liefert die feste Vergleichskonstante `USINT#0` als Adapter-Wert (`INIT_VAL=USINT#0`). Ein Adapter-Sockel kann die Konstante nicht als Parameter bekommen, deshalb wird sie über diesen Baustein eingespeist.
    - **AUS_GT**: `adapter::iec61131::comparison::AUS_GT` — adapter-nativer Vergleich `IN1 > IN2`.
- **Funktionsweise**: `IN` → `AUS_GT.IN1`; Konstante 0 → `AUS_GT.IN2`; Ergebnis → `AX_OUT`.

## Programmablauf und Verbindungen

1. `INIT` → `initval_AUS.INIT`; `initval_AUS.INITO` → `INITO`.
2. `IN` (Adapter) → `AUS_GT.IN1`.
3. `initval_AUS.OUT` (Konstante `USINT#0`) → `AUS_GT.IN2`.
4. `AUS_GT.OUT` → `AX_OUT` (Adapter).

## Anwendungsszenarien

- Umwandlung von ein vorzeichenloser 8-Bit-Wert in ein einfaches boolesches Adapter-Signal für VT-Statusanzeigen oder Freigabelogik.

## Zusammenfassung

`AUS_GT_0` ist ein kompakter Wrapper um den adapter-nativen Vergleich `AUS_GT` — wandelt „USINT > 0“ direkt in ein AX-Adapter-Signal um, ohne dass der Aufrufer die Null-Konstante selbst anlegen und verdrahten muss.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
