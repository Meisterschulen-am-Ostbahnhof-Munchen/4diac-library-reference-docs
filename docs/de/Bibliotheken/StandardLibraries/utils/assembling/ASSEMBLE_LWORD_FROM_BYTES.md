# ASSEMBLE_LWORD_FROM_BYTES

![ASSEMBLE_LWORD_FROM_BYTES](ASSEMBLE_LWORD_FROM_BYTES.svg)

* * * * * * * * * *

## Einleitung

Die Funktion `ASSEMBLE_LWORD_FROM_BYTES` kombiniert acht BYTE-Werte zu einem einzigen LWORD-Wert. Sie ist nützlich, wenn Daten aus mehreren Byte-Quellen (z. B. Protokollfelder) zu einer 64-Bit-Einheit zusammengesetzt werden sollen.

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **REQ**: Startet die Kombination der BYTE-Werte zu einem LWORD. Verknüpft mit `BYTE_00` bis `BYTE_07`.

### **Ereignis-Ausgänge**

- **CNF**: Signalisiert die abgeschlossene Kombination. Der LWORD-Wert liegt am Daten-Ausgang an.

### **Daten-Eingänge**

- **BYTE_00**: Niedrigstwertiges Byte (B0) des LWORD.
- **BYTE_01**: Byte B1.
- **BYTE_02**: Byte B2.
- **BYTE_03**: Byte B3.
- **BYTE_04**: Byte B4.
- **BYTE_05**: Byte B5.
- **BYTE_06**: Byte B6.
- **BYTE_07**: Höchstwertiges Byte (B7) des LWORD.

### **Daten-Ausgänge**

- **(unbenannt)**: Der zusammengesetzte LWORD-Wert (Rückgabewert der Funktion).

### **Adapter**

Keine Adapter vorhanden.

## Funktionsweise

Die Funktion weist jedes Eingangs-Byte direkt dem entsprechenden Byte des Rückgabewerts zu:

- `BYTE_00` → `%B0` (niedrigstwertig)
- `BYTE_01` → `%B1`
- `BYTE_02` → `%B2`
- `BYTE_03` → `%B3`
- `BYTE_04` → `%B4`
- `BYTE_05` → `%B5`
- `BYTE_06` → `%B6`
- `BYTE_07` → `%B7` (höchstwertig)

## Technische Besonderheiten

- Implementiert als Funktion (`FUNCTION ... : LWORD`) in ST (Structured Text), Paket `eclipse4diac::utils::assembling`.
- Der Zugriff auf einzelne Bytes erfolgt über die partielle Zugriffssyntax `%B0` bis `%B7`.
- Keine eigene Zustandshaltung.

## Anwendungsszenarien

- Zusammensetzen von 64-Bit-Werten aus Einzelbytes, z. B. bei Protokollimplementierungen oder CAN-Daten.

## ⚖️ Vergleich mit ähnlichen Bausteinen

- [ASSEMBLE_DWORD_FROM_BYTES](./ASSEMBLE_DWORD_FROM_BYTES.md) setzt vier Bytes zu einem DWORD zusammen, [ASSEMBLE_LWORD_FROM_WORDS](./ASSEMBLE_LWORD_FROM_WORDS.md) vier Worte zu einem LWORD.

## Fazit

`ASSEMBLE_LWORD_FROM_BYTES` setzt acht BYTE-Werte byteweise zu einem LWORD zusammen.
