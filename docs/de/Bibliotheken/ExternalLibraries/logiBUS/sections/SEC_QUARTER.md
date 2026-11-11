# SEC_QUARTER

* * * * * * * * * *

## Einleitung

**SEC_QUARTER** ist eine Konstantenliste (`.gcf`, Package `logiBUS::sections`) mit den vier Werten einer Section im Condensed Work State, 2 Bit je Section. Die Konstanten sind vom Typ `BYTE`.

## Konstanten

| Name | Wert | Bedeutung |
| ---- | ---- | --------- |
| `QUARTER_OFF` | `2#00` | aus |
| `QUARTER_ON` | `2#01` | an |
| `QUARTER_ERROR` | `2#10` | Fehler |
| `QUARTER_NA` | `2#11` | nicht verfügbar |

## Technische Besonderheiten

- Die Bedeutung von `2#10` und `2#11` hängt davon ab, ob ein Ist-Wert oder ein Sollwert gemeint ist. Siehe [Condensed Work State](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/isobus-other-docs/de/latest/Condensed-Work-State/). Die Konstanten benennen nur die Bitmuster.
- Der Switcher [`SEC_SWITCH_LR_QUARTER`](SEC_SWITCH_LR_QUARTER.md) benutzt sie für seine Maske: frei `QUARTER_ON`, gesperrt `QUARTER_OFF`, Sections oberhalb von `N` `QUARTER_NA`.

## Fazit

`SEC_QUARTER` gibt den vier 2-Bit-Werten lesbare Namen.
