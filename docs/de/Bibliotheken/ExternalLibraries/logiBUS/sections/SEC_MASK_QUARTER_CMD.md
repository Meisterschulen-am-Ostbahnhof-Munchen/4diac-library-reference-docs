# SEC_MASK_QUARTER_CMD

![SEC_MASK_QUARTER_CMD](./SEC_MASK_QUARTER_CMD.svg)

* * * * * * * * * *

## Einleitung

**SEC_MASK_QUARTER_CMD** sperrt Sections in einem Condensed Work State anhand einer BIT-Maske (1 = Section frei). Der Baustein ist für den Sollwert (Setpoint Condensed Work State) gedacht. Er ist ein Simple-FB mit `REQ`/`CNF`, `IN` (`ARRAY[0..15] OF DWORD`, 2 Bit je Section), `MASK` (`ARRAY[0..7] OF DWORD`, 1 Bit je Section) und `OUT` (`ARRAY[0..15] OF DWORD`).

## Schnittstellenstruktur

### **Ereignis-Eingänge**

| Name | Typ | Kommentar | Zugeordnete Daten |
| ---- | --- | --------- | ----------------- |
| REQ | Event | Maske anwenden | IN, MASK |

### **Ereignis-Ausgänge**

| Name | Typ | Kommentar | Zugeordnete Daten |
| ---- | --- | --------- | ----------------- |
| CNF | Event | Ergebnis berechnet | OUT |

### **Daten-Eingänge**

| Name | Typ | Kommentar |
| ---- | --- | --------- |
| IN | DWORD[16] | Condensed Work State, 2 Bit je Section, Wort k = Sections 16k+1..16k+16 |
| MASK | DWORD[8] | BIT-Maske, 1 = Section frei, Wort k = Sections 32k+1..32k+32 |

### **Daten-Ausgänge**

| Name | Typ | Kommentar |
| ---- | --- | --------- |
| OUT | DWORD[16] | Ergebnis nach Anwendung der Maske, gleiche Anordnung wie `IN` |

### **Adapter**

Keine.

## Funktionsweise

Gesperrte Sections werden immer `00` (aus), auch wenn der Eingang `10` oder `11` enthält. Freie Sections bleiben unverändert.

| Eingang (Section gesperrt) | Ausgang |
| -------------------------- | ------- |
| `00` | `00` |
| `01` | `00` |
| `10` | `00` |
| `11` | `00` |

Section s liegt im Condensed Work State in Wort `(s-1) DIV 16`, Bits `2*((s-1) MOD 16)+1..2*((s-1) MOD 16)`, in der BIT-Maske in Wort `(s-1) DIV 32`, Bit `(s-1) MOD 32`.

## Technische Besonderheiten

- Beim Sollwert bedeutet `11` „keine Änderung“. Würde eine gesperrte Section `11` durchreichen, bliebe sie nach einem früheren `01` an. Deshalb wird sie auf `00` gezwungen.
- Es gibt zwei Masken-Bausteine, weil `11` beim Sollwert und beim Istwert Verschiedenes bedeutet. Siehe [Condensed Work State](https://meisterschulen-am-ostbahnhof-munchen-docs.readthedocs.io/projects/isobus-other-docs/de/latest/Condensed-Work-State/) und [`SEC_MASK_QUARTER_ACT`](SEC_MASK_QUARTER_ACT.md).
- Die Maske kommt typischerweise von einem BIT-Switcher, zum Beispiel [`SEC_SWITCH_LR_BIT`](SEC_SWITCH_LR_BIT.md).
- Die Schleife läuft über alle 256 Sections (16 Wörter).

## Fazit

`SEC_MASK_QUARTER_CMD` wendet eine BIT-Maske auf einen Sollwert-Condensed-Work-State an und erhält dabei die Bedeutung der 2-Bit-Werte.
