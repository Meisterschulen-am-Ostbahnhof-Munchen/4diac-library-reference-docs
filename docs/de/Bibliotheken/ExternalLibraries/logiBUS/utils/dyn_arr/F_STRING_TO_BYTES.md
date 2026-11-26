# F_STRING_TO_BYTES

![F_STRING_TO_BYTES](./F_STRING_TO_BYTES.svg)

* * * * * * * * * *

## Einleitung

Die Funktion `F_STRING_TO_BYTES` (Version 2.0) kopiert die Zeichen eines `STRING` in ein `BYTE`-Array. Es werden nur die Zeichen kopiert, **ohne** Textende `16#00`. Das passt für Dateien, zum Beispiel mit `FsWrite` (Übung 156). Soll der Text mit `16#00` abgeschlossen werden, etwa für Strings über CAN, gibt es [F_STRING_TO_BYTES_TERM](F_STRING_TO_BYTES_TERM.md). Sie gehört zum Package `logiBUS::utils::dyn_arr` der Bibliothek `logiBUS_utils` 3.0.0.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

-   **REQ** (Typ: `Event`): Startet das Kopieren (mit Daten: `IN`, `DATA`)

### **Ereignis-Ausgänge**

-   **CNF** (Typ: `Event`): Kopieren beendet (mit Daten: Rückgabewert, `DATA`)

### **Daten-Eingänge**

-   **IN** (Typ: `STRING`): Text, je Zeichen 1 Byte
-   **DATA** (Typ: `ARRAY[*] OF BYTE`, IN/OUT): Zielpuffer, zum Beispiel von `PROVIDE_ARR_xxxx_BYTE`. Der Puffer wird ganz beschrieben.

### **Daten-Ausgänge**

-   **Rückgabewert** (Typ: `UINT`): Anzahl der Zeichen, höchstens die Größe von `DATA`

## Funktionsweise

1. Die Länge von `IN` wird auf die Größe von `DATA` begrenzt.
2. Alle Bytes von `DATA` werden mit `16#FF` gefüllt (nicht genutzte Bytes, wie bei J1939/ISOBUS üblich).
3. Die Zeichen von `IN` werden ab dem ersten Index von `DATA` hineinkopiert.
4. Zurückgegeben wird die Anzahl der kopierten Zeichen.

## Änderung gegenüber Version 1.1

Bis Version 1.1 stand hinter den Zeichen ein `16#00` (falls Platz war). Version 2.0 schreibt kein `16#00` mehr, die Rückgabe ist die reine Zeichenzahl. Das alte Verhalten bietet jetzt `F_STRING_TO_BYTES_TERM`.

**Kompatibilität:** Alte Programme, die `F_STRING_TO_BYTES` für Strings über CAN nutzen, senden mit der neuen ELF kein `16#00` mehr. Der Empfänger findet mit `F_BYTES_TO_STRING` dann kein Textende. Solche Programme müssen auf `F_STRING_TO_BYTES_TERM` umgestellt werden. Die Übungen 138, 138b, 142, 144, 147, 149 und 149d nutzen bereits `F_STRING_TO_BYTES_TERM`.

Hinweis: Die Seite wurde aus der Typdefinition (`F_STRING_TO_BYTES.fct`, Stand 2026-11-26) erzeugt. Nicht auf Hardware getestet.
