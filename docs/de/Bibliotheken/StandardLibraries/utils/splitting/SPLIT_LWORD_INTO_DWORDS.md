# SPLIT_LWORD_INTO_DWORDS

![SPLIT_LWORD_INTO_DWORDS](SPLIT_LWORD_INTO_DWORDS.svg)

* * * * * * * * * *

## Einleitung

Die Funktion `SPLIT_LWORD_INTO_DWORDS` extrahiert die zwei 32-Bit-Doppelwörter (`DWORD`) aus einem 64-Bit-Langwort (`LWORD`).

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- `REQ`: Startet die Verarbeitung. Verbundene Daten: `IN`

### **Ereignis-Ausgänge**

- `CNF`: Signalisiert die Verarbeitung. Verbundene Daten: `DWORD_00`, `DWORD_01`

### **Daten-Eingänge**

- `IN` (`LWORD`): Zu zerlegendes Langwort

### **Daten-Ausgänge**

- `DWORD_00` (`DWORD`): Niederwertiges Doppelwort (Bits 0 bis 31)
- `DWORD_01` (`DWORD`): Höherwertiges Doppelwort (Bits 32 bis 63)

### **Adapter**

Diese Funktion verwendet keine Adapter.

## Funktionsweise

Die Funktion weist die Teile über den Teilzugriff zu:

```text
DWORD_00 := IN.%D0;
DWORD_01 := IN.%D1;
```

## Technische Besonderheiten

- Implementierung in Structured Text (ST) als `FUNCTION` ohne internen Zustand
- Teil des Pakets `eclipse4diac::utils::splitting`
- Die Umkehrung bildet `ASSEMBLE_LWORD_FROM_WORDS` bzw. die `ASSEMBLE_*`-Funktionen im Paket `assembling` (z. B. [`ASSEMBLE_LWORD_FROM_WORDS`](../assembling/ASSEMBLE_LWORD_FROM_WORDS.md) für `WORD`-Teile).

## Anwendungsszenarien

- Zerlegen eines 64-Bit-Werts in zwei 32-Bit-Werte, z. B. für Fieldbus- oder Registerzugriffe

## Fazit

`SPLIT_LWORD_INTO_DWORDS` teilt ein `LWORD` in sein niederwertiges und sein höherwertiges `DWORD`.
