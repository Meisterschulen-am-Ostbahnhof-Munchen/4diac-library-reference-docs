# AS_GT_0

![AS_GT_0_network](./AS_GT_0_network.svg)

* * * * * * * * * *

## Einleitung

`AS_GT_0` prüft, ob ein SINT-Adapter-Wert größer als 0 ist, und gibt das Ergebnis als boolesches AX-Adapter-Signal aus. Typischer Eingangswert ist ein vorzeichenbehafteter 8-Bit-Wert. Der Baustein ist für jeden Zahlentyp gleich aufgebaut (`AS`, `AI`, `ADI`, `ALI`, `AUS`, `AUI`, `AUDI`, `AULI`, `AR`, `ALR`).

![AS_GT_0](AS_GT_0.svg)

## Schnittstelle

- **Ereignis-Eingang**: `INIT` (Initialisierung)
- **Ereignis-Ausgang**: `INITO` (Initialisierung abgeschlossen, kommt vom internen `initval_AS`)
- **Sockel**: `IN` (`AS`) — der Wert, der gegen 0 verglichen wird
- **Stecker**: `AX_OUT` (`AX`) — TRUE, wenn `IN > 0`

## Verwendete Funktionsbausteine (FBs)

### Sub-Bausteine: AS_GT_0

- **Typ**: SubAppType
- **Verwendete interne FBs**:
    - **initval_AS**: `adapter::types::unidirectional::AS::initval::initval_AS` — liefert die feste Vergleichskonstante `SINT#0` als Adapter-Wert (`INIT_VAL=SINT#0`). Ein Adapter-Sockel kann die Konstante nicht als Parameter bekommen, deshalb wird sie über diesen Baustein eingespeist.
    - **AS_GT**: `adapter::iec61131::comparison::AS_GT` — adapter-nativer Vergleich `IN1 > IN2`.
- **Funktionsweise**: `IN` → `AS_GT.IN1`; Konstante 0 → `AS_GT.IN2`; Ergebnis → `AX_OUT`.

## Programmablauf und Verbindungen

1. `INIT` → `initval_AS.INIT`; `initval_AS.INITO` → `INITO`.
2. `IN` (Adapter) → `AS_GT.IN1`.
3. `initval_AS.OUT` (Konstante `SINT#0`) → `AS_GT.IN2`.
4. `AS_GT.OUT` → `AX_OUT` (Adapter).

## Anwendungsszenarien

- Umwandlung von ein vorzeichenbehafteter 8-Bit-Wert in ein einfaches boolesches Adapter-Signal für VT-Statusanzeigen oder Freigabelogik.

## Zusammenfassung

`AS_GT_0` ist ein kompakter Wrapper um den adapter-nativen Vergleich `AS_GT` — wandelt „SINT > 0“ direkt in ein AX-Adapter-Signal um, ohne dass der Aufrufer die Null-Konstante selbst anlegen und verdrahten muss.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
