# AOption_AD_DEMUX_7

![AOption_AD_DEMUX_7](AOption_AD_DEMUX_7.svg)

* * * * * * * * * *

## Einleitung

Der Funktionsblock AOption_AD_DEMUX_7 ist ein Demultiplexer (natives Generic-FB, kein ECC und kein Algorithmus in diesem Typ) für 7 Options-Ausgange vom Typ `Option::AOption`. Die Auswahl erfolgt über eine **Bitmaske** am bidirektionalen Adapter `K` (`adapter::types::bidirectional::AD2`, DWORD). Es handelt sich um eine Multi-Select-Variante: beliebig viele Ausgänge können gleichzeitig aktiv sein.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

Der Funktionsblock verfügt über keine direkten Ereignis-Eingänge – die Kommunikation läuft ausschließlich über die Adapter.

### **Ereignis-Ausgänge**

Der Funktionsblock verfügt über keine direkten Ereignis-Ausgänge.

### **Daten-Eingänge**

Der Funktionsblock verfügt über keine direkten Daten-Eingänge.

### **Daten-Ausgänge**

Der Funktionsblock verfügt über keine direkten Daten-Ausgänge.

### **Adapter**

- **K** (Socket, `adapter::types::bidirectional::AD2`): Bitmaske (Bit 0 = OUT1, Bit 1 = OUT2, ...). Bidirektional: `EO1`/`DO1` = neuer Bitmasken-Wunsch kommt herein, `EI1`/`DI1` = Quittung mit der übernommenen Bitmaske geht zurück
- **OUT1** (Plug, `Option::AOption`): Option 1, aktiv wenn Bit 0 von K gesetzt ist (Wert 1)
- **OUT2** (Plug, `Option::AOption`): Option 2, aktiv wenn Bit 1 von K gesetzt ist (Wert 2)
- **OUT3** (Plug, `Option::AOption`): Option 3, aktiv wenn Bit 2 von K gesetzt ist (Wert 4)
- **OUT4** (Plug, `Option::AOption`): Option 4, aktiv wenn Bit 3 von K gesetzt ist (Wert 8)
- **OUT5** (Plug, `Option::AOption`): Option 5, aktiv wenn Bit 4 von K gesetzt ist (Wert 16)
- **OUT6** (Plug, `Option::AOption`): Option 6, aktiv wenn Bit 5 von K gesetzt ist (Wert 32)
- **OUT7** (Plug, `Option::AOption`): Option 7, aktiv wenn Bit 6 von K gesetzt ist (Wert 64)

Jeder Ausgang `OUTx` verwendet den Adapter `Option::AOption`: das Ereignis `INIT_REQ` (zusammen mit `QI_OUT`) läuft in Richtung Plug → Socket, die Bestätigung `INIT_CNF` in Gegenrichtung.

## Funktionsweise

**Bitzuordnung (Multi-Select):**

- Bit 0 (Wert 1) – `OUT1`
- Bit 1 (Wert 2) – `OUT2`
- Bit 2 (Wert 4) – `OUT3`
- Bit 3 (Wert 8) – `OUT4`
- Bit 4 (Wert 16) – `OUT5`
- Bit 5 (Wert 32) – `OUT6`
- Bit 6 (Wert 64) – `OUT7`

Beispiel: `K = 3` (Bit 0 und Bit 1 gesetzt) aktiviert `OUT1` und `OUT2`, alle anderen Ausgänge sind aus. Es können beliebig viele bzw. alle Ausgänge gleichzeitig aktiv sein.

**Umschaltverhalten:** Nur Bits, die sich gegenüber dem vorherigen `K`-Wert ändern, lösen am zugehörigen Ausgang ein `INIT_REQ` aus (0→1: `QI=TRUE`, aktivieren; 1→0: `QI=FALSE`, deaktivieren). Unveränderte Bits werden nicht erneut getriggert – jeder Ausgang wird pro eintreffendem `K`-Wert maximal einmal getriggert.

**Busy-Gating über den bidirektionalen K-Adapter:** Solange noch auf `INIT_CNF` von mindestens einem durch den letzten K-Wechsel betroffenen Ausgang gewartet wird, wird jeder weitere über `K` (Richtung Plug → Socket, `EO1`/`DO1`) eintreffende Bitmasken-Wunsch ignoriert – er wird weder gepuffert noch nachgeholt. Sind alle ausgelösten `INIT_CNF` eingetroffen, meldet der Baustein dies rückwärts über `K` (Richtung Socket → Plug, `EI1`/`DI1`) mit der übernommenen Bitmaske zurück (nicht mit dem Zustand der Ausgänge: auch Bits außerhalb der vorhandenen Ausgänge werden unverändert quittiert). Erst danach werden wieder neue K-Wünsche angenommen.

## ⚖️ Vergleich mit ähnlichen Bausteinen

[AOption_AUI_DEMUX_7](AOption_AUI_DEMUX_7.md) ist die Single-Select-Variante mit Index (`AUI2`) statt Bitmaske: dort ist höchstens ein Ausgang gleichzeitig aktiv.
