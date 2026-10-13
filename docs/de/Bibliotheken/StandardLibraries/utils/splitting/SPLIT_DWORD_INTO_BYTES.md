# SPLIT_DWORD_INTO_BYTES

![SPLIT_DWORD_INTO_BYTES](SPLIT_DWORD_INTO_BYTES.svg)

* * * * * * * * * *

## Einleitung

Die Funktion `SPLIT_DWORD_INTO_BYTES` zerlegt einen `DWORD`-Wert in seine vier `BYTE`-Anteile. Sie ist das Gegenstück zu [`ASSEMBLE_DWORD_FROM_BYTES`](../assembling/ASSEMBLE_DWORD_FROM_BYTES.md).

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- **REQ**: Startet die Zerlegung. Verbundene Daten: `IN`.

### **Ereignis-Ausgänge**

- **CNF**: Signalisiert die abgeschlossene Zerlegung. Verbundene Daten: `BYTE_00` bis `BYTE_03`.

### **Daten-Eingänge**

- **IN** (`DWORD`): Zu zerlegender Wert

### **Daten-Ausgänge**

- **BYTE_00** (`BYTE`): niedrigstwertiges Byte (`%B0`)
- **BYTE_01** (`BYTE`): Byte `%B1`
- **BYTE_02** (`BYTE`): Byte `%B2`
- **BYTE_03** (`BYTE`): höchstwertiges Byte (`%B3`)

### **Adapter**

Keine Adapter vorhanden.

## Funktionsweise

Die Funktion weist jedem Ausgang das entsprechende Byte des Eingangs zu:

- `BYTE_00 := IN.%B0`
- `BYTE_01 := IN.%B1`
- `BYTE_02 := IN.%B2`
- `BYTE_03 := IN.%B3`

## Technische Besonderheiten

- Implementiert als ST-Funktion (`FUNCTION`) mit `VAR_OUTPUT`, Paket `eclipse4diac::utils::splitting`
- Der Zugriff auf einzelne Bytes erfolgt über die partielle Zugriffssyntax `%B0` bis `%B3`
- Keine eigene Zustandshaltung

## Anwendungsszenarien

- Zerlegen von 32-Bit-Werten in Einzelbytes, z. B. für Protokolle oder CAN-Daten

## ⚖️ Vergleich mit ähnlichen Bausteinen

- [`SPLIT_WORD_INTO_BYTES`](./SPLIT_WORD_INTO_BYTES.md) zerlegt ein `WORD` in zwei Bytes, [`SPLIT_DWORD_INTO_WORDS`](./SPLIT_DWORD_INTO_WORDS.md) ein `DWORD` in zwei Worte.

## Fazit

`SPLIT_DWORD_INTO_BYTES` zerlegt einen `DWORD` byteweise in vier `BYTE`-Ausgänge.
