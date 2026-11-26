# F_BYTES_TO_STRING

![F_BYTES_TO_STRING](./F_BYTES_TO_STRING.svg)

* * * * * * * * * *

## Einleitung

Die Funktion `F_BYTES_TO_STRING` (Version 1.1) liest die ersten `SIZE` Bytes eines `BYTE`-Arrays als `STRING`. Sie ist das Gegenstück zu [F_STRING_TO_BYTES_TERM](F_STRING_TO_BYTES_TERM.md) und [F_STRING_TO_BYTES](F_STRING_TO_BYTES.md). Sie gehört zum Package `logiBUS::utils::dyn_arr` der Bibliothek `logiBUS_utils` 3.0.0.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

-   **REQ** (Typ: `Event`): Startet das Auslesen (mit Daten: `SIZE`, `STOP_AT_NUL`, `DATA`)

### **Ereignis-Ausgänge**

-   **CNF** (Typ: `Event`): Auslesen beendet (mit Daten: Rückgabewert, `DATA`)

### **Daten-Eingänge**

-   **SIZE** (Typ: `UINT`): Anzahl gültiger Bytes (zum Beispiel `u16DaSize`), höchstens die Größe von `DATA`
-   **STOP_AT_NUL** (Typ: `BOOL`): TRUE: Der Text endet vor dem ersten `16#00` (Vorgabewert: `TRUE`)
-   **DATA** (Typ: `ARRAY[*] OF BYTE`, IN/OUT): Quellpuffer, zum Beispiel von `PROVIDE_ARR_xxxx_BYTE`

### **Daten-Ausgänge**

-   **Rückgabewert** (Typ: `STRING`): Text, je Byte 1 Zeichen

## Funktionsweise

Das Array wird bis `SIZE` gelesen. Bei `STOP_AT_NUL = TRUE` endet der Text vor dem ersten `16#00`. Daten, die mit `F_STRING_TO_BYTES` (ohne `16#00`) geschrieben wurden, sollten mit passendem `SIZE` gelesen werden, weil dort kein Textende steht.

Hinweis: Die Seite wurde aus der Typdefinition (`F_BYTES_TO_STRING.fct`, Stand 2026-11-26) erzeugt. Nicht auf Hardware getestet.
