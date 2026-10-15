# AR_GT_0

![AR_GT_0_network](./AR_GT_0_network.svg)

* * * * * * * * * *

## Einleitung

`AR_GT_0` prüft, ob ein REAL-Adapter-Wert größer als 0 ist, und gibt das Ergebnis als boolesches AX-Adapter-Signal aus. Typischer Eingangswert ist ein Gleitkommawert (REAL), z. B. eine Geschwindigkeit. Der Baustein ist für jeden Zahlentyp gleich aufgebaut (`AS`, `AI`, `ADI`, `ALI`, `AUS`, `AUI`, `AUDI`, `AULI`, `AR`, `ALR`).

![AR_GT_0](AR_GT_0.svg)

## Schnittstelle

- **Ereignis-Eingang**: `INIT` (Initialisierung)
- **Ereignis-Ausgang**: `INITO` (Initialisierung abgeschlossen, kommt vom internen `LIT_0`)
- **Sockel**: `IN` (`AR`) — der Wert, der gegen 0 verglichen wird
- **Stecker**: `AX_OUT` (`AX`) — TRUE, wenn `IN > 0`

## Verwendete Funktionsbausteine (FBs)

### Sub-Bausteine: AR_GT_0

- **Typ**: SubAppType
- **Verwendete interne FBs**:
    - **LIT_0**: `adapter::types::unidirectional::AR::initval::initval_AR` — liefert die feste Vergleichskonstante `0.0` als Adapter-Wert (`INIT_VAL=0.0`). Ein Adapter-Sockel kann die Konstante nicht als Parameter bekommen, deshalb wird sie über diesen Baustein eingespeist.
    - **AR_GT**: `adapter::iec61131::comparison::AR_GT` — adapter-nativer Vergleich `IN1 > IN2`.
- **Funktionsweise**: `IN` → `AR_GT.IN1`; Konstante 0 → `AR_GT.IN2`; Ergebnis → `AX_OUT`.

## Programmablauf und Verbindungen

1. `INIT` → `LIT_0.INIT`; `LIT_0.INITO` → `INITO`.
2. `IN` (Adapter) → `AR_GT.IN1`.
3. `LIT_0.OUT` (Konstante `0.0`) → `AR_GT.IN2`.
4. `AR_GT.OUT` → `AX_OUT` (Adapter).

## Anwendungsszenarien

- Umwandlung von ein Gleitkommawert (REAL), z. B. eine Geschwindigkeit in ein einfaches boolesches Adapter-Signal für VT-Statusanzeigen oder Freigabelogik.

## Zusammenfassung

`AR_GT_0` ist ein kompakter Wrapper um den adapter-nativen Vergleich `AR_GT` — wandelt „REAL > 0“ direkt in ein AX-Adapter-Signal um, ohne dass der Aufrufer die Null-Konstante selbst anlegen und verdrahten muss.

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
