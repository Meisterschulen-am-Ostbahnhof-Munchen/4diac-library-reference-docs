# ASSEMBLE_LWORD_FROM_DWORDS

![ASSEMBLE_LWORD_FROM_DWORDS](ASSEMBLE_LWORD_FROM_DWORDS.svg)

* * * * * * * * * *

## Einleitung

Die Funktion `ASSEMBLE_LWORD_FROM_DWORDS` kombiniert zwei 32-Bit-Doppelwörter (`DWORD`) zu einem 64-Bit-Langwort (`LWORD`).

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- `REQ`: Startet die Verarbeitung. Verbundene Daten: `DWORD_00`, `DWORD_01`

### **Ereignis-Ausgänge**

- `CNF`: Signalisiert die Verarbeitung und die Ausgabe des kombinierten `LWORD`.

### **Daten-Eingänge**

- `DWORD_00`: Doppelwort 0 (`DWORD`), unterste 32 Bit des Ergebnisses
- `DWORD_01`: Doppelwort 1 (`DWORD`), oberste 32 Bit (Bits 32 bis 63)

### **Daten-Ausgänge**

- (kein Name): Das kombinierte 64-Bit-Langwort (`LWORD`), zugleich der Rückgabewert der Funktion

### **Adapter**

Diese Funktion verwendet keine Adapter.

## Funktionsweise

Die Funktion weist die beiden Eingabewerte über den Teilzugriff `.%D0` und `.%D1` dem Rückgabewert zu:

```text
ASSEMBLE_LWORD_FROM_DWORDS.%D0 := DWORD_00;
ASSEMBLE_LWORD_FROM_DWORDS.%D1 := DWORD_01;
```

`DWORD_00` bildet damit das niederwertigste und `DWORD_01` das höchstwertige Doppelwort des `LWORD`.

## Technische Besonderheiten

- Implementierung in Structured Text (ST) als `FUNCTION` ohne internen Zustand
- Teil des Pakets `eclipse4diac::utils::assembling`
- Die Umkehrfunktion ist [`SPLIT_LWORD_INTO_DWORDS`](../splitting/SPLIT_LWORD_INTO_DWORDS.md)
- Version 1.0 (2026-05-16)

## Anwendungsszenarien

- Zusammensetzen eines 64-Bit-Werts aus zwei 32-Bit-Registern (z. B. Fieldbus-Daten)
- Erzeugen von 64-Bit-Bitmustern aus zwei Doppelwörtern

## Fazit

`ASSEMBLE_LWORD_FROM_DWORDS` setzt zwei `DWORD`-Werte, beginnend mit dem niederwertigsten, zu einem `LWORD` zusammen.
