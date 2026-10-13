# AS2_S_TO_SINT

![AS2_S_TO_SINT_network](./AS2_S_TO_SINT_network.svg)

* * * * * * * * * *

## Einleitung

`AS2_S_TO_SINT` ist ein Composite-FB, der zwischen einem `SINT`-Datenpaar (`OUT`/`IN`) und dem bidirektionalen Adapter `AS2` vermittelt. Im Gegensatz zur Plug-Variante `AS2_SINT_TO_R` besitzt er einen Adapter-**Socket** (Eingang). Er vervollständigt die Konvertierungsfamilie analog zu `AR2`/`AB2`/`AE2`.

![AS2_S_TO_SINT](AS2_S_TO_SINT.svg)

## Schnittstellenstruktur

### **Ereignis-Eingänge**

- `REQ`: Anforderung an den Adapter. Verbundene Daten: `OUT`.

### **Ereignis-Ausgänge**

- `CNF`: Bestätigung vom Adapter. Verbundene Daten: `IN`.

### **Daten-Eingänge**

- `OUT` (`SINT`): Wert, der zum Adapter gesendet wird.

### **Daten-Ausgänge**

- `IN` (`SINT`): Vom Adapter empfangener Wert.

### **Adapter**

| Richtung | Name | Adaptertyp | Kommentar |
| :--- | :--- | :--- | :--- |
| **Socket** (Eingang) | `AS2_IN` | `adapter::types::bidirectional::AS2` | Adapter-Eingang. |

## Funktionsweise

Der Baustein enthält keine internen FBs, sondern nur Verbindungen:

- `REQ` → `AS2_IN.EI1` und `OUT` → `AS2_IN.DI1`: Ein `REQ`-Ereignis sendet den Wert `OUT` über den Adapter.
- `AS2_IN.EO1` → `CNF` und `AS2_IN.DO1` → `IN`: Ein vom Adapter kommendes Ereignis wird als `CNF` ausgegeben, der empfangene Wert erscheint an `IN`.

## Technische Besonderheiten

- Reine Durchreichung ohne Logik und ohne internen Zustand.
- Der Datentyp ist `SINT` (vorzeichenbehaftete 8-Bit-Ganzzahl).
- Die Benennung `OUT`/`IN` ist aus Sicht des Adapters gewählt: `OUT` geht zum Adapter hinaus, `IN` kommt vom Adapter herein.

## Anwendungsszenarien

- Einen `SINT`-Wert auf einen bidirektionalen `AS2`-Adapter ausgeben und gleichzeitig dessen Rückkanal als Datenausgang auslesen, wenn der Adapter als Socket vorliegt.

## Zusammenfassung

`AS2_S_TO_SINT` verbindet Ereignisse und `SINT`-Daten 1:1 mit dem bidirektionalen Adapter `AS2` (Socket-Variante).
