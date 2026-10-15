# AUI_GT_0

![AUI_GT_0_network](./AUI_GT_0_network.svg)

* * * * * * * * * *

## Einleitung

`AUI_GT_0` prüft, ob ein UINT-Adapter-Wert größer als 0 ist, und gibt das Ergebnis als boolesches AX-Adapter-Signal aus. Typischer Eingangswert ist ein Objekt-ID- oder Zählerwert (z. B. „wie viele Kanäle sind aktiv“). Der Baustein ist für jeden Zahlentyp gleich aufgebaut (`AS`, `AI`, `ADI`, `ALI`, `AUS`, `AUI`, `AUDI`, `AULI`, `AR`, `ALR`).

![AUI_GT_0](AUI_GT_0.svg)

## Schnittstelle

- **Ereignis-Eingang**: `INIT` (Initialisierung)
- **Ereignis-Ausgang**: `INITO` (Initialisierung abgeschlossen, kommt vom internen `initval_AUI`)
- **Sockel**: `IN` (`AUI`) — der Wert, der gegen 0 verglichen wird
- **Stecker**: `AX_OUT` (`AX`) — TRUE, wenn `IN > 0`

## Verwendete Funktionsbausteine (FBs)

### Sub-Bausteine: AUI_GT_0

- **Typ**: SubAppType
- **Verwendete interne FBs**:
    - **initval_AUI**: `adapter::types::unidirectional::AUI::initval::initval_AUI` — liefert die feste Vergleichskonstante `UINT#0` als Adapter-Wert (`INIT_VAL=UINT#0`). Ein Adapter-Sockel kann die Konstante nicht als Parameter bekommen, deshalb wird sie über diesen Baustein eingespeist.
    - **AUI_GT**: `adapter::iec61131::comparison::AUI_GT` — adapter-nativer Vergleich `IN1 > IN2`.
- **Funktionsweise**: `IN` → `AUI_GT.IN1`; Konstante 0 → `AUI_GT.IN2`; Ergebnis → `AX_OUT`.

## Programmablauf und Verbindungen

1. `INIT` → `initval_AUI.INIT`; `initval_AUI.INITO` → `INITO`.
2. `IN` (Adapter) → `AUI_GT.IN1`.
3. `initval_AUI.OUT` (Konstante `UINT#0`) → `AUI_GT.IN2`.
4. `AUI_GT.OUT` → `AX_OUT` (Adapter).

## Anwendungsszenarien

- Umwandlung von ein Objekt-ID- oder Zählerwert (z. B. „wie viele Kanäle sind aktiv“) in ein einfaches boolesches Adapter-Signal für VT-Statusanzeigen oder Freigabelogik.

## Zusammenfassung

`AUI_GT_0` ist ein kompakter Wrapper um den adapter-nativen Vergleich `AUI_GT` — wandelt „UINT > 0“ direkt in ein AX-Adapter-Signal um, ohne dass der Aufrufer die Null-Konstante selbst anlegen und verdrahten muss.

## 🛠️ Zugehörige Übungen

- [Uebung_071_AUI](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_071_AUI/)
- [Uebung_071a_AUI](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/4diac-exercises-docs/de/latest/Uebungen/test_AX/Uebungen_doc/Uebung_071a_AUI/)

---

### 🌐 Passende Themen-Unterseiten auf ms-muc-docs.de

- [🌐 Eclipse 4diac IDE & Farb-Referenz auf ms-muc-docs.de](https://www.ms-muc-docs.de/iec-61499/eclipse-4diac/)
