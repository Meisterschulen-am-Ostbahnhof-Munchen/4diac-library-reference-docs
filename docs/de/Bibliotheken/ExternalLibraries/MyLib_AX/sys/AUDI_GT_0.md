# AUDI_GT_0

![AUDI_GT_0_network](./AUDI_GT_0_network.svg)

* * * * * * * * * *

## Einleitung

`AUDI_GT_0` prüft, ob ein UDINT-Adapter-Wert größer als 0 ist, und gibt das Ergebnis als boolesches AX-Adapter-Signal aus. Typischer Eingangswert ist ein vorzeichenloser 32-Bit-Zählerwert. Der Baustein ist für jeden Zahlentyp gleich aufgebaut (`AS`, `AI`, `ADI`, `ALI`, `AUS`, `AUI`, `AUDI`, `AULI`, `AR`, `ALR`).

![AUDI_GT_0](AUDI_GT_0.svg)

## Schnittstelle

- **Ereignis-Eingang**: `INIT` (Initialisierung)
- **Ereignis-Ausgang**: `INITO` (Initialisierung abgeschlossen, kommt vom internen `initval_AUDI`)
- **Sockel**: `IN` (`AUDI`) — der Wert, der gegen 0 verglichen wird
- **Stecker**: `AX_OUT` (`AX`) — TRUE, wenn `IN > 0`

## Verwendete Funktionsbausteine (FBs)

### Sub-Bausteine: AUDI_GT_0

- **Typ**: SubAppType
- **Verwendete interne FBs**:
    - **initval_AUDI**: `adapter::types::unidirectional::AUDI::initval::initval_AUDI` — liefert die feste Vergleichskonstante `UDINT#0` als Adapter-Wert (`INIT_VAL=UDINT#0`). Ein Adapter-Sockel kann die Konstante nicht als Parameter bekommen, deshalb wird sie über diesen Baustein eingespeist.
    - **AUDI_GT**: `adapter::iec61131::comparison::AUDI_GT` — adapter-nativer Vergleich `IN1 > IN2`.
- **Funktionsweise**: `IN` → `AUDI_GT.IN1`; Konstante 0 → `AUDI_GT.IN2`; Ergebnis → `AX_OUT`.

## Programmablauf und Verbindungen

1. `INIT` → `initval_AUDI.INIT`; `initval_AUDI.INITO` → `INITO`.
2. `IN` (Adapter) → `AUDI_GT.IN1`.
3. `initval_AUDI.OUT` (Konstante `UDINT#0`) → `AUDI_GT.IN2`.
4. `AUDI_GT.OUT` → `AX_OUT` (Adapter).

## Anwendungsszenarien

- Umwandlung von ein vorzeichenloser 32-Bit-Zählerwert in ein einfaches boolesches Adapter-Signal für VT-Statusanzeigen oder Freigabelogik.

## Zusammenfassung

`AUDI_GT_0` ist ein kompakter Wrapper um den adapter-nativen Vergleich `AUDI_GT` — wandelt „UDINT > 0“ direkt in ein AX-Adapter-Signal um, ohne dass der Aufrufer die Null-Konstante selbst anlegen und verdrahten muss.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
