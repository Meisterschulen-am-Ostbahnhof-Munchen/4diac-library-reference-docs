# F_STRING_TO_BYTES_TERM

![F_STRING_TO_BYTES_TERM](./F_STRING_TO_BYTES_TERM.svg)

* * * * * * * * * *

## Einleitung

Die Funktion `F_STRING_TO_BYTES_TERM` (Version 1.0) kopiert die Zeichen eines `STRING` in ein `BYTE`-Array und schließt den Text mit `16#00` ab, wenn Platz ist. Sie verhält sich wie `F_STRING_TO_BYTES` bis Version 1.1. Sie ist für Strings über CAN gedacht (`AlPgnTxNew_TP`, `DlPgnSend`), damit `F_BYTES_TO_STRING` beim Empfänger das Textende findet. Für Dateien ohne Textende gibt es [F_STRING_TO_BYTES](F_STRING_TO_BYTES.md). Sie gehört zum Package `logiBUS::utils::dyn_arr` der Bibliothek `logiBUS_utils` 3.0.0.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

-   **REQ** (Typ: `Event`): Startet das Kopieren (mit Daten: `IN`, `DATA`)

### **Ereignis-Ausgänge**

-   **CNF** (Typ: `Event`): Kopieren beendet (mit Daten: Rückgabewert, `DATA`)

### **Daten-Eingänge**

-   **IN** (Typ: `STRING`): Text, je Zeichen 1 Byte
-   **DATA** (Typ: `ARRAY[*] OF BYTE`, IN/OUT): Zielpuffer, zum Beispiel von `PROVIDE_ARR_xxxx_BYTE`. Der Puffer wird ganz beschrieben.

### **Daten-Ausgänge**

-   **Rückgabewert** (Typ: `UINT`): Zeichen + 1 (das `16#00`), höchstens die Größe von `DATA`

## Funktionsweise

1. Die Zeichen von `IN` werden in `DATA` kopiert.
2. Dahinter folgt `16#00` als Textende, falls Platz ist.
3. Alle restlichen Bytes sind `16#FF`.

Das Gegenstück ist [F_BYTES_TO_STRING](F_BYTES_TO_STRING.md): Es liest `SIZE` Bytes und hört beim ersten `16#00` auf.

Hinweis: Die Seite wurde aus der Typdefinition (`F_STRING_TO_BYTES_TERM.fct`, Stand 2026-11-26) erzeugt. Nicht auf Hardware getestet.
