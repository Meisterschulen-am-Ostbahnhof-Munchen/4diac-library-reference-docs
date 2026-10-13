# AOption_AD_DEMUX_4

![AOption_AD_DEMUX_4](./AOption_AD_DEMUX_4.svg)

* * * * * * * * * *

## Einleitung

`AOption_AD_DEMUX_4` ist ein Bitmasken-Demultiplexer (natives Generic-FB `GEN_AOption_AD_DEMUX`, kein ECC und kein Algorithmus in diesem Typ) für `N=4` Options-Ausgänge vom Typ [`Option::AOption`](../AOption.md). Über den bidirektionalen Bitmasken-Adapter `K` (`adapter::types::bidirectional::AD2`, DWORD) werden beliebig viele der 4 Ausgänge gleichzeitig aktiviert (Multi-Select).

## Schnittstellenstruktur

### **Ereignis-Eingänge / -Ausgänge**

Keine direkten Ereignisanschlüsse; die Ereignisse laufen über die Adapter.

### **Daten-Eingänge / -Ausgänge**

Keine direkten Datenanschlüsse; die Daten laufen über die Adapter.

### **Adapter**

| Adapter | Typ | Richtung | Kommentar |
|---------|-----|----------|-----------|
| `OUT1` | `Option::AOption` | Plug | Option 1, aktiv wenn Bit 0 von `K` gesetzt ist (Wert 1). |
| `OUT2` | `Option::AOption` | Plug | Option 2, aktiv wenn Bit 1 von `K` gesetzt ist (Wert 2). |
| `OUT3` | `Option::AOption` | Plug | Option 3, aktiv wenn Bit 2 von `K` gesetzt ist (Wert 4). |
| `OUT4` | `Option::AOption` | Plug | Option 4, aktiv wenn Bit 3 von `K` gesetzt ist (Wert 8). |
| `K` | `adapter::types::bidirectional::AD2` | Socket | Bitmaske (Bit0=OUT1, Bit1=OUT2, ...), bidirektional: `EO1`/`DO1` = neuer Bitmasken-Wunsch rein, `EI1`/`DI1` = Quittung mit der übernommenen Bitmaske zurück. |

## Funktionsweise

**Bitzuordnung (Multi-Select):**

- `Bit 0` (Wert `1`): `OUT1`
- `Bit 1` (Wert `2`): `OUT2`
- `Bit 2` (Wert `4`): `OUT3`
- `Bit 3` (Wert `8`): `OUT4`

Beispiel: `K = 3` (Bit 0 und Bit 1 gesetzt) aktiviert `OUT1` und `OUT2`; alle anderen Ausgänge bleiben aus. Es können beliebig viele bzw. alle Ausgänge gleichzeitig aktiv sein.

**Umschaltverhalten:** Nur Bits, die sich gegenüber dem vorherigen `K`-Wert ändern, lösen am zugehörigen Ausgang ein `INIT_REQ` aus (0->1: `QI=TRUE`, aktivieren; 1->0: `QI=FALSE`, deaktivieren). Unveränderte Bits werden nicht erneut getriggert; jeder Ausgang wird pro eintreffendem `K`-Wert höchstens einmal getriggert.

**Busy-Gating über den K-Adapter:** Solange noch auf `INIT_CNF` von mindestens einem durch den letzten `K`-Wechsel betroffenen Ausgang gewartet wird, wird jeder weitere über `K` (Plug -> Socket, `EO1`/`DO1`) eintreffende Bitmasken-Wunsch ignoriert; er wird weder gepuffert noch nachgeholt. Sobald alle ausgelösten `INIT_CNF` eingetroffen sind, meldet der Baustein dies rückwärts über `K` (Socket -> Plug, `EI1`/`DI1`) mit der übernommenen Bitmaske zurück (nicht mit dem Zustand der Ausgänge; auch Bits außerhalb der vorhandenen Ausgänge werden unverändert quittiert). Erst danach werden wieder neue `K`-Wünsche angenommen.

## Technische Besonderheiten

- **Multi-Select** im Gegensatz zum Single-Select von `AOption_AUI_DEMUX_4`.
- **Natives Generic-FB**: Das Verhalten ist in der Laufzeitumgebung implementiert, nicht als ECC oder Algorithmus.
- **Quittierung**: `K` wird erst nach Eintreffen aller `INIT_CNF` zurückgemeldet; zwischenzeitliche Wünsche gehen verloren.

## Anwendungsszenarien

- Gleichzeitiges Zu- und Abschalten mehrerer optionaler Funktionsgruppen (z. B. [`OptionalGroup_001`](../../MyLib_AX_Option/sys/OptionalGroup_001.md)) über eine Bitmaske.

## Fazit

`AOption_AD_DEMUX_4` schaltet per Bitmaske `K` beliebige Kombinationen von 4 Optionen und meldet die übernommene Bitmaske erst nach Abschluss aller Aktivierungen zurück.
