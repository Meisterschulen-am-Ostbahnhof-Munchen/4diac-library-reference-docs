# sections

## Einleitung

Das Package `logiBUS::sections` gehört zu zwei Bibliotheken ([PR #347](https://github.com/Meisterschulen-am-Ostbahnhof-Munchen/4diac_training1/pull/347)), die beide nur von `core` abhängen und deshalb auch ohne ISOBUS nutzbar sind:

- `logiBUS_sections_bit`: 1 Bit je Section, 1 = frei.
- `logiBUS_sections_quarter`: 2 Bit je Section (Condensed Work State).

Anlass war der Section-Switcher einer Getreidehacke: Vier Tasten schalten Teilbreiten von links bzw. rechts ab und wieder zu. Section 1 ist links und liegt in den niederwertigsten Bits.

## Bibliothek `logiBUS_sections_bit`

| Baustein | `MASK` | Sections |
| -------- | ------ | -------- |
| [`SEC_SWITCH_LR_BIT`](SEC_SWITCH_LR_BIT.md) | `ARRAY[0..7] OF DWORD` | 256 |
| [`SEC_SWITCH_LR_BIT_WORD`](SEC_SWITCH_LR_BIT_WORD.md) | `WORD` | 16 |
| [`SEC_SWITCH_LR_BIT_DWORD`](SEC_SWITCH_LR_BIT_DWORD.md) | `DWORD` | 32 |
| [`SEC_SWITCH_LR_BIT_LWORD`](SEC_SWITCH_LR_BIT_LWORD.md) | `LWORD` | 64 |

## Bibliothek `logiBUS_sections_quarter`

| Baustein | `MASK` | Sections |
| -------- | ------ | -------- |
| [`SEC_SWITCH_LR_QUARTER`](SEC_SWITCH_LR_QUARTER.md) | `ARRAY[0..15] OF DWORD` | 256 |
| [`SEC_SWITCH_LR_QUARTER_WORD`](SEC_SWITCH_LR_QUARTER_WORD.md) | `WORD` | 8 |
| [`SEC_SWITCH_LR_QUARTER_DWORD`](SEC_SWITCH_LR_QUARTER_DWORD.md) | `DWORD` | 16 |
| [`SEC_SWITCH_LR_QUARTER_LWORD`](SEC_SWITCH_LR_QUARTER_LWORD.md) | `LWORD` | 32 |

Dazu kommen:

| Baustein | Aufgabe |
| -------- | ------- |
| [`SEC_MASK_QUARTER_CMD`](SEC_MASK_QUARTER_CMD.md) | Sollwert mit BIT-Maske sperren: gesperrt immer `00` |
| [`SEC_MASK_QUARTER_ACT`](SEC_MASK_QUARTER_ACT.md) | Istwert mit BIT-Maske sperren: nur `01` wird `00`, `10` und `11` bleiben |
| [`SEC_QUARTER`](SEC_QUARTER.md) | Konstanten `QUARTER_OFF`, `QUARTER_ON`, `QUARTER_ERROR`, `QUARTER_NA` |

Das Wort k des QUARTER-Switchers entspricht dem Wort k des TC-DDI.

## Gemeinsames Verhalten der Switcher

| Ereignis | Wirkung |
| -------- | ------- |
| `INIT+` | `N` übernehmen, Zähler 0, `INITO+`. Im aktiven Zustand neu initialisieren. |
| `INIT-` | deaktiviert, Maske alle frei, `INITO-` (auch vor `INIT+`) |
| `REQ+` / `REQ-` | Maske erneut ausgeben, `CNF+` / nichts ändern, `CNF-` |
| `LEFT_OFF_INC/DEC`, `RIGHT_OFF_INC/DEC` | eine Section von links bzw. rechts ab- oder zuschalten, `CNF+`. An der Grenze keine Reaktion. |
| `ALL_ON` / `ALL_OFF` | alle frei / alle gesperrt (`LEFT_OFF` = `N`), `CNF+` |

- `N` und `QI` sind per `WITH` nur an `INIT` verbunden, `QI` zusätzlich an `REQ`.
- Nach `ALL_OFF` schalten beide Seiten symmetrisch zu (seit Version 1.1, PR #349): `LEFT_OFF_DEC` beginnt bei der rechten Section, `RIGHT_OFF_DEC` bei der linken.
- Die Service-Sequenzen (17 je Switcher) sind in den Typseiten als Sequenzdiagramme dargestellt.

## Zwei Masken-Bausteine

`11` bedeutet beim Sollwert „keine Änderung“, beim Istwert „nicht installiert“ (ISO 11783-11, DDI 290 bzw. 161). Würde eine gesperrte Section beim Sollwert `11` durchreichen, bliebe sie nach einem früheren `01` an. Deshalb gibt es `SEC_MASK_QUARTER_CMD` und `SEC_MASK_QUARTER_ACT`. Hintergrund und Verknüpfungsregeln: [Condensed Work State](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/isobus-other-docs/de/latest/Condensed-Work-State/).

## Stand

- Die Bibliotheken sind in `develop` von `4diac_training1`.
- Sie sind noch nicht in FORTE getestet und haben noch kein Export-/ELF-Modul (Stand der PR-Beschreibung).
