# AUI2_I_TO_UINT

![AUI2_I_TO_UINT_network](./AUI2_I_TO_UINT_network.svg)

* * * * * * * * * *

## Einleitung

`AUI2_I_TO_UINT` ist ein Composite-FB, der zwischen einem `UINT`-Datenpaar (`OUT`/`IN`) und dem bidirektionalen Adapter `AUI2` vermittelt. Im Gegensatz zur Plug-Variante `AUI2_UINT_TO_R` besitzt er einen Adapter-**Socket** (Eingang). Er vervollständigt die Konvertierungsfamilie analog zu `AR2`/`AB2`/`AE2`.

![AUI2_I_TO_UINT](AUI2_I_TO_UINT.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- `REQ`: Anforderung an den Adapter. Verbundene Daten: `OUT`.

### **Ereignis-Ausgänge**

- `CNF`: Bestätigung vom Adapter. Verbundene Daten: `IN`.

### **Daten-Eingänge**

- `OUT` (`UINT`): Wert, der zum Adapter gesendet wird.

### **Daten-Ausgänge**

- `IN` (`UINT`): Vom Adapter empfangener Wert.

### **Adapter**

| Richtung | Name | Adaptertyp | Kommentar |
| :--- | :--- | :--- | :--- |
| **Socket** (Eingang) | `AUI2_IN` | `adapter::types::bidirectional::AUI2` | Adapter-Eingang. |

## Funktionsweise

Der Baustein enthält keine internen FBs, sondern nur Verbindungen:

- `REQ` → `AUI2_IN.EI1` und `OUT` → `AUI2_IN.DI1`: Ein `REQ`-Ereignis sendet den Wert `OUT` über den Adapter.
- `AUI2_IN.EO1` → `CNF` und `AUI2_IN.DO1` → `IN`: Ein vom Adapter kommendes Ereignis wird als `CNF` ausgegeben, der empfangene Wert erscheint an `IN`.

## Technische Besonderheiten

- Reine Durchreichung ohne Logik und ohne internen Zustand.
- Der Datentyp ist `UINT` (vorzeichenlose 16-Bit-Ganzzahl).
- Die Benennung `OUT`/`IN` ist aus Sicht des Adapters gewählt: `OUT` geht zum Adapter hinaus, `IN` kommt vom Adapter herein.

## Anwendungsszenarien

- Einen `UINT`-Wert auf einen bidirektionalen `AUI2`-Adapter ausgeben und gleichzeitig dessen Rückkanal als Datenausgang auslesen, wenn der Adapter als Socket vorliegt.

## Zusammenfassung

`AUI2_I_TO_UINT` verbindet Ereignisse und `UINT`-Daten 1:1 mit dem bidirektionalen Adapter `AUI2` (Socket-Variante).
