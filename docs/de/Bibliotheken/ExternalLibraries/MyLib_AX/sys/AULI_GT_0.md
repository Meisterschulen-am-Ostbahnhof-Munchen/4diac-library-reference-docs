# AULI_GT_0

![AULI_GT_0_network](./AULI_GT_0_network.svg)

* * * * * * * * * *

## Einleitung

`AULI_GT_0` prüft, ob ein ULINT-Adapter-Wert größer als 0 ist, und gibt das Ergebnis als boolesches AX-Adapter-Signal aus. Typischer Eingangswert ist ein vorzeichenloser 64-Bit-Zählerwert. Der Baustein ist für jeden Zahlentyp gleich aufgebaut (`AS`, `AI`, `ADI`, `ALI`, `AUS`, `AUI`, `AUDI`, `AULI`, `AR`, `ALR`).

![AULI_GT_0](AULI_GT_0.svg)

## Schnittstelle

- **Ereignis-Eingang**: `INIT` (Initialisierung)
- **Ereignis-Ausgang**: `INITO` (Initialisierung abgeschlossen, kommt vom internen `initval_AULI`)
- **Sockel**: `IN` (`AULI`) — der Wert, der gegen 0 verglichen wird
- **Stecker**: `AX_OUT` (`AX`) — TRUE, wenn `IN > 0`

## Verwendete Funktionsbausteine (FBs)

### Sub-Bausteine: AULI_GT_0

- **Typ**: SubAppType
- **Verwendete interne FBs**:
    - **initval_AULI**: `adapter::types::unidirectional::AULI::initval::initval_AULI` — liefert die feste Vergleichskonstante `ULINT#0` als Adapter-Wert (`INIT_VAL=ULINT#0`). Ein Adapter-Sockel kann die Konstante nicht als Parameter bekommen, deshalb wird sie über diesen Baustein eingespeist.
    - **AULI_GT**: `adapter::iec61131::comparison::AULI_GT` — adapter-nativer Vergleich `IN1 > IN2`.
- **Funktionsweise**: `IN` → `AULI_GT.IN1`; Konstante 0 → `AULI_GT.IN2`; Ergebnis → `AX_OUT`.

## Programmablauf und Verbindungen

1. `INIT` → `initval_AULI.INIT`; `initval_AULI.INITO` → `INITO`.
2. `IN` (Adapter) → `AULI_GT.IN1`.
3. `initval_AULI.OUT` (Konstante `ULINT#0`) → `AULI_GT.IN2`.
4. `AULI_GT.OUT` → `AX_OUT` (Adapter).

## Anwendungsszenarien

- Umwandlung von ein vorzeichenloser 64-Bit-Zählerwert in ein einfaches boolesches Adapter-Signal für VT-Statusanzeigen oder Freigabelogik.

## Zusammenfassung

`AULI_GT_0` ist ein kompakter Wrapper um den adapter-nativen Vergleich `AULI_GT` — wandelt „ULINT > 0“ direkt in ein AX-Adapter-Signal um, ohne dass der Aufrufer die Null-Konstante selbst anlegen und verdrahten muss.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
